# Glowo'yu Android APK Yapıp APKPure'a Yükleme Rehberi

## Önce önemli bir gerçeği söyleyeyim

Bu APK'yı **kendi bilgisayarımda (bu sohbet ortamında) senin için üretemedim** çünkü buradaki
sistemin internet erişimi kısıtlı — Android'in kendi altyapısına (Google'ın sunucularına) ve
hatta bazı temel paket sunucularına bile ulaşamıyor. Firebase kurulumunda da aynı sorunu
yaşamıştık, hatırlarsın: o yüzden seni Firebase Console üzerinden adım adım yönlendirmiştim.
Android APK için de aynı mantıkla ilerleyeceğiz: **Codemagic** adında ücretsiz bir bulut servisi
senin yerine gerçek bir bilgisayarda (kısıtlaması olmayan) derlemeyi yapacak, ben de o derlemeyi
tetikleyecek dosyaları (`codemagic.yaml` ve proje dosyaları) senin için hazırladım.

Ayrıca şunu da baştan netleştireyim: **APKPure'a fiilen yükleme işlemini** (geliştirici hesabı
açmak, APK dosyasını yüklemek, uygulama açıklaması/ekran görüntüsü girmek) **senin kendin
yapman gerekiyor** — bu, üçüncü parti bir web sitesinde hesap açmayı gerektirdiği için benim
senin adına yapabileceğim bir şey değil. Ama APK dosyası elinde olduktan sonra o kısım oldukça
basit, aşağıda onu da anlatıyorum.

---

## Adım 1 — GitHub'da ücretsiz bir depo (repo) oluştur

Codemagic'in derleme yapabilmesi için proje dosyalarının bir yerde (GitHub gibi) durması lazım.

1. https://github.com adresine git, hesabın yoksa ücretsiz bir hesap oluştur ("Sign up").
2. Giriş yaptıktan sonra sağ üstteki **+** işaretine tıkla → **New repository**.
3. İsim olarak `glowo-app` yaz (istediğin ismi verebilirsin), **Private** ya da **Public**
   seçebilirsin (fark etmez), **Create repository** de.
4. Açılan sayfada **"uploading an existing file"** linkine tıkla (ya da "Add file" →
   "Upload files").
5. Sana ayrıca gönderdiğim `glowo-native.zip` dosyasını bilgisayarına indir, **zip'i aç**
   (çıkar), içindeki TÜM dosya ve klasörleri (www, resources, package.json, capacitor.config.json,
   codemagic.yaml, README.md, .gitignore) GitHub'ın açtığı yükleme kutusuna sürükle-bırak yap.
6. Altta **"Commit changes"** butonuna basarak yükle.

Git komut satırı kullanmana gerek yok, tamamen tarayıcıdan sürükle-bırakla yapılabiliyor.

---

## Adım 2 — Codemagic hesabı aç ve depoyu bağla

1. https://codemagic.io adresine git, **"Sign up"** ile ücretsiz hesap aç — GitHub hesabınla
   giriş yapman en kolayı (**"Sign up with GitHub"**).
2. Giriş yaptıktan sonra **"Add application"** de, listeden az önce oluşturduğun `glowo-app`
   deposunu seç.
3. Codemagic, depodaki `codemagic.yaml` dosyasını otomatik olarak bulacak (onu senin için zaten
   hazırladım, içinde `android-release` adında bir iş akışı (workflow) var).

---

## Adım 3 — Derlemeyi başlat

1. Codemagic panelinde uygulamanı aç, üstte iş akışı (workflow) seçme kısmından
   **"Glowo - Android Release (APK)"** seçeneğini seç.
2. **"Start new build"** butonuna bas.
3. Derleme yaklaşık 5-15 dakika sürer (ilk seferinde biraz daha uzun olabilir). Sayfayı açık
   tutup bekleyebilir ya da kapatıp sonra kontrol edebilirsin.
4. Derleme bittiğinde sayfanın altında **"Artifacts"** (çıktılar) bölümünde bir `.apk` dosyası
   ve bir `glowo-release-key.jks` dosyası göreceksin.
   - `.apk` dosyası → telefonuna kurabileceğin / APKPure'a yükleyeceğin dosya.
   - `.jks` dosyası → uygulamanın "imza anahtarı". **Bunu indirip güvenli bir yerde sakla**
     (örneğin Google Drive'a yedekle). Uygulamayı ileride güncellersen, mağazaların
     güncellemeyi kabul etmesi için aynı anahtarla imzalanmış olması gerekir. Kaybedersen,
     uygulamanın yeni bir sürümünü eskisinin üzerine güncelleme olarak yayınlayamazsın
     (yeni bir uygulama gibi baştan yayınlaman gerekir).

**Eğer derleme hata verirse**: hata mesajının ekran görüntüsünü bana gönder, birlikte
çözeriz — tıpkı Firebase kurulumunda yaptığımız gibi. Bunu bu ortamda önceden test edemedim,
o yüzden ilk seferde küçük bir pürüz çıkma ihtimali var, ama çözülebilir türden şeyler olur
genelde (bir ayar eksikliği gibi).

*(İstersen önce daha basit olan **"Glowo - Android Debug (test APK)"** iş akışını çalıştırıp
hızlıca kendi telefonuna kurup deneyebilirsin — ama bu imzasız/test APK'sı olduğu için
APKPure'a yüklemeye uygun değil, sadece "çalışıyor mu" diye hızlı kontrol için.)*

---

## Adım 4 — APK'yı APKPure'a yükle (bu kısmı sen yapacaksın)

1. https://apkpure.com adresine git, sayfanın altında **"Developer"** / **"Submit App"**
   gibi bir bağlantı bulacaksın (APKPure'un geliştirici yükleme sayfası zaman zaman değişebilir,
   göremezsen "APKPure developer upload" diye arayabilirsin).
2. Geliştirici hesabı oluştur (e-posta ile kayıt).
3. "Yeni uygulama ekle" / "Submit new app" gibi bir seçenekle Codemagic'ten indirdiğin
   `.apk` dosyasını yükle.
4. İstenen bilgileri doldur: uygulama adı (Glowo), kısa açıklama, kategori (Sosyal), ekran
   görüntüleri (uygulamadan birkaç ekran görüntüsü alman yeterli), uygulama ikonu
   (`resources/icon.png` dosyasını kullanabilirsin), gizlilik politikası linki (varsa).
5. İncelemeye gönder — APKPure genelde kendi incelemesini yapıp birkaç gün içinde yayınlıyor.

---

## Sonraki güncellemelerde

Uygulamada değişiklik yaptığımızda (yeni özellik, hata düzeltmesi vb.):
1. Güncellenmiş `www/index.html` dosyasını GitHub deponda eskisinin üzerine yükle
   (Adım 1.5'teki gibi sürükle-bırak, "Commit changes").
2. Codemagic'te tekrar **"Start new build"** de.
3. Yeni APK'yı indirip APKPure'daki "update" akışıyla yükle.

Her seferinde aynı imza anahtarını kullanman gerektiğini unutma (Adım 3'teki not).
