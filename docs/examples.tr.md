# Uygulamalı örnekler

[English](examples.md) · [简体中文](examples.zh-CN.md) · [日本語](examples.ja.md) · [Español](examples.es.md) · [Português](examples.pt-BR.md) · [Tiếng Việt](examples.vi.md) · Türkçe

Açık kaynak uygulamalarda gerçek çalıştırmalar, önceden yerleştirilmiş hiçbir hata yok. Her sayı, tüm raporların cihaz cihaz listelendiği [çalıştırmalardan](runs/) geliyor.

## Sadece erişilebilirlik değil

En çarpıcı ekran görüntülerini büyük yazı boyutu veriyor ama bu, ajanların raporladıklarının küçük bir kısmı. Her personanın bir hedefi var ("bir görevi İngilizce olmadan bitir", "her metin alanını Türkçe karakterler, emoji ve çok uzun metinle doldur", "uygulamadan işin ortasında çık, sonra geri dön") ve bu hedefe yeni bir kullanıcı gibi ulaşmaya çalışıyor; bu yüzden ajanlar önlerine çıkan her şeyi raporluyor:

- **Bozuk akışlar ve sessiz hatalar**: sonsuza kadar yüklenen ve hiçbir şey bulunamadığını asla söylemeyen bir cihaz listesi (20 telefonun 12'sinde), yalnızca "Error" diyen başarısız bir gönderim, bir adres cevap vermediğinde sonsuza kadar bekleyen bir diyalog, tek bir yanlış dokunuşta yazdıklarını silen bir metin diyaloğu.
- **İlk kullanım ve izinler**: AnkiDroid, sistem sayfasında tüm dosyalara erişim izni verene kadar kurulumu hiçbir açıklama yapmadan engelliyor (10 ajanının 8'i).
- **Çeviriler**: Portekizce, İspanyolca ve Japonca telefonlarda İngilizce kalmış "Get started", Portekizcede "Jogo da velha" (XOX oyunu) diye etiketlenmiş # sekmesi, Arapçada "B 14" diye yazılmış boyutlar, telefonun biçimi yerine ISO tarihleri, yalnızca İngilizce olan bir gizlilik politikası.
- **Metinler**: Flutter'ın resmi örneğinde bir yazım hatası, "Files: 1" diye sayılan bir metin mesajı, ana ekranda hiçbir açıklaması olmayan "#16 #15".
- **Cihazlar**: telefon düzenini dar bir sütunda gösteren tabletler, İptal ve Onayla butonlarının üstüne gelen klavye, bir listenin son öğelerinin üstüne gelen Android gezinme çubuğu, karanlık modu görmezden gelen bir uygulama.
- **Veriler**: yeniden başlatmadan sonra hiçbir şeyin korunmaması (demo uygulamada).

LocalSend'de 46 hatanın 34'ünün yazı boyutu ya da erişilebilirlikle ilgisi yoktu. Her rapor ayrıca personanın görüşü ve 10 üzerinden bir puanla (uygulamayı kullanmaya devam eder miydi?) bitiyor; böylece bir çalıştırma yalnızca neyin bozuk olduğunu değil, yeni kullanıcıların nerede kaybolduğunu da gösteriyor.

## LocalSend: 20 ajan, 20 cihaz, 14 dil

<p align="center"><img src="../assets/real-findings-localsend-20.png" alt="Flutter'ın taşma şeritlerini gösteren, her biri kırmızı çerçeveyle işaretlenmiş dört LocalSend ekranı: %200 yazı boyutunda, altı Gönder butonunun hepsinin taştığı bir Pixel 7; en büyük yazı boyutunda İsveççe, mesaj diyaloğunun 149 piksel taşıp metin alanını ve Onayla'yı gizlediği bir iPhone SE; Ekle butonunun kartından 114 piksel taştığı Almanca bir iPad Air; ve varsayılan yazı boyutunda bir satırın 28 piksel taştığı Fransızca bir iPhone SE" width="100%"></p>

[LocalSend](https://github.com/localsend/localsend), 100 bin yıldızlı bir Flutter uygulaması. Ajanların bulduğu:

- iOS'un en büyük yazı boyutunda İsveççe iPhone SE: mesaj diyaloğu 149 px taşıyor, bu yüzden metin alanı ve Onayla ekranın dışında kalıyor ve hiç metin gönderemiyorsun.
- Android'in %200 yazı boyutunda Pixel 7: altı Gönder butonunun hepsi taşıyor ve etiketleri yarıdan kesiliyor. İlk erişilebilirlik boyutunda İtalyanca bir iPhone 16 Plus'ta da aynısı.
- Büyük yazılı Almanca iPad Air: "Hinzufügen" kartının 114 px dışına taşıyor ve Ayarlar'daki değerler kelimenin ortasından bölünüyor.
- Varsayılan yazı boyutunda Fransızca iPhone SE: "Recevoir via un lien" 28 px taşıyor ve bir onay kutusunu gizliyor.
- İspanyolca Pixel Tablet: klavye, adres diyaloğunun İptal ve Onayla butonlarını kapatıyor.
- Başarısız bir manuel gönderim, hiçbir sebep belirtmeden yalnızca "Error", "Erreur", "Fehler", "エラー", "Hata" ya da "Erro" diyor. iOS'ta cevap vermeyen bir adres, diyaloğu hiçbir ilerleme göstermeden bekletiyor (üç ajan).

Toplam 103 rapor (1 engelleyici, 15 önemli). Elle gruplandığında 46 farklı hata çıkıyor ve bunların çoğunu yalnızca tek bir telefon gördü:

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="../assets/bugs-per-phone-dark.svg">
  <img src="../assets/bugs-per-phone-light.svg" alt="LocalSend'in 46 hatasından 29'unu, tek engelleyici dahil, yalnızca bir telefon gördü. 6 hatayı 2 telefon, 6 hatayı 3 telefon gördü; 4, 5, 6, 10 ve 12 telefonun gördüğü birer hata var: büyük yazıda taşan gönder butonları, erişilebilirlik etiketi olmayan ikon butonlar, yalnızca Error diyen başarısız gönderim, kelimenin ortasından kesilen ayar değerleri ve sonsuza kadar yüklenen cihaz listesi. Hiçbir hatayı 20 telefonun 12'sinden fazlası görmedi." width="100%">
</picture>

Başlangıçtan son rapora kadar 69 dakika (Free plan aynı anda 5 macOS işi çalıştırıyor), 20 telefon için 4,62 $ model kullanımı. Tüm raporlar, cihaz cihaz: [docs/runs/localsend-20.md](runs/localsend-20.md). Kendin çalıştır:

```bash
gh workflow run agent-testers.yml -f source_repo=localsend/localsend -f app_dir=app \
  -f app_id=org.localsend.localsend_app -f android_personas=all -f ios_personas=all -f record_video=true
```

## AnkiDroid: 10 ajan, doğrudan release APK'sinden

<p align="center"><img src="../assets/real-findings-ankidroid.png" alt="Her sorunun kırmızı çerçeveyle işaretlendiği dört AnkiDroid ekranı: %200 yazı boyutunda Again, Hard, Good ve Easy butonlarının yarıdan kesildiği bir Pixel 7 (büyütülmüş olarak), aynı telefonda satırların sağ kenardan taştığı İstatistikler sayfası ve Get started butonunun hâlâ İngilizce olduğu Portekizce ve Japonca karşılama ekranları" width="100%"></p>

[AnkiDroid](https://github.com/ankidroid/Anki-Android), 12 bin yıldızlı bir Kotlin uygulaması. GitHub release'indeki APK'den, build olmadan, 10 Android emülatöründe 10 ajan:

- Android'in %200 yazı boyutunda Pixel 7: Again / Hard / Good / Easy butonları yarıdan kesiliyor ve İstatistikler metni sağ kenardan taşıyor. İkinci bir ajan, butonların 2.26 alpha sürümünde de kesik olduğunu buldu; "Show large answer buttons" açıkken her iki satırda da.
- Karşılama ekranı, Portekizce, İspanyolca ve Japoncaya ayarlı cihazlarda "Get started" diyor, ekranın geri kalanı ise çevrilmiş (üç ajan, her biri kendi başına).

83 rapor, 43 farklı soruna gruplanıyor (bunların ikisi uygulamadan değil, Android 10 ve 11 emülatör imajlarındaki eski sistem WebView'ından kaynaklanıyor). Başlangıçtan son rapora kadar 10 dakikadan kısa, 10 telefon için 4,69 $ model kullanımı. Tüm raporlar: [docs/runs/ankidroid-10.md](runs/ankidroid-10.md). Kendin çalıştır:

```bash
gh workflow run agent-testers.yml -f app_dir=. -f app_id=com.ichi2.anki -f brief=briefs/ankidroid.md \
  -f android_build="curl -fsSL -o anki.apk https://github.com/ankidroid/Anki-Android/releases/download/v2.25.3/AnkiDroid-2.25.3-full-universal.apk" \
  -f apk_path=anki.apk -f android_personas=all -f ios_personas=none -f block_hosts="ankiweb.net www.google-analytics.com"
```

## Flutter'ın resmi animasyon örneği

<p align="center"><img src="../assets/real-finding-flutter.png" alt="Flutter'ın animasyon örnek uygulamasının aynı ekranında iki telefon, bir Pixel 7 ve Fransızca bir iPhone SE; Streamling your courses başlığı daire içine alınmış: iki ajan da bu yazım hatasını kendi başına raporladı" width="100%"></p>

[flutter/packages'ın örnek uygulamasında](https://github.com/flutter/packages/tree/main/packages/animations/example), Pixel 7'deki bir ajan ve Fransızca bir iPhone SE'deki başka bir ajan aynı yazım hatasını, "Streamling your courses" ([110. satır](https://github.com/flutter/packages/blob/main/packages/animations/example/lib/shared_axis_transition.dart#L110)), birbirinden bağımsız olarak raporladı; Android'deki ajan ayrıca uygulamanın karanlık modu görmezden geldiğini fark etti. 19 dakika, 0,47 $ model kullanımı.

Kendi uygulamanda çalıştırdın mı? [Neler bulduğunu paylaş](https://github.com/pierridotite/mobile-agent-testers/issues/new?title=Finding:%20), [çalıştırmalara](runs/) eklensin. Bu uygulamalardan birinin bakımını yapıyorsan ve bir bulgunun yanlış olduğunu düşünüyorsan bir issue aç; düzeltilir ve yeniden çalıştırılır.
