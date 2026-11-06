# Kodlama Standartları

## Çalışma kuralı
- Her prompt TEK dosya ister. Dosyanın TAM kodunu ver; "...", "geri kalanı aynı", placeholder yasak.
- Açıklama yazma. Çıktı: dosya yolu (ilk satır yorum) + kod + "TEST" başlığı altında en fazla 5 satır test adımı.
- Önceki dosyalarla imza uyumsuzluğu görürsen kod yazmadan önce tek cümleyle uyar.
- Bir hata düzeltmesi istenirse yalnızca değişen fonksiyon/blok verilir.

## PHP
- PHP 8.2+, her dosyada declare(strict_types=1), namespace App\..., PSR-4, PSR-12 biçim
- Tüm parametre ve dönüş tipleri yazılır; readonly/constructor promotion kullanılabilir
- Veritabanı: yalnızca PDO prepared statements, ATTR_EMULATE_PREPARES=false, ERRMODE_EXCEPTION, her sorgu parametreli
- Çoklu yazma işlemleri transaction içinde; stok güncelleme SELECT ... FOR UPDATE ile
- Kod/sınıf/tablo/sütun adları İngilizce, yorumlar ve arayüz metinleri Türkçe
- Tablo adları snake_case çoğul, sınıflar PascalCase, metotlar camelCase

## Katman sorumlulukları
- Controller: istek al, doğrula, servisi çağır, yanıt döndür. İş mantığı ve SQL YOK
- Service: iş kuralları, transaction
- Model: tek tablo erişimi (CRUD + sorgular). HTML YOK
- View: yalnızca sunum, çıktı her zaman e() ile kaçışlanır
- Middleware: auth, rol/permission, CSRF, rate limit

## Yanıt formatı
- AJAX uç noktaları JSON: {"ok": true, "data": {...}} veya {"ok": false, "error": {"code": "...", "message": "..."}}
- Doğrulama hataları 422, yetkisiz 401/403, bulunamadı 404, sunucu hatası 500 (kullanıcıya ayrıntı sızdırma, loga yaz)

## Frontend
- Bootstrap 5, mobile-first, erişilebilir (label, aria, odak), Vanilla JS modülleri (fetch + CSRF başlığı X-CSRF-Token)
- Satır içi JS/CSS yok (CSP için); dosya assets/ altında