+++
date = '2025-10-30T17:20:45+03:00'
draft = true
title = 'Spirit Notes'
+++

### Bu siteyi kurmamın sebebi nedir?

Ücretsiz senkronizasyon _sunan_ ve bütün problemlerime cevap bulan bir not uygulaması bulamamamdan kaynaklıydı. Bu beni ücretsiz, notlarımı yükleyebildiğim, kişiselleştirebileceğim bir site yapmaya itti.

### Bu siteyi nasıl kurdum?

Her zaman Linux distrolarından birine geçmek istemiştim. Madem hafif kodlama öğrenmem lazım diyerekten, bir de Windows kasmaları da eklenince bilgisayarıma Arch Linux tabanlı **CachyOS** distrosunu kurdum. Linux'ta terminal kullanarak site kurmanın daha kolay olacağını düşündüm.

Arch Linux, Debian/Ubuntu tabanlı distroların kullandığı paket yöneticilerini (package managers) kullanmıyor. **`pacman`** ve **`yay`** kullanıyor.

`pacman` ve `yay` arasındaki fark:

- `pacman`, **resmi** paket yöneticisidir. Arch'ın resmi depolarındaki yazılımları kurar.
    
- `yay` ise bir **AUR Yardımcısıdır**. AUR (Arch User Repository) yani "Arch Kullanıcı Deposu" demektir. Arch kullanıcıları topluluğu tarafından binlerce ekstra paketi (resmi olmayan, beta sürümler) içerir.
    

`yay` kullanarak aşağıdaki yazılımları bilgisayarımıza yükledik:

1. **Obsidian:** Markdown formatında not tutmamı sağlayan bir note-taking app.
    
2. **Hugo:** Markdown formatlı notlarımı, web sitesine (`.html` formatına) dönüştüren bir motor.
    
3. **Git/GitHub:** Bu dosyaları kaydeden ve bulutta (GitHub) depolayan bir "sürüm kontrol" sistemi.
    
4. **Netlify:** GitHub'daki sitemizi alıp yayınlayan ücretsiz bir "hosting" hizmeti.
    

### İlk adımımız Hugo

Hugo'yu kurduktan sonra terminalde `hugo new site spirit-notes` komutunu çalıştırdık.

Bu komut, temel iskeletimizi oluşturdu. `spirit-notes` diye bir klasör oluşturdu ve içine şunları koydu:

- `archetypes/`: Yeni yazı `.md` dosyaları oluştururken kullanılacak şablonlar.
    
- `content/`: Yazılarımızın (`.md` dosyaları) yaşadığı yer.
    
- `data/`: Veri dosyaları.
    
- `layouts/`: **Normalde boş gelir.** Burası bizim "kendi temamızı" yaratacağımız yerdi.
    
- `static/`: Resimler, CSS dosyaları gibi "statik" dosyaların yeri.
    
- `themes/`: **Normalde boş gelir.** Burası başkalarının temalarını koyacağımız yerdi (ama biz bunu kullanmamaya karar verdik).
    
- `hugo.toml`: Sitenin ana yapılandırma dosyası.
    

Sitemizi oluşturduk. Bundan sonra tema kurmamız lazımdı. Google'a `hugo themes` yazınca onlarca tema olduğunu gördüm ama ben hiçbirini beğenemedim. Özelleştirmeye çalıştım ama problemler yaşadım. Ben de kendi temamı yapmaya karar verdim.

- **`layouts/_default/`:** Hugo, bir tema (`themes/`) bulamadığında, HTML şablonları için otomatik olarak `layouts/_default/` klasörüne bakar. Biz de ona "İşte senin şablonların burada!" demiş olduk.
    
- **`assets/`:** Bu klasör, Hugo'nun "Hugo Pipes" adını verdiği bir sistemle **işlemesi (process)** gereken dosyalar içindir. Bizim `spirit.scss` dosyamız gibi dosyalar buraya konur, böylece Hugo onu normal CSS'e dönüştürebilir. (Resimler gibi _işlenmeyen_ dosyalar ise `static/` klasörüne konur).
    

Bu klasörleri oluşturduk. Sonra da terminalde `touch layouts/_default/baseof.html`, `touch layouts/_default/list.html` ve `touch layouts/_default/single.html` kodlarını çalıştırıp tuğlaları dizdik.

1. **`baseof.html` (Ana Çerçeve):**
    
    - Bu, bizim ana HTML şablonumuzdur. Sitedeki _her_ sayfanın etrafını saran bir "resim çerçevesi" gibi düşün.
        
    - İçinde `<html>`, `<head>` (stil dosyalarımızı bağladığımız yer), `<header>` (Spirit Notes başlığı) ve `<footer>` (Tüm hakları saklıdır) gibi tekrar eden kodlar bulunur.
        
    - En önemli görevi, `{{ block "main" . }}` adında bir "boş alan" tanımlamaktır.
        
2. **`list.html` (Yazı Listesi):**
    
    - Bu dosya, ana sayfa (`/`) veya `posts` bölümü gibi bir yazı listesi görüntülendiğinde, `baseof.html`'deki o "boş alanın" içine girer.
        
    - Görevi, `range` komutuyla tüm yazıları dolaşıp (döngüye girip) bizim "kartlarımızı" oluşturmaktır.
        
3. **`single.html` (Tekil Yazı):**
    
    - Bu dosya, bir yazının _kendisine_ tıkladığımızda `baseof.html`'deki "boş alanın" içine girer.
        
    - Görevi, yazının başlığını (`.Title`), asıl içeriğini (`.Content`) ve İçindekiler Tablosu'nu (`.TableOfContents`) göstermektir.