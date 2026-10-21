# Aponun Köyü

**Anadolu'da tarım, geçim derdi ve köy siyaseti.**

Aponun Köyü; tarımı, ticareti ve hikâye görevlerini Anadolu köyü atmosferinde buluşturan, Unity ile geliştirilen birinci şahıs bir oyun projesidir. Tarladaki işlerden köylülerle kurulan ilişkilere uzanan gündelik yaşam, siyasi taşlama ve karakter odaklı anlatıyla bir araya gelir.

Oyunun merkezinde üretmek, ürününü satmak ve köyde kendi yolunu çizmek vardır. Görevler, diyaloglar ve sinematik sahneler bu yaşam döngüsüne bağlanır.

> **Durum:** Aktif geliştirme aşamasında. Bu depo oyun kaynaklarını, prototipleri ve geliştirme araçlarını içerir; tamamlanmış bir oyun sürümü değildir.

**Unity 6** · **C#** · **ScriptableObject** · **TextMeshPro**

## Oynanış ve sistemler

| Sistem | Kapsam |
| --- | --- |
| **Tarım** | Tohum ekimi, sulama, oyun gününe bağlı büyüme aşamaları, kuruma ve hasat. |
| **Envanter ve ticaret** | Slot ve hızlı erişim çubuğu, eşya yığınları, sürükle bırak arayüzü ve ürün satışı. |
| **Ekonomi** | `Muhasebeci` üzerinden bakiye, ürün fiyatları ve satış kesintileri için altyapı. |
| **Görevler** | Ana görevler ve adımlar; satın alma, satış, hasat, konuşma, konuma gitme, gün bekleme ve seçim koşulları. |
| **Diyalog ve sinematikler** | Sesli altyazılar, satır gecikmeleri, kamera ve fade geçişleri; görev veya bölge tetikleyicilerine bağlı sahneler. |
| **Araçlar** | WheelCollider tabanlı sürüş, yapılandırılabilir araç fiziği, binme/inme ve tarım aracı etkileşimleri. |
| **Günlük yaşam prototipleri** | Gün döngüsüne bağlı futbol kuponu; iki maç, 1/X/2 seçimi ve kombine oran hesabı. |
| **Kayıt sistemi** | Manuel kayıt/yükleme ve otomatik kayıt için ortak giriş; oyuncu, görev, envanter ve dünya verilerinin entegrasyonu sürüyor. |

Seçim adımları ve oyun durumu anahtarları mevcut. Seçimlerin hikâyedeki etkileri, sahne bağlantıları ve kayıt bütünlüğü geliştirme sürecinin bir parçası.

## Geliştirme görüntüleri

Aşağıdaki kayıtlar farklı geliştirme aşamalarını gösterir; son görsel kaliteyi veya bütün sistemlerin güncel durumunu temsil etmeyebilir.

<p align="center">
  <img src="Images/trade.gif" alt="Ürün satışı ve ticaret arayüzünün erken geliştirme kaydı" width="800" />
</p>

<details>
<summary><strong>Tarım ve tarla etkileşimleri</strong></summary>

### Hasat

<img src="Images/collect.gif" alt="Olgunlaşan ürünlerin toplanması" width="800" />

### Tarla ve araç prototipi

<img src="Images/build.gif" alt="Tarla ve araç etkileşimlerinin geliştirme kaydı" width="800" />

</details>

<details>
<summary><strong>Envanter</strong></summary>

<img src="Images/inventory.gif" alt="Envanterde eşya taşıma ve slot etkileşimleri" width="800" />

</details>

<details>
<summary><strong>NPC etkileşimleri ve konum görevleri</strong></summary>

### NPC etkileşimi

<img src="Images/npc.gif" alt="NPC ile etkileşim ve diyalog prototipi" width="800" />

### Konuma bağlı görevler

<img src="Images/location.gif" alt="Belirli bir konuma ulaşınca ilerleyen görev adımı" width="800" />

</details>

## Projeyi açma

1. Depoyu bilgisayarına klonla:

   ```bash
   git clone https://github.com/thetruth2003/aponun_resmi_koyu-main.git
   ```

2. Unity Hub'da **Add / Add project from disk** ile depo kökünü seç.
3. Unity sürümü için [ProjectVersion.txt](ProjectSettings/ProjectVersion.txt) dosyasını esas al. Depoda kayıtlı sürüm `6000.2.6f2`; aktif yerel geliştirmede `6000.5.0f1` sürümüne geçiş yapılıyor. Geçişteki uyumluluk düzenlemeleri henüz bu deponun tamamına yansımamış olabilir.
4. İlk açılışta paketlerin çözülmesini ve asset import işlemlerinin tamamlanmasını bekle.
5. Menü akışı için `Assets/Scenes/Main Menu.unity` sahnesini aç. Sistemleri doğrudan incelemek için aşağıdaki test sahnesini kullan.

| Sahne | Kullanım |
| --- | --- |
| `Assets/Scenes/Main Menu.unity` | Ana menü ve oyuna giriş akışı. |
| `Assets/Scenes/main/MAİN_TEST.unity` | Sistemlerin birlikte denendiği aktif test/sandbox sahnesi. Dosya adındaki `İ` karakterine dikkat et. |
| `Assets/Scenes/main/aponun orjinal koyu.unity` | Diğer ana köy sahnesi. |

`MAİN_TEST` bir geliştirme sahnesidir. Eski ve yeni bileşenler bir arada bulunduğundan sahne referansları, aktif objeler ve başlangıç akışı kontrol gerektirebilir.

## Varsayılan kontroller

| Girdi | İşlev |
| --- | --- |
| `W A S D` / yön tuşları | Hareket |
| Fare | Etrafa bakma |
| `Sol Shift` | Koşma |
| `Space` | Zıplama |
| `E` | Etkileşim |
| Sol tık | Seçili araca veya hedefe göre toplama ve tarım işlemi |
| `Tab` / `F` | Envanteri açma/kapatma |
| `Q` basılı tutma | Radial menü |
| `1–9` | Hızlı erişim çubuğu |
| `Esc` | Duraklatma menüsü |
| `V` / `L` | Manuel kayıt / son kaydı yükleme |

Kontroller, sahnedeki etkin bileşenlere ve Inspector ayarlarına bağlı olarak değişebilir.

## Kodda nereden başlamalı?

Oynanış verileri için ScriptableObject asset'leri, sahne akışı için MonoBehaviour bileşenleri kullanılır. Görev, diyalog ve eşya içerikleri ilgili editörler ve Inspector üzerinden düzenlenir.

| Alan | Başlangıç noktaları |
| --- | --- |
| Görevler ve seçimler | [QuestSystem](Assets/Scripts/QuestSystem), [GameStateTracker.cs](Assets/Scripts/dictionary/GameStateTracker.cs) |
| Diyalog | [diyolog](Assets/Scripts/diyolog) |
| Sinematik akışı | [cutscene](Assets/Scripts/cutscene), [CutsceneActivator.cs](Assets/Scripts/QuestSystem/CutsceneActivator.cs) |
| Tarım | [farm](Assets/Scripts/farm) |
| Ekonomi ve kupon | [market](Assets/Scripts/market) |
| Oyuncu, envanter ve etkileşim | [player](Assets/Scripts/player) |
| Güncel araç kontrolü | [VehicleController.cs](Assets/Scripts/cars/yeni%20car/VehicleController.cs), [VehicleConfig.cs](Assets/Scripts/cars/yeni%20car/VehicleConfig.cs) |
| Kayıt ve yükleme | [SaveCoordinator.cs](Assets/Scripts/save/SaveCoordinator.cs) |

Görev düzenleyicisine Unity menüsünden **Window → Quest Editor** ile ulaşılabilir. Görev tanımları `QuestEditorAsset` ve `QuestContainer` üzerinden tutulur; adımlar ortak `IQuestStep` arayüzünü kullanır.

`SaveCoordinator`, kayıt/yükleme akışının ana girişidir. Dünya kaydının bir bölümü hâlâ `savetest` üzerinden yürür; mevcut sistem JSON dosyaları ve PlayerPrefs kullanır. Eski para ve kayıt bileşenlerini tek akışta toplama çalışması sürüyor.

## Geliştirme odağı

- Görev, diyalog ve sinematik zincirlerini köydeki gerçek karakter ve konumlarla tamamlamak.
- Oyuncu seçimlerinin sonraki görev ve diyaloglara etkisini genişletmek.
- Para ve kayıt sistemlerini tek kaynağa bağlamak; kayıt/yükleme davranışını sahne ve araç geçişleriyle doğrulamak.
- Araç modellerini, fizik ayarlarını ve binme/inme akışını iyileştirmek.
- Performansı sahne ölçümleriyle değerlendirip oynanış ve görsel sunumu geliştirmek.

---

Geliştiren: [thetruth2003](https://github.com/thetruth2003)
