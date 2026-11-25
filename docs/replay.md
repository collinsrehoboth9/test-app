# Replay the path of a tester (experimental)

Two runs never take the same path. So when a tester finds a bug, the path that led to it is worth keeping.

Each Android tester now leaves two more files in its results, next to `report.md`:

- `trace.json`: what the agent did, step by step. A tap is stored with the name of the element that was under the finger, not with its coordinates, so the path still replays when the layout moves. It also names the model that explored, when the run says it.
- `trace.params.json`: every text the agent typed (an e-mail, a password). The trace only holds their names, `TEXT_1`, `TEXT_2`. **Do not commit this file.**

Nothing changes for the agent: same tools, same brief. The taps are noted by a pass-through in front of `adb` ([scripts/android/shim/adb](../scripts/android/shim/adb)), and [scripts/ecr_trace.py](../scripts/ecr_trace.py) writes the trace when the agent is done. An agent often taps several times after one screenshot: the pass-through then saves the screen tree again just before the tap (`tree-N.xml`), so each tap is named after what was under the finger at that moment. It costs a second or two per tap.

To see these paths on a map of the app, comment each screen and add checks before compiling: [docs/dashboard.md](dashboard.md).

## From a trace to a test that reruns with no model

The trace is in the format of [Explore-Compile-Replay](https://github.com/pierridotite/explore-compile-replay), which compiles it to a [Maestro](https://maestro.dev) flow:

```bash
bash scripts/collect.sh <run id>                       # decrypts the results into runs/<run id>/
git clone https://github.com/pierridotite/explore-compile-replay && cd explore-compile-replay && pnpm install
pnpm ecr compile ../runs/<run id>/anna/trace.json --adapter packages/adapter-maestro/src/index.ts
maestro test -e TEXT_1="..." ../runs/<run id>/anna/anna-<date>.flow.yaml
```

## Replay it right after the run

With `-f replay_path=true`, each Android tester's path is replayed on the same phone as soon as the agent is done, with no model:

```bash
gh workflow run agent-testers.yml -f android_personas=anna -f ios_personas=none -f replay_path=true
```

The run page says how it went in one line, for example `replay: pass, 22 steps with no model in 41 s` or `replay: stopped at step 7 of 22 (target_not_found, tapOn by name, after tapOn by name, eraseText, inputText), after 38 s`: the step, why, its kind and the kinds of the three steps before it. It never prints a text of your app. Three more files join the encrypted results: the compiled flow (`anna-<date>.flow.yaml`), `replay.json` (the status, the step where it stopped and why) and `replay.txt` (everything the tools printed), plus what Maestro kept in `replay-debug/`.

The replay runs with the model keys taken out of its environment ([scripts/ecr_replay.py](../scripts/ecr_replay.py)), and a replay that stops never fails the run. It adds a few minutes to each tester: installing Maestro, then the replay itself.

When a replay stops, the app may have shown something else the second time (an account that now exists, a list that is no longer empty), or the trace missed something the agent did. Tell us which line you got in an issue: it shows what the trace still misses.

## What it is today

- **Experimental.** A path of 44 steps on Tiny Todo, the demo app, followed by a script with the tools of an agent, is written as a trace and replays from end to end on an Android 11 emulator with no model: the [Replay check](../.github/workflows/replay-check.yml) workflow, started by hand, free. One path of a real agent has replayed from end to end so far: the 37 steps of the map maker ([briefs/map.md](../briefs/map.md)), with no model, in 91 s. Before it, the path of a tester (72 steps) stopped at step 12, because the trace did not count elements that share a name the way Maestro does, and another one (62 steps) stopped at step 58, on a tap by name. Its trace is encrypted, so the cause is not known for sure. Two things the trace missed that day are now in it: a field the agent empties, and where a tap lands before a field is emptied. The check replays the path as it was once written, to show where each of these stops it. One path is not a rate: how often a tester's path replays is not measured yet.
- The agent's waits are not in the trace. Maestro waits for each element by itself, which may not be enough on a slow screen.
- **A path, with no check of its own.** The trace of an agent has no checks: replayed as it is, it only fails when a step no longer finds its target. The checks are added by a person in the [dashboard](dashboard.md), on a screen or from a finding of the report, where a finding becomes a check recorded as a known bug.
- **Android only.** iOS comes next.
- An element with no name and no id is stored as a point on the screen, in percent. It breaks on another screen size.
- When several elements share a name, the trace says which one was tapped, counted as Maestro counts them: only the deepest element of each (a button and its label are one), from the top of the screen, then from the left.
- A swipe keeps its direction and its two ends, in percent of the screen. The replay moves the finger between the same two points at Maestro's own speed, so a long list may still stop on another row.
- The agent's `type.py` empties a field from its end. Maestro erases before the cursor and has no way to move it. So the tap that opens a field before it is emptied is replayed where the text ends (`"at": "97%,90%"` of the field, its left end for a language written from right to left) and not on its centre. When the text is longer than the field, part of it may stay.
- A trace written since this change needs a clone of Explore-Compile-Replay that knows `at`: run `git pull` in an older one.
- A command the trace cannot hold (a key outside Back, Enter, Home and Backspace, a drag) is left out and counted on the `trace:` line of the run, with its kind.
- Typed texts stay out of the trace, also when the agent taps one: a tap on a task it just added is stored as `TEXT_1`. When the name only holds a typed text (its e-mail in a greeting), the tap is stored by the id of the element, or as a point.
