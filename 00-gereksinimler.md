# YÖRESEL MARKET: Gereksinimler (MVP)

## Proje
Tek satıcılı, Türkiye'ye yönelik, fiziksel yöresel gıda e-ticaret sitesi. Para birimi TL. Dil Türkçe. Mobil öncelikli (mobile-first). Sunucu: Apache/XAMPP, PHP 8.2+, MySQL/MariaDB.

## Roller
- admin: tam yetki (kullanıcı, rol, ayar, kupon, banner, menü, silme, iade, log)
- manager: sipariş takibi ve durum güncelleme, kargo bilgisi girme, ürün/kategori/stok ekleme ve güncelleme. Silme, iade, kullanıcı/rol/ayar/kupon/log YOK
- customer: alışveriş, hesap, sipariş takibi
Yetkiler roles/permissions/role_permissions tablolarıyla yönetilir; kodda rol adı değil permission kodu kontrol edilir.

## Katalog
- Kategori (üst/alt), ürün, varyant (gramaj/boyut) bazlı SKU, fiyat, stok
- Gıdaya özel alanlar: yöre/menşei, ağırlık (gram), raf ömrü bilgisi, saklama koşulu, içindekiler, alerjen metni, üretici
- Ürün görselleri (çoklu), indirimli fiyat

## Stok
- Varyant bazlı stok. Sipariş oluşturulunca stok rezerve/düşer, iptal veya başarısız ödemede iade edilir
- Ürün/varyant bazlı kritik stok eşiği; dashboard'da uyarı
- Tüm hareketler stock_movements tablosuna yazılır

## Fiyat kuralları
- İndirimli fiyat, kupon (yüzde/sabit, min. sepet tutarı, toplam ve kullanıcı başı kullanım limiti, tarih aralığı), ücretsiz kargo eşiği (ayarlardan)
- Fiyat ve toplamlar HER ZAMAN sunucuda hesaplanır; istemciden gelen fiyat dikkate alınmaz

## Sepet ve sipariş
- Misafir sepeti (oturum), girişte kullanıcı sepetiyle birleşir
- Checkout: adres, fatura tipi (bireysel/kurumsal), kargo ücreti hesabı, kupon, ödeme yöntemi, mesafeli satış sözleşmesi ve KVKK onayı (onay zamanı ve IP kaydedilir)
- Sipariş no insan okunur (örn. YM-2026-000123), PayTR için ayrı alfanümerik merchant_oid
- Sipariş durumları: pending_payment, paid, preparing, shipped, delivered, cancelled, refunded. Her değişiklik order_status_history'ye yazılır
- order_items fiyat/ürün adı anlık görüntüsü (snapshot) tutar

## Ödeme
- Kredi/banka kartı: PayTR iFrame API (bildirim URL'si ile onay)
- Havale/EFT: sipariş pending_payment, admin dekont kontrolüyle onaylar
- Kapıda ödeme: opsiyonel kapıda ödeme ücreti
- Kart verisi asla sistemde saklanmaz

## Kargo
Manuel: admin kargo firması + takip numarası girer; müşteri hesabında ve e-postada takip bağlantısı görür.

## Arayüz
- Banner (admin yönetimli, sıralı, zamanlanabilir, masaüstü/mobil ayrı görsel) ve çok seviyeli menü (mega menü, mobilde offcanvas) önceliklidir
- Sticky arama, sepet rozeti, duyuru şeridi (ör. ücretsiz kargo eşiği)
- Bootstrap 5, Vanilla JS (Fetch)

## Admin panel
Dashboard (günlük satış, bekleyen sipariş, kritik stok), ürün/varyant/stok, kategori, sipariş + durum + kargo, havale onayı, kupon, banner, menü, CMS sayfaları, site ayarları, kullanıcı/rol, audit log.

## Yasal (MVP'ye dahil)
KVKK aydınlatma, çerez onayı, mesafeli satış sözleşmesi, iade/iptal metni, gıda için cayma istisnası açıklaması.

## MVP DIŞI (v2)
Yorum/puan, favoriler, e-fatura, kargo API, terk edilmiş sepet, 2FA, çoklu dil.