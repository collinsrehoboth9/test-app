# mobile-agent-testers

<a href="https://github.com/pierridotite/explore-compile-replay"><img src="assets/ecr-now-building.svg" alt="Next: every bug becomes a free test. Explore-Compile-Replay" width="340" height="54"></a>
<a href="https://www.linkedin.com/in/pierre-raffalli/"><picture><source media="(prefers-color-scheme: dark)" srcset="assets/linkedin-follow-dark.svg"><img src="assets/linkedin-follow-light.svg" alt="Follow Pierre Raffalli on LinkedIn" width="160" height="54"></picture></a>
<a href="https://trendshift.io/repositories/371956?utm_source=trendshift-badge&amp;utm_medium=badge&amp;utm_campaign=badge-trendshift-371956" target="_blank" rel="noopener noreferrer"><img src="https://trendshift.io/api/badge/trendshift/repositories/371956/daily?language=Python" alt="pierridotite%2Fmobile-agent-testers | Trendshift" width="250" height="55"/></a>

<sub>Sırada yaptığım şey: bir ajan hata bulduğunda, <a href="https://github.com/pierridotite/explore-compile-replay">Explore-Compile-Replay</a> ajanın izlediği yolu, her PR'da modelsiz yeniden çalışan bir teste dönüştürür. Henüz erken bir taslak.</sub>

**20 farklı telefonda 20 yapay zekâ test edici, paralel olarak, GitHub'ın ücretsiz runner'larında.**

**[Örnekler](docs/examples.tr.md)** · **[Karşılaştırma](docs/compare.md)** · **[SSS](docs/faq.tr.md)** · **[Personalar](personas.json)** · **[Katkıda bulunma](CONTRIBUTING.md)** · **[Sürümler](https://github.com/pierridotite/mobile-agent-testers/releases)**

[English](README.md) · [简体中文](README.zh-CN.md) · [日本語](README.ja.md) · [Español](README.es.md) · [Português](README.pt-BR.md) · [Tiếng Việt](README.vi.md) · Türkçe

<sub>İngilizce README'nin çevirisidir: geride kalabilir, esas alınan sürüm [README.md](README.md) dosyasıdır.</sub>

Yazılacak test scripti yok. Her test edici, GitHub Actions üzerinde kendi iOS
Simulator'ı ya da Android emülatörü olan bir Claude Code ajanıdır. Bir persona
canlandırır (en büyük yazı boyutunda İsveççe bir iPhone SE, %200'de bir Pixel 7,
Almanca bir iPad, sağdan sola Arapça...), uygulamanı yeni bir kullanıcı gibi kullanır
(ya da gösterdiğin özelliği baştan sona dener) ve ekran görüntüleriyle bir hata raporu yazar.
Flutter, React Native, Expo, native Kotlin ve Swift, Kotlin Multiplatform, Capacitor ve .NET MAUI.

GitHub'ın runner'ları public repolar için ücretsizdir. Yalnızca modelin ücretini,
Claude aboneliğinle ya da API anahtarınla ödersin: [bu çalıştırmalarda](docs/runs/) telefon başına yaklaşık 0,15 ile 0,55 $.

<p align="center"><a href="assets/localsend-20.mp4"><img src="assets/localsend-20.gif" alt="LocalSend'i aynı anda kullanan 20 yapay zekâ test edicinin gerçek ekran kayıtları: 14 dilde 10 Android emülatörü ve 10 iOS Simulator, küçük bir Pixel'den 13 inç bir iPad'e kadar. Bir test edici işini bitirdiğinde telefonu kaç sorun bulduğunu gösterir. Videoyu açar." width="100%"></a><br>
<sub>LocalSend çalıştırmasının gerçek ekran kayıtları, boşta geçen süreler kesildi. <a href="assets/localsend-20.mp4">Videonun tamamı (MP4)</a> · <a href="docs/runs/localsend-20.md">Çalıştırma, cihaz cihaz</a></sub></p>

[![Tests](https://github.com/pierridotite/mobile-agent-testers/actions/workflows/tests.yml/badge.svg)](https://github.com/pierridotite/mobile-agent-testers/actions/workflows/tests.yml)
[![Stars](https://img.shields.io/github/stars/pierridotite/mobile-agent-testers?style=flat)](https://github.com/pierridotite/mobile-agent-testers/stargazers)
[![License: MIT](https://img.shields.io/badge/license-MIT-green)](LICENSE)
![iOS + Android](https://img.shields.io/badge/platforms-iOS%20%2B%20Android-blue)
![Claude Code plugin](https://img.shields.io/badge/Claude%20Code-plugin-orange)
[![Last commit](https://img.shields.io/github/last-commit/pierridotite/mobile-agent-testers)](https://github.com/pierridotite/mobile-agent-testers/commits/main)
[![Discord](https://img.shields.io/badge/Discord-join-5865F2?logo=discord&logoColor=white)](https://discord.gg/WuaCANscr8)

## Ne yapar

Her çalıştırmada tek bir kullanım; [Kendi uygulamanı test et](#kendi-uygulamanı-test-et) bölümündeki girdilerle seçilir.

- **Yeni kullanıcılar gibi keşfet** (varsayılan). Her persona kendi hedefine ulaşmaya çalışır ve önüne çıkan engelleri numaralı ekran görüntüleriyle raporlar.
- **Tek bir özelliği baştan sona dene.** Kendi yazdığın talimat (brief), her ajanı bütün bir akıştan geçirir: birkaç adımlı bir rezervasyon, geri alma özelliği olan bir editör, iki ekran arasında bir senkronizasyon. Ajanlar dokunur, uzun basar, kaydırır ve yazı yazar; uygulamadan çıkıp geri döner, neyin kaydedildiğini görmek için uygulamayı yeniden başlatır ya da doğrudan özellikten başlamak için bir deep link açar. Bir talimat, yaklaşık 30 eylemlik bütçeyi artırabilir. [AnkiDroid'inki](briefs/ankidroid.md), yeni bir kartın Arka yüz alanının klavyenin arkasında kaldığını buldu.
- **Bir hata raporunu yeniden üret**, her telefonda: bir issue ya da adımları ver, her ajan ekran görüntüleriyle evet, hayır ya da belirsiz diye cevaplasın ([briefs/reproduce.md](briefs/reproduce.md)).
- **İyileştirme raporu al.** Her persona uygulamayı bir kez kullanır, sonra nerede tereddüt ettiğini, ne beklediğini ve önce neyin değişmesi gerektiğini alana ve etkiye göre söyler ([briefs/improve.md](briefs/improve.md)).
- **İki sürümü karşılaştır.** İki branch'te ya da iki build'de aynı personalar, ardından tek bir tablo: ulaşılan hedefler, eylemler, puanlar, yalnızca bir sürümde görülen sorunlar ([docs/compare-versions.md](docs/compare-versions.md)). Gerçek bir A/B testinden önce yapılan bir kontroldür, onun yerine geçmez.
- **Daha ucuz bir model kullan**: Claude Haiku ya da Anthropic uyumlu herhangi bir API (OpenRouter, DeepSeek, Kimi, GLM, MiniMax, Qwen). Bulgular, modelin ekran görüntülerini okuması ve akıl yürütmesi kadar iyidir ([docs/cheaper-models.md](docs/cheaper-models.md)).

Maestro ya da Appium senin yazdığın testleri çalıştırır. Bu ajanlar ise kimsenin test etmeyi düşünmediği şeyleri arar: [nasıl karşılaştırıldığı](docs/compare.md).

## Uygulamalı örnekler

Açık kaynak uygulamalarda gerçek çalıştırmalar, önceden yerleştirilmiş hiçbir hata yok.

| Uygulama | Telefonlar | Ajanların bulduğu | Süre, model kullanımı |
|---|---|---|---|
| [LocalSend](https://github.com/localsend/localsend) (Flutter, 100 bin yıldız) | 20: 10 Android, 10 iOS, 14 dil | 46 farklı hata, 1 engelleyici: en büyük yazı boyutunda İsveççe bir iPhone SE'de metin mesajı hiç gönderilemiyor | 69 dk, 4,62 $ |
| [AnkiDroid](https://github.com/ankidroid/Anki-Android) (Kotlin, 12 bin yıldız, release APK) | 10 Android | 43 farklı sorun; aralarında %200 yazı boyutunda yarıdan kesilen cevap butonları ve üç dilde İngilizce kalmış "Get started" var | 10 dk'nın altında, 4,69 $ |
| [Flutter'ın animasyon örneği](https://github.com/flutter/packages/tree/main/packages/animations/example) | 1 Android, 1 iOS | İki ajanın birbirinden bağımsız olarak raporladığı aynı yazım hatası ve karanlık modu görmezden gelen bir uygulama | 19 dk, 0,47 $ |

LocalSend'in 46 hatasından 34'ünün yazı boyutu ya da erişilebilirlikle ilgisi yoktu: bozuk akışlar, sessizce başarısız olan işlemler, eksik çeviriler, tablet düzenleri. Ekran görüntüleri, tüm bulgular ve onları yeniden çalıştırma komutları: [docs/examples.tr.md](docs/examples.tr.md).

## Hızlı başlangıç

```bash
gh repo create my-testers --public --template pierridotite/mobile-agent-testers --clone
cd my-testers && ./setup.sh           # key pair + secrets + Claude token (Windows: .\setup.ps1)
gh workflow run agent-testers.yml     # demo: 2 Android + 2 iOS testers, ~20 min
bash scripts/collect.sh <run id>      # decrypts reports, groups duplicates: runs/<id>/SUMMARY.md
```

Uygulamanızın tüm ekranlarını tek bir haritada, test edenlerin yolları ve her ekranda bir ısı haritasıyla görmek ve her ekrana yorum eklemek için: `python3 scripts/dashboard.py runs/<id>` ([docs/dashboard.md](docs/dashboard.md)). [Demoyu deneyin](https://pierridotite.github.io/mobile-agent-testers/) veya [videoyu izleyin](assets/dashboard-demo.mp4).

Demo uygulamada [bilerek bırakılmış 11 hata](demo-app/BUGS.md) var. [Bir çalıştırmada](docs/runs/) 4 demo test edici bunların 9'unu buldu (ajan başına 2 ile 7 dakika, build'ler dahil tüm çalıştırma yaklaşık 21 dakika), ayrıca kimsenin yerleştirmediği gerçek sorunlar da çıktı (Android gezinme çubuğunun altında kalan bir buton, yeniden başlatmadan sonra korunmayan veriler). Model kullanımı: 4 test edici için 0,87 $.

Ya da Claude'un bunu senin uygulaman için kurmasına izin ver:

```
/plugin marketplace add pierridotite/mobile-agent-testers
/plugin install mobile-agent-testers@mobile-agent-testers
```

## Kendi uygulamanı test et

```bash
gh workflow run agent-testers.yml -f source_repo=you/app -f app_id=com.you.app \
  -f android_personas=anna,malik -f ios_personas=clara,jonas
```

Tüm girdiler (build'ler, talimatlar, hata raporları, daha ucuz modeller, build argümanları, video) ve desteklenen framework'ler: [docs/configuration.tr.md](docs/configuration.tr.md).

## Katkıda bulunma

Yardıma açık olduğumuz fikirler. Aynı şeyi iki kişi yapmasın diye, hangisini üstlendiğini söylemek için önce bir issue aç. [CONTRIBUTING.md](CONTRIBUTING.md) her birinin nereden başladığını ve çevrim dışı nasıl test edileceğini anlatır.

- **Diğer ajan CLI'ları**: OpenCode, Codex CLI, Gemini CLI, Goose. Araçlar sade scriptler, görev de bir Markdown talimatı; bu yüzden ajan, platform başına tek bir komut.
- **Web uygulamaları**: aynı personalar, Playwright ile bir mobil tarayıcıda; PWA'lar ve mobil siteler için.
- **Fiziksel Android telefonlar**, adb üzerinden, self-hosted bir runner'da.
- **Yeni personalar**: renk körlüğü filtreleri, yavaş ya da kesintili bir ağ.
- **Bulgulardan issue'lara**: özeti ekran görüntüleriyle birlikte taslak GitHub issue'larına ya da bir Slack veya Discord mesajına dönüştürmek.
- **Diğer CI'lar**: GitLab CI, Bitrise, Codemagic.
- **Daha fazla build sistemi**: NativeScript, Unity, Godot; her biri `scripts/build/` içinde bir case.
- README ve dokümanların **çevirileri**.
- **Model karşılaştırması**: demo uygulamadaki 11 yerleştirilmiş hata küçük bir benchmark oluşturur. Her modelin neyi bulduğunu ve maliyetini gösteren bir tablo.

## Ekipler için barındırılan sürüm (araştırılıyor)

Buradaki workflow ücretsiz, açık kaynak ve eksiksiz kalacak. Private uygulamaları olan ekipler için barındırılan bir sürümü araştırıyorum: macOS runner'ları dahil (private bir repoda GitHub her macOS dakikasını on kat sayar) ve raporları ürün yöneticileri de dahil tüm ekiple paylaşmak için bir web paneli.

Bu ekibinin işine yarar mı? [Üç soruyu cevapla](https://github.com/pierridotite/mobile-agent-testers/issues/new?template=teams.yml) ya da [bana LinkedIn'den yaz](https://www.linkedin.com/in/pierre-raffalli/). Kendi uygulamanda zaten çalıştırdın mı? [Neler bulduğunu ve nelerin önüne engel çıktığını anlat](https://github.com/pierridotite/mobile-agent-testers/issues/new?template=usage-feedback.yml).

## Yıldız geçmişi

[![Star History Chart](https://api.star-history.com/svg?repos=pierridotite/mobile-agent-testers&type=Date)](https://star-history.com/#pierridotite/mobile-agent-testers&Date)

## Teşekkürler

[Claude Code](https://docs.claude.com/en/docs/claude-code) ·
[AXe](https://github.com/cameroncooke/axe) ·
[android-emulator-runner](https://github.com/ReactiveCircus/android-emulator-runner)

## Katkıda bulunanlar

Katkıda bulunan herkese teşekkürler. Yardım etmek ister misin? [Buradan başla](CONTRIBUTING.md).

<a href="https://github.com/pierridotite/mobile-agent-testers/graphs/contributors"><img src="https://contrib.rocks/image?repo=pierridotite/mobile-agent-testers" alt="Contributors" /></a>

[@pierridotite](https://github.com/pierridotite) tarafından yapıldı ([LinkedIn](https://www.linkedin.com/in/pierre-raffalli/)) · MIT lisansı
