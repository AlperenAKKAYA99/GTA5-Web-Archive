# PlayGTA5 - Yerel Tarayıcı Oyunu (Local Browser Game)

Bu proje, **PlayGTA5** tarayıcı oyununun `mirror` dizini altındaki **12.482 dosyanın tamamını** içeren derlenmiş istemci anlık görüntüsüdür (compiled client snapshot). Orijinal oyun motoru kaynak kodlarını içermez; doğrudan modern web tarayıcıları üzerinde çalıştırılmak üzere derlenmiş WebAssembly ve WebGPU istemcisidir.

Oyun verilerinin toplam boyutu yaklaşık **21,2 GB (19,7 GiB)** seviyesindedir. Bazı derlenmiş oyun dosyaları GitHub'ın 100 MiB Git dosya sınırını aştığı için, tüm oyun verileri Git commit'leri yerine **GitHub Releases** üzerinden 14 parça halinde barındırılmaktadır. Bu nedenle sadece **Code → Download ZIP** seçeneğini kullanmak oyun verilerini indirmez; oyunu oynayabilmek için aşağıdaki kurulum adımları izlenmelidir.

---

## İçindekiler

- [Hızlı Başlangıç (Windows)](#hızlı-başlangıç-windows)
- [Sistem Gereksinimleri](#sistem-gereksinimleri)
- [Düşük Bellek ve Performans Modu](#düşük-bellek-ve-performans-modu)
- [Manuel Kurulum (Alternatif Yöntem)](#manuel-kurulum-alternatif-yöntem)
- [Dosya Bütünlüğünü Doğrulama](#dosya-bütünlüğünü-doğrulama)
- [Diğer İşletim Sistemleri (Linux ve macOS)](#diğer-işletim-sistemleri-linux-ve-macos)
- [Yerel Sunucunun Çalışma Mantığı](#yerel-sunucunun-çalışma-mantığı)
- [Sorun Giderme (Sık Karşılaşılan Hatalar)](#sorun-giderme-sık-karşılaşılan-hatalar)
- [Kapsam, Köken ve Yasal Uyarı](#kapsam-köken-ve-yasal-uyarı)

---

## Hızlı Başlangıç (Windows)

Windows kullanıcıları için süreç tamamen otomatikleştirilmiştir. Harici bir Python kurulumu, Netlify hesabı, üyelik veya API anahtarı gerekmez.

1. **Depoyu İndirin:** Bu sayfadaki **Code → Download ZIP** düğmesine tıklayın ve indirilen ZIP arşivini bilgisayarınızda boş bir klasöre çıkartın (veya depoyu doğrudan Git ile klonlayın).
2. **İndiriciyi Başlatın:** Çıkarttığınız klasördeki **`Download-Game.cmd`** dosyasına çift tıklayın.
   - Bu betik, [Full Game v1 Sürümü](https://github.com/alihammoud21/playgta5/releases/tag/full-game-v1) üzerinden 14 arşiv parçasını sırayla indirir.
   - Her parçanın SHA-256 sağlama toplamını doğrular, dosyaları ayıklar ve taşınabilir Python ortamını hazırlar.
   - Oyun dosyaları, indirme aracının bulunduğu klasörle çakışmayı önlemek için otomatik olarak `game` alt klasörüne kurulur. İndirilen arşiv önbelleği indirme aracının klasöründe tutulur.
   - İndirilen arşiv parçaları ayıklandıktan sonra otomatik olarak silinir, böylece disk alanı tasarrufu sağlanır.
   - Ekranda yeşil renkli **"Download complete"** yazısını görene kadar pencereyi kapatmayın.
   - Bağlantı koparsa veya indirme yarıda kalırsa, dosyayı tekrar çalıştırmanız yeterlidir; tamamlanan kısımlar korunur ve indirme kaldığı yerden devam eder.
3. **Oyunu Başlatın:** Kurulum bittiğinde `game` klasöründeki **`Launch-Local.cmd`** dosyasına çift tıklayın. Oyunu oynadığınız süre boyunca açılan sunucu penceresini kapatmayın.
4. **Tarayıcıda Açın:** Varsayılan tarayıcınızda otomatik olarak **http://localhost:8000/** adresi açılacaktır.
   - Tarayıcınız otomatik açılmazsa, Google Chrome veya Microsoft Edge adres çubuğuna bu adresi manuel olarak yapıştırın.
   - Açılan ana menüden **Story Mode** (Hikaye Modu) veya **Sandbox Mode** (Serbest Gezinti Modu) seçeneğini belirleyin.
5. **Kontroller:**
   - Fare imlecini oyuna kilitlemek (pointer lock) ve tarayıcı sesini etkinleştirmek için oyun alanına bir kez tıklayın.
   - Fare kontrolünü serbest bırakmak için klavyeden **Esc** tuşuna basın.
   - Oynamayı bitirdiğinizde sunucu konsol penceresini kapatabilirsiniz.

> [!NOTE]
> Oyun tamamen kendi bilgisayarınızda yerel olarak çalıştırılır. Bu depo çevrim içi bir oyun barındırma hizmeti veya sunucusu değildir.

---

## Sistem Gereksinimleri

- **İşletim Sistemi:** Dahili gelen gömülü çalışma ortamı için 64-bit Windows 10 veya 11. (Diğer işletim sistemleri için Python 3.11+ gereklidir).
- **Tarayıcı:** Donanım ivmesi (**Hardware Acceleration**) ve **WebGPU** desteği etkinleştirilmiş güncel bir **Google Chrome** veya **Microsoft Edge**.
- **Ekran Kartı ve Bellek:** WebGPU destekli harici bir grafik kartı (GPU) ve yeterli sistem belleği. Proje yerel olarak RTX 3070 ve 32 GB RAM konfigürasyonunda test edilmiştir; giriş seviyesi veya entegre grafik kartlarında akıcı performans garanti edilmez.
- **Boş Disk Alanı:**
  - Otomatik kurulum (`Download-Game.cmd`) için: En az **25 GB** boş alan (arşivler açıldıkça silindiği için).
  - Manuel kurulum yapıp ZIP arşivlerini saklamak isterseniz: Yaklaşık **45 GB** boş alan.
- **İnternet Bağlantısı:** İlk aşamadaki yaklaşık 21 GB boyutundaki oyun varlıklarını indirmek için hızlı ve kararlı bir internet bağlantısı gerekir. Kurulum tamamlandıktan sonra oyun tamamen çevrimdışı (offline) oynanabilir.
- **İlk Yükleme Süresi:** İlk dünya yüklemesi, WebAssembly modüllerinin başlatılması ve WebGPU gölgelendirici (shader) derlemeleri sisteminize bağlı olarak birkaç dakika sürebilir. İlerleme çubuğu yavaş ilerliyor gibi görünse bile sekmeyi kapatmayın.

---

## Düşük Bellek ve Performans Modu

Daha düşük donanıma sahip bilgisayarlarda bellek taşması (Out of Memory) veya tarayıcı sekmesi çökmelerini önlemek için optimize edilmiş parametrelerle başlatabilirsiniz:

👉 **[http://localhost:8000/?low=1&fps=30](http://localhost:8000/?low=1&fps=30)**

Bu parametreler:
- Bellek tüketimini sınırlar (`low=1`),
- Kare hızını saniyede 30 kareye sabitler (`fps=30`),
- Düşük VRAM'e sahip sistemlerde kararlılığı önemli ölçüde artırır.

---

## Manuel Kurulum (Alternatif Yöntem)

İndirme işlemini betik kullanmadan tarayıcı veya indirme yöneticisi ile kendiniz yapmak isterseniz:

1. [Releases → Full game v1](https://github.com/alihammoud21/playgta5/releases/tag/full-game-v1) sayfasına gidin.
2. Sayfada yer alan **14 adet ZIP arşivinin tamamını** (`playgta5-part-01-of-14.zip` ile `playgta5-part-14-of-14.zip` arası) ve `file-inventory.json` dosyasını indirin.
3. Bu arşivler parçalı/bölünmüş arşiv değil, birbirinden bağımsız ZIP arşivleridir. Hepsini **aynı boş klasörün içerisine** ayıklayın ve `file-inventory.json` dosyasını da bu klasörün kök dizinine yerleştirin.
4. Kurulum tamamlandığında klasör yapınız şu şekilde görünmelidir:

```text
playgta5/
├── Launch-Local.cmd
├── Start-Local.ps1
├── serve_local.py
├── verify_game.py
├── Verify-Game.cmd
├── file-inventory.json
├── runtime/
│   └── python.exe
└── mirror/
    └── playgta5.com/
        ├── index.html
        ├── b/
        └── data/
```

5. Oynamak için **`Launch-Local.cmd`** dosyasını çalıştırın.

> [!WARNING]
> `index.html` dosyasına doğrudan çift tıklayarak tarayıcıda açmaya çalışmayın! WebGPU iş parçacıkları (workers), `SharedArrayBuffer` için gereken çapraz kaynak izolasyonu (Cross-Origin Isolation) ve oyun verisi akışı (byte-range streaming) için depoda bulunan yerel HTTP sunucusu (`serve_local.py`) zorunludur.

---

## Dosya Bütünlüğünü Doğrulama

Kurulum sonrasında veya oyun açılırken herhangi bir eksik dosya hatası alırsanız, dosyaların sağlamlığını test etmek için:

- **`Verify-Game.cmd`** dosyasına çift tıklayın.

Otomatik kurulumda `game` klasöründeki **`Verify-Game.cmd`** dosyasını çalıştırın. Bu araç, `verify_game.py` betiğini çalıştırarak kurulan tüm dosyaları `file-inventory.json` içerisindeki resmi SHA-256 sağlama toplamları ile karşılaştırır. Disk üzerindeki yaklaşık 21 GB veriyi tek tek okuduğu için işlem diskinizin hızına bağlı olarak 2-5 dakika sürebilir.

Ayrıca indirilen ZIP arşivlerinin sağlama toplamları depodaki `SHA256SUMS.txt` dosyasında mevcuttur.

---

## Diğer İşletim Sistemleri (Linux ve macOS)

Proje Linux ve macOS üzerinde de çalıştırılabilir:

1. Tüm sürüm arşivlerini indirin ve aynı klasöre çıkartın.
2. Sisteminizde **Python 3.11 veya daha yeni** bir sürümün kurulu olduğundan emin olun.
3. Terminali açıp oyun klasörüne gidin ve yerel sunucuyu başlatın:
   ```bash
   python3 serve_local.py --open
   ```
4. WebGPU desteği açık olan Google Chrome veya Chromium tabanlı bir tarayıcıda `http://localhost:8000/` adresini açın.

> [!NOTE]
> Paket içerisindeki `runtime/python.exe` yalnızca Windows içindir; Linux ve macOS sistemlerde işletim sisteminin kendi Python ortamı kullanılır.

---

## Yerel Sunucunun Çalışma Mantığı

`serve_local.py` sıradan bir web sunucusu değildir; tarayıcının oyun motorunu çalıştırabilmesi için özel olarak yapılandırılmıştır:

1. **Çapraz Kaynak İzolasyon Başlıkları (Cross-Origin Isolation):**
   - `Cross-Origin-Opener-Policy: same-origin`
   - `Cross-Origin-Embedder-Policy: require-corp`
   - `Cross-Origin-Resource-Policy: same-origin`
   Bu başlıklar, modern tarayıcıların yüksek hassasiyetli zamanlayıcıları ve `SharedArrayBuffer` belleğini güvenle tahsis etmesi için şarttır.
2. **HTTP Kısmi Aralık İstekleri (206 Partial Content / Byte Ranges):**
   - Sunucu `Accept-Ranges: bytes` başlığını ve parça isteklerini destekler. Oyun motorunun 20 GB'lık devasa varlık dosyalarını tek seferde RAM'e yüklemek yerine ihtiyaç duydukça parça parça akıtmasını (streaming) sağlar.
3. **Toplu Veri Giriş/Çıkış API'si (`POST /data/batch`):**
   - Oyun motorunun tek bir HTTP isteğinde 1.000 adede kadar farklı dosya kesitini (maksimum 64 MiB) topluca talep edebilmesini sağlar. İsteğe bağlı `gzip` sıkıştırmasıyla veri aktarım gecikmesini en aza indirir.

---

## Sorun Giderme (Sık Karşılaşılan Hatalar)

- **Beyaz Ekran / Eksik Sayfa (Missing Mirror):**
  İndirme veya ayıklama işlemi eksik kalmış olabilir. 14 arşiv parçasının hepsinin indiğinden emin olun ve otomatik kurulum için `game\Verify-Game.cmd` aracını çalıştırarak eksik dosyaları tespit edin.
- **Port Çakışması ("Address already in use" - Hata 10048):**
  Bilgisayarınızda 8000 numaralı bağlantı noktasını başka bir program kullanıyor olabilir. İlgili programı kapatabilir veya sunucuyu farklı bir port üzerinden başlatabilirsiniz:
  ```cmd
  runtime\python.exe serve_local.py --port 8001 --open
  ```
  *(Sisteminizde Python yüklüyse: `python serve_local.py --port 8001 --open`)*
- **WebGPU Kullanılamıyor (WebGPU Unavailable):**
  - Tarayıcınızı ve ekran kartı sürücülerinizi (GPU drivers) en son sürüme güncelleyin.
  - Tarayıcı ayarlarınızdan donanım ivmesini açın: `Ayarlar → Sistem → Kullanılabilir olduğunda grafik ivmesini kullan`.
  - Durumu kontrol etmek için tarayıcınızın adres çubuğuna `chrome://gpu` yazın ve WebGPU durumunun aktif olduğunu doğrulayın.
- **Bellek Yetersizliği / Sekme Çökmesi (Out of Memory / "Hay Aksi!"):**
  Tarayıcınızdaki diğer ağır sekmeleri ve arka planda RAM tüketen uygulamaları kapatın. Ardından düşük bellek modunu kullanın:
  `http://localhost:8000/?low=1&fps=30`
- **İndirme Hatası veya Kopması:**
  `Download-Game.cmd` dosyasını yeniden çalıştırın. `curl` aracı kaldığı bayttan indirmeye devam eder; doğrulanmış arşivler tekrar indirilmez.
- **Windows SmartScreen / Güvenlik Uyarısı:**
  Toplu iş ve PowerShell betikleri imzasız olduğu için Windows uyarı verebilir. Betikler açık kaynaklıdır; sağ tıklayıp kodlarını inceleyebilirsiniz. Çalıştırmak için **Ek bilgi → Yine de çalıştır** seçeneğini kullanabilirsiniz.

---

## Kapsam, Köken ve Yasal Uyarı

- **Paket İçeriği:** Bu paket yalnızca orijinal yerel ayna (mirror) dosyalarını, optimize yerel sunucuyu, indirme/başlatma/doğrulama betiklerini ve taşınabilir minimal Python çalışma ortamını içerir. Barındırma kimlik bilgileri, Netlify/Cloudflare yapılandırmaları, kişisel kayıt dosyaları (save files), Ollama veya henüz tamamlanmamış deneysel polis modu bu pakette **bulunmaz**.
- **Bağımsızlık Bildirisi:** Bu projenin Rockstar Games, Take-Two Interactive veya orijinal oyunun yapımcılarıyla hiçbir ticari ya da resmi bağı yoktur; onlar tarafından desteklenmemekte veya onaylanmamaktadır.
- **Telif Hakları:** Oyun adları, ticari markalar, görseller, sesler ve tüm oyun içi materyaller ilgili hak sahiplerinin mülkiyetindedir. Bu projede söz konusu materyaller üzerinde hiçbir mülkiyet veya dağıtım lisansı iddiasında bulunulmamaktadır. İçeriklerin kullanılabilirliği hak sahiplerinin taleplerine ve GitHub politikalarına tabidir.
- **Lisanslar:** Gömülü Python çalışma ortamının lisans metnine `runtime/LICENSE.txt` dosyasından ulaşılabilir.
