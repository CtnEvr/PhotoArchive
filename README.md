# Memories: Photo & Video Archive (macOS) (English)

Memories is a privacy-focused photo and video archiving application developed for macOS users. It discovers media files in user-specified folders, reads their metadata, and presents this information in a calendar-based interface, allowing users to easily manage their personal archives. All data is stored locally on the user's device using Core Data and is not exposed externally.

## Core Purpose

* Discover photos and videos in folders designated by the user.
* Read metadata of media files (especially creation date).
* Securely store this information in a local Core Data database.
* Present archived media in an easy-to-navigate calendar interface, grouped by year, month, and day.
* Manage all data locally on the device, prioritizing user privacy.

## Key Technologies Used

* **Platform:** macOS
* **User Interface:** SwiftUI
* **Data Storage:** Core Data (with a local SQLite database)
* **Asynchronous Operations:** Modern Swift Concurrency (async/await, Task)
* **Language:** Swift

## Core Components and Responsibilities

* **PhotoArchiveApp.swift:** The main entry point of the application. Initializes essential services like `AppViewModel` and `PersistenceController`.
* **ContentView.swift:** Hosts the main user interface (typically a sidebar, calendar, and media grid).
* **AppViewModel.swift:** The central ViewModel managing the application's overall state. Holds information such as the selected source folder, selected date, loaded media, scanning status, and directs user interactions to the relevant services.
* **SourceManager.swift:** Manages user-added source folders (as `SourceItem`). Persists folder information (name, security-scoped bookmark data) using `UserDefaults`. Maintains an in-memory count of media for each source.
* **SyncService.swift:** Scans a specified source folder to find media files, reads their metadata via `MetadataService`, and saves or updates this information in Core Data through `PersistenceController`. Provides information on scanning progress and summary.
* **PersistenceController.swift:** Sets up and manages the Core Data stack (`NSPersistentContainer`). Contains helper functions to facilitate database operations (saving, fetching, counting, etc.).
* **MetadataService.swift:** Responsible for extracting metadata (especially creation date) from photo and video files.
* **ThumbnailService.swift:** Generates preview images (thumbnails) for media files.

## Core Data Models

* **SourceItem (Model File: SourceItem.swift):** Represents a source folder.
    * `id`: Unique identifier (UUID).
    * `name`: User-defined name.
    * `bookmarkData`: macOS bookmark data for secure and persistent access to the folder.
    * `mediaCount`: Number of non-ignored media files in this source (dynamically calculated from Core Data).
* **MediaAsset (Core Data Entity):** Represents a media file.
    * `id`: Unique identifier (UUID).
    * `sourceId`: ID of the `SourceItem` it belongs to.
    * `filename`: File name.
    * `relativePath`: Relative path within the source folder.
    * `creationDate`: Creation date of the media.
    * `mediaTypeRaw`: Raw string value indicating the media type (e.g., "image", "video", "ignored").
    * *Other potential metadata fields (e.g., `cityName`, `countryName`).*
* **IgnoredAsset (Core Data Entity):** Keeps a record of media that the user wants to remove from the archive (but not delete from disk). The `mediaTypeRaw` of matching `MediaAsset`s is updated to "ignored".

## Application Features

* **Source Folder Management:**
    * Add, name, and delete new photo/video source folders.
    * Show source folders in Finder.
* **Media Scanning and Indexing:**
    * Scanning of media files located on local or external storage.
    * Reading of metadata, especially the creation date.
    * Saving information as `MediaAsset` to Core Data.
* **Media Count Display in Sidebar:**
    * Dynamic display of the total non-ignored media count for each source folder.
    * Media count; scanning, ignoring, and file system changes are updated manually via the respective button.
* **Calendar Interface:**
    * Display of media grouped by creation date (year, month, and day).
* **Media Grid View:**
    * Listing of media for a selected day in a grid layout with thumbnails.
    * Ability to filter media files as photos and videos within the calendar interface.
* **Lightbox Functionality:**
    * Opening a large preview of a media item (photo or video) in a lightbox-style interface upon clicking.
    * Secure URL access via `SourceManager`.
    * Full-screen photo/video display.
* **Media Info Popover:**
    * A popover displaying file name, full path, size, etc., when right-clicking on a media item.
* **Media Ignoring:**
    * Allowing the user to remove media from the archive (database) without deleting the file from disk.
    * Ignored media are marked as `IgnoredAsset` and excluded from counts.
* **Scan Cancellation:**
    * Ability for the user to cancel an ongoing folder scanning process.

  /////////////////////////////////////////////////////////////////////////////////////////////////////////

  # Memories: Photo & Video Archive (macOS) (Turkish)

Memories, macOS kullanıcıları için geliştirilmiş, yerel gizliliğe odaklanan bir fotoğraf ve video arşivleme uygulamasıdır. Kullanıcıların belirlediği klasörlerdeki medya dosyalarını keşfeder, meta verilerini okur ve bu bilgileri takvim tabanlı bir arayüzde sunarak kişisel arşivlerini kolayca yönetmelerini sağlar. Tüm veriler kullanıcının kendi cihazında, Core Data kullanılarak yerel olarak saklanır ve dışarıya kapalıdır.

## Temel Amaç

* Kullanıcının belirlediği klasörlerdeki fotoğraf ve videoları keşfetmek.
* Medya dosyalarının meta verilerini (özellikle oluşturma tarihi) okumak.
* Bu bilgileri yerel bir Core Data veritabanında güvenli bir şekilde saklamak.
* Arşivlenen medyaları yıl, ay ve gün bazında gruplandırılmış, gezinmesi kolay bir takvim arayüzünde sunmak.
* Kullanıcı gizliliğini ön planda tutarak tüm verileri cihazda yerel olarak yönetmek.

## Kullanılan Ana Teknolojiler

* **Platform:** macOS
* **Kullanıcı Arayüzü:** SwiftUI
* **Veri Saklama:** Core Data (yerel SQLite veritabanı ile)
* **Asenkron İşlemler:** Modern Swift Concurrency (async/await, Task)
* **Dil:** Swift

## Projenin Ana Bileşenleri ve Sorumlulukları

* **PhotoArchiveApp.swift:** Uygulamanın ana giriş noktası. `AppViewModel` ve `PersistenceController` gibi temel servisleri başlatır.
* **ContentView.swift:** Ana kullanıcı arayüzünü barındırır (genellikle kenar çubuğu, takvim ve medya ızgarası).
* **AppViewModel.swift:** Uygulamanın genel durumunu yöneten merkezi ViewModel. Seçili kaynak klasör, seçili tarih, yüklenen medyalar, tarama durumu gibi bilgileri tutar ve kullanıcı etkileşimlerini ilgili servislere yönlendirir.
* **SourceManager.swift:** Kullanıcının eklediği kaynak klasörleri (`SourceItem` olarak) yönetir. Bu klasörlerin bilgilerini (isim, güvenlik kapsamlı bookmark verisi) `UserDefaults` kullanarak kalıcı hale getirir. Her kaynak için anlık medya sayısını tutar.
* **SyncService.swift:** Belirtilen bir kaynak klasörü tarayarak medya dosyalarını bulur, `MetadataService` aracılığıyla meta verilerini okur ve bu bilgileri `PersistenceController` üzerinden Core Data'ya kaydeder veya günceller. Tarama ilerlemesi ve özeti hakkında bilgi sağlar.
* **PersistenceController.swift:** Core Data yığınını (`NSPersistentContainer`) kurar ve yönetir. Veritabanı işlemlerini (kaydetme, getirme, sayma vb.) kolaylaştıran yardımcı fonksiyonlar içerir.
* **MetadataService.swift:** Fotoğraf ve video dosyalarından meta verileri (özellikle oluşturma tarihi) çıkarmakla sorumludur.
* **ThumbnailService.swift:** Medya dosyaları için önizleme görselleri (thumbnail) oluşturur.

## Core Data Modelleri

* **SourceItem (Model Dosyası: SourceItem.swift):** Bir kaynak klasörü temsil eder.
    * `id`: Benzersiz kimlik (UUID).
    * `name`: Kullanıcının verdiği isim.
    * `bookmarkData`: Klasöre güvenli ve kalıcı erişim için macOS bookmark verisi.
    * `mediaCount`: Bu kaynakta bulunan (yoksayılmamış) medya dosyası sayısı (Core Data'dan dinamik olarak hesaplanır).
* **MediaAsset (Core Data Entity):** Bir medya dosyasını temsil eder.
    * `id`: Benzersiz kimlik (UUID).
    * `sourceId`: Ait olduğu `SourceItem`'ın ID'si.
    * `filename`: Dosya adı.
    * `relativePath`: Kaynak klasör içindeki göreceli yolu.
    * `creationDate`: Medyanın oluşturulma tarihi.
    * `mediaTypeRaw`: Medya tipini ("image", "video", "ignored") belirten ham string değeri.
    * *Diğer potansiyel meta veri alanları (örn: `cityName`, `countryName`).*
* **IgnoredAsset (Core Data Entity):** Kullanıcı tarafından arşivden kaldırılması istenen (ancak diskten silinmeyen) medyaların kaydını tutar. Eşleşen `MediaAsset`'lerin `mediaTypeRaw` değeri "ignored" olarak güncellenir.

## Uygulama Özellikleri

* **Kaynak Klasör Yönetimi:**
    * Yeni fotoğraf/video kaynak klasörleri ekleme, isimlendirme, silme.
    * Kaynak klasörleri Finder'da gösterme.
* **Medya Tarama ve İndeksleme:**
    * Lokal veya harici depolamada bulunan medya dosyalarının taranması.
    * Oluşturma tarihi başta olmak üzere meta verilerin okunması.
    * Bilgilerin `MediaAsset` olarak Core Data'ya kaydedilmesi.
* **Kenar Çubuğunda Medya Sayısı Gösterimi:**
    * Her kaynak klasör için yoksayılmamış toplam medya sayısının dinamik olarak gösterilmesi.
    * Medya sayısı; tarama, yoksayma ve dosya sistemi değişiklikleri ilgili butonla manuel olarak güncellenir.
* **Takvim Arayüzü:**
    * Medyaların oluşturma tarihlerine göre yıl, ay ve gün bazında gruplandırılmış gösterimi.
* **Medya Izgarası (Grid View):**
    * Seçilen bir güne ait medyaların önizlemeleriyle (thumbnail) ızgara düzeninde listelenmesi.
    * Medya dosyalarını fotoğraf ve video olarak takvim arayüzünde filtreleyebilme.
* **Lightbox İşlevselliği:**
    * Bir medya öğesine tıklandığında büyük önizlemesinin (fotoğraf veya video) lightbox benzeri bir arayüzde açılması.
    * `SourceManager` üzerinden güvenli URL erişimi.
    * Tam ekran fotoğraf/video gösterimi.
* **Medya Bilgisi Popover'ı:**
    * Bir medyaya sağ tıklandığında dosya adı, tam yolu, boyutu gibi bilgilerin gösterildiği popover.
* **Medya Yoksayma:**
    * Medyayı arşivden (veritabanından) kaldırma (dosya diskten silinmez).
    * Yoksayılan medyalar `IgnoredAsset` olarak işaretlenir ve sayımlara dahil edilmez.
* **Tarama İptali:**
    * Devam eden bir klasör tarama işlemini iptal etme özelliği.
