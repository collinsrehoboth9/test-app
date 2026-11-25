# mobile-agent-testers

<a href="https://github.com/pierridotite/explore-compile-replay"><img src="assets/ecr-now-building.svg" alt="Tiếp theo: mọi bug đều trở thành một test miễn phí. Explore-Compile-Replay" width="340" height="54"></a>
<a href="https://www.linkedin.com/in/pierre-raffalli/"><picture><source media="(prefers-color-scheme: dark)" srcset="assets/linkedin-follow-dark.svg"><img src="assets/linkedin-follow-light.svg" alt="Theo dõi Pierre Raffalli trên LinkedIn" width="160" height="54"></picture></a>
<a href="https://trendshift.io/repositories/371956?utm_source=trendshift-badge&amp;utm_medium=badge&amp;utm_campaign=badge-trendshift-371956" target="_blank" rel="noopener noreferrer"><img src="https://trendshift.io/api/badge/trendshift/repositories/371956/daily?language=Python" alt="pierridotite%2Fmobile-agent-testers | Trendshift" width="250" height="55"/></a>

<sub>Dự án tiếp theo của tôi: khi một agent tìm thấy một bug, <a href="https://github.com/pierridotite/explore-compile-replay">Explore-Compile-Replay</a> biến đường đi của nó thành một test chạy lại trên mọi PR, không cần model. Bản nháp đầu tiên.</sub>

**20 tester AI trên 20 chiếc điện thoại khác nhau, chạy song song, trên các runner miễn phí của GitHub.**

**[Ví dụ](docs/examples.vi.md)** · **[So sánh](docs/compare.md)** · **[FAQ](docs/faq.vi.md)** · **[Personas](personas.json)** · **[Đóng góp](CONTRIBUTING.md)** · **[Bản phát hành](https://github.com/pierridotite/mobile-agent-testers/releases)**

[English](README.md) · [简体中文](README.zh-CN.md) · [日本語](README.ja.md) · [Español](README.es.md) · [Português](README.pt-BR.md) · Tiếng Việt · [Türkçe](README.tr.md)

<sub>Bản dịch của README tiếng Anh: có thể chậm hơn bản gốc, bản chuẩn là [README.md](README.md).</sub>

Không cần viết test script. Mỗi tester là một agent Claude Code với iOS
Simulator hoặc Android emulator riêng trên GitHub Actions. Nó đóng vai một persona (iPhone SE
dùng tiếng Thụy Điển ở cỡ chữ lớn nhất, Pixel 7 ở 200%, iPad dùng tiếng Đức, tiếng Ả Rập
đọc từ phải sang trái...), dùng ứng dụng của bạn như một người dùng mới (hoặc chạy qua tính năng bạn
chỉ định) và viết một báo cáo bug kèm ảnh chụp màn hình. Flutter, React Native, Expo, Kotlin và Swift native, Kotlin
Multiplatform, Capacitor và .NET MAUI.

Runner của GitHub miễn phí cho các repo công khai. Bạn chỉ trả tiền cho model, bằng gói đăng ký
Claude hoặc API key của bạn: khoảng $0.15 đến $0.55 mỗi điện thoại trong [các lần chạy này](docs/runs/).

<p align="center"><a href="assets/localsend-20.mp4"><img src="assets/localsend-20.gif" alt="Bản ghi màn hình thật của 20 tester AI dùng LocalSend cùng lúc: 10 Android emulator và 10 iOS Simulator bằng 14 ngôn ngữ, từ một chiếc Pixel nhỏ đến một chiếc iPad 13 inch. Khi một tester chạy xong, điện thoại của nó hiển thị số vấn đề nó đã tìm thấy. Mở video." width="100%"></a><br>
<sub>Bản ghi màn hình thật của lần chạy LocalSend, đã cắt thời gian chờ. <a href="assets/localsend-20.mp4">Video đầy đủ (MP4)</a> · <a href="docs/runs/localsend-20.md">Lần chạy, theo từng thiết bị</a></sub></p>

[![Tests](https://github.com/pierridotite/mobile-agent-testers/actions/workflows/tests.yml/badge.svg)](https://github.com/pierridotite/mobile-agent-testers/actions/workflows/tests.yml)
[![Stars](https://img.shields.io/github/stars/pierridotite/mobile-agent-testers?style=flat)](https://github.com/pierridotite/mobile-agent-testers/stargazers)
[![License: MIT](https://img.shields.io/badge/license-MIT-green)](LICENSE)
![iOS + Android](https://img.shields.io/badge/platforms-iOS%20%2B%20Android-blue)
![Claude Code plugin](https://img.shields.io/badge/Claude%20Code-plugin-orange)
[![Last commit](https://img.shields.io/github/last-commit/pierridotite/mobile-agent-testers)](https://github.com/pierridotite/mobile-agent-testers/commits/main)
[![Discord](https://img.shields.io/badge/Discord-join-5865F2?logo=discord&logoColor=white)](https://discord.gg/WuaCANscr8)

## Nó làm được gì

Một mục đích cho mỗi lần chạy, đặt qua các input của [Kiểm thử ứng dụng của bạn](#kiểm-thử-ứng-dụng-của-bạn).

- **Khám phá như người dùng mới** (mặc định). Mỗi persona cố gắng đạt mục tiêu của mình và báo cáo những gì cản trở, kèm ảnh chụp màn hình được đánh số.
- **Chạy một tính năng từ đầu đến cuối.** Brief của riêng bạn đưa mọi agent qua một cơ chế hoàn chỉnh: một lượt đặt chỗ gồm nhiều bước, một trình soạn thảo có hoàn tác, một đồng bộ giữa hai màn hình. Agent chạm, nhấn giữ, vuốt và gõ, rời ứng dụng rồi quay lại, mở lại ứng dụng để kiểm tra những gì đã được lưu, hoặc mở một deep link để bắt đầu ngay tại tính năng, và một brief có thể nâng ngân sách khoảng 30 hành động. Brief của [AnkiDroid](briefs/ankidroid.md) đã tìm thấy trường Back của một thẻ mới bị bàn phím che khuất.
- **Tái hiện một báo cáo bug** trên mọi điện thoại: đưa một issue hoặc các bước, và mỗi agent trả lời có, không hoặc không rõ, kèm ảnh chụp màn hình ([briefs/reproduce.md](briefs/reproduce.md)).
- **Nhận một báo cáo cải tiến.** Mỗi persona dùng ứng dụng một lần, rồi nói nó đã do dự ở đâu, nó mong đợi điều gì và nên thay đổi gì trước, theo khu vực và mức tác động ([briefs/improve.md](briefs/improve.md)).
- **So sánh hai phiên bản.** Cùng các persona trên hai branch hoặc hai bản build, rồi một bảng: mục tiêu đạt được, số hành động, điểm số, các vấn đề chỉ thấy ở một phiên bản ([docs/compare-versions.md](docs/compare-versions.md)). Một bước kiểm tra trước một A/B test thật, không phải thứ thay thế cho nó.
- **Dùng một model rẻ hơn**: Claude Haiku, hoặc bất kỳ API tương thích Anthropic nào (OpenRouter, DeepSeek, Kimi, GLM, MiniMax, Qwen). Các phát hiện chỉ tốt bằng khả năng đọc ảnh chụp màn hình và suy luận của model ([docs/cheaper-models.md](docs/cheaper-models.md)).

Maestro hay Appium chạy các test bạn viết. Các agent này tìm những gì không ai nghĩ tới để test: [so sánh chi tiết](docs/compare.md).

## Ví dụ thực tế

Các lần chạy thật trên các ứng dụng mã nguồn mở, không gài sẵn gì cả.

| Ứng dụng | Điện thoại | Agent đã tìm thấy gì | Thời gian, mức dùng model |
|---|---|---|---|
| [LocalSend](https://github.com/localsend/localsend) (Flutter, 100k stars) | 20: 10 Android, 10 iOS, 14 ngôn ngữ | 46 bug khác nhau, 1 bug chặn: trên một chiếc iPhone SE dùng tiếng Thụy Điển ở cỡ chữ lớn nhất, bạn không thể gửi tin nhắn văn bản chút nào | 69 phút, $4.62 |
| [AnkiDroid](https://github.com/ankidroid/Anki-Android) (Kotlin, 12k stars, APK bản release) | 10 Android | 43 vấn đề khác nhau, trong đó có các nút trả lời bị cắt một nửa ở font 200% và "Get started" còn nguyên tiếng Anh trong ba ngôn ngữ | dưới 10 phút, $4.69 |
| [Ứng dụng mẫu animations của Flutter](https://github.com/flutter/packages/tree/main/packages/animations/example) | 1 Android, 1 iOS | Cùng một lỗi chính tả, được cả hai agent tự báo cáo độc lập, và một ứng dụng phớt lờ chế độ tối | 19 phút, $0.47 |

34 trong số 46 bug của LocalSend không liên quan gì đến cỡ chữ hay khả năng tiếp cận: các luồng bị hỏng, lỗi âm thầm, thiếu bản dịch, bố cục máy tính bảng. Ảnh chụp màn hình, mọi phát hiện và các lệnh để chạy lại chúng: [docs/examples.vi.md](docs/examples.vi.md).

## Bắt đầu nhanh

```bash
gh repo create my-testers --public --template pierridotite/mobile-agent-testers --clone
cd my-testers && ./setup.sh           # key pair + secrets + Claude token (Windows: .\setup.ps1)
gh workflow run agent-testers.yml     # demo: 2 Android + 2 iOS testers, ~20 min
bash scripts/collect.sh <run id>      # decrypts reports, groups duplicates: runs/<id>/SUMMARY.md
```

Để xem mọi màn hình của ứng dụng trên một bản đồ, với đường đi của các tester và bản đồ nhiệt trên từng màn hình, và bình luận từng màn hình: `python3 scripts/dashboard.py runs/<id>` ([docs/dashboard.md](docs/dashboard.md)). [Thử bản demo](https://pierridotite.github.io/mobile-agent-testers/) hoặc [xem video](assets/dashboard-demo.mp4).

Ứng dụng demo có [11 bug cố ý](demo-app/BUGS.md). Trong [một lần chạy](docs/runs/), 4 tester demo đã tìm thấy 9 trong số đó (2 đến 7 phút mỗi agent, khoảng 21 phút cho cả lần chạy, tính cả thời gian build), cộng thêm các vấn đề thật không ai gài (một nút nằm dưới thanh điều hướng Android, không dữ liệu nào được giữ lại sau khi mở lại). Mức dùng model: $0.87 cho cả 4.

Hoặc để Claude cài đặt cho ứng dụng của bạn:

```
/plugin marketplace add pierridotite/mobile-agent-testers
/plugin install mobile-agent-testers@mobile-agent-testers
```

## Kiểm thử ứng dụng của bạn

```bash
gh workflow run agent-testers.yml -f source_repo=you/app -f app_id=com.you.app \
  -f android_personas=anna,malik -f ios_personas=clara,jonas
```

Mọi input (build, brief, báo cáo bug, model rẻ hơn, tham số build, video) và các framework được hỗ trợ: [docs/configuration.vi.md](docs/configuration.vi.md).

## Đóng góp

Các ý tưởng mà sự giúp đỡ của bạn được chào đón. Mở một issue trước để nói bạn nhận ý tưởng nào, để hai người không xây cùng một thứ. [CONTRIBUTING.md](CONTRIBUTING.md) nói mỗi ý tưởng bắt đầu ở đâu và cách test nó offline.

- **Các agent CLI khác**: OpenCode, Codex CLI, Gemini CLI, Goose. Các công cụ là script thuần và nhiệm vụ là một brief Markdown, nên agent là một lệnh cho mỗi nền tảng.
- **Ứng dụng web**: cùng các persona trên một trình duyệt di động bằng Playwright, cho PWA và các trang web di động.
- **Điện thoại Android thật** qua adb, trên một self-hosted runner.
- **Persona mới**: bộ lọc mù màu, một mạng chậm hoặc chập chờn.
- **Từ phát hiện thành issue**: biến bản tóm tắt thành các GitHub issue nháp kèm ảnh chụp màn hình của chúng, hoặc một tin nhắn Slack hay Discord.
- **CI khác**: GitLab CI, Bitrise, Codemagic.
- **Thêm hệ thống build**: NativeScript, Unity, Godot, mỗi cái là một case trong `scripts/build/`.
- **Bản dịch** của README và tài liệu.
- **So sánh model**: 11 bug được gài sẵn của ứng dụng demo tạo thành một benchmark nhỏ. Một bảng về những gì mỗi model tìm thấy và chi phí của nó.

## Phiên bản hosted cho nhóm (đang thăm dò)

Workflow ở đây vẫn miễn phí, mã nguồn mở và đầy đủ. Tôi đang thăm dò một phiên bản hosted cho các nhóm có ứng dụng riêng tư: kèm macOS runner (trên một repo riêng tư, GitHub tính mỗi phút macOS gấp mười lần) và một dashboard web để chia sẻ các báo cáo với cả nhóm, gồm cả các quản lý sản phẩm.

Điều đó có giúp ích cho nhóm của bạn không? [Trả lời ba câu hỏi](https://github.com/pierridotite/mobile-agent-testers/issues/new?template=teams.yml) hoặc [viết cho tôi trên LinkedIn](https://www.linkedin.com/in/pierre-raffalli/). Đã chạy nó trên ứng dụng của bạn rồi? [Kể cho tôi nó đã tìm thấy gì và điều gì gây trở ngại](https://github.com/pierridotite/mobile-agent-testers/issues/new?template=usage-feedback.yml).

## Lịch sử star

[![Star History Chart](https://api.star-history.com/svg?repos=pierridotite/mobile-agent-testers&type=Date)](https://star-history.com/#pierridotite/mobile-agent-testers&Date)

## Ghi công

[Claude Code](https://docs.claude.com/en/docs/claude-code) ·
[AXe](https://github.com/cameroncooke/axe) ·
[android-emulator-runner](https://github.com/ReactiveCircus/android-emulator-runner)

## Người đóng góp

Cảm ơn tất cả những người đã đóng góp. Muốn giúp một tay? [Bắt đầu tại đây](CONTRIBUTING.md).

<a href="https://github.com/pierridotite/mobile-agent-testers/graphs/contributors"><img src="https://contrib.rocks/image?repo=pierridotite/mobile-agent-testers" alt="Contributors" /></a>

Được làm bởi [@pierridotite](https://github.com/pierridotite) ([LinkedIn](https://www.linkedin.com/in/pierre-raffalli/)) · Giấy phép MIT
