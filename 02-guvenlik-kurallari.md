# Güvenlik Kuralları (her dosyada uygulanır)

## Girdi/çıktı
1. Tüm SQL parametreli (PDO prepared). Dinamik sütun/sıralama ADI sadece beyaz liste (allowlist) ile.
2. Tüm girdi sunucuda doğrulanır (tip, uzunluk, biçim). İstemci doğrulaması yalnızca kolaylık.
3. Tüm çıktı e() (htmlspecialchars ENT_QUOTES|ENT_SUBSTITUTE, UTF-8) ile kaçışlanır. HTML kabul edilen alanlar (CMS) için ayrı temizleme/beyaz liste.
4. Mass assignment yok: modeller doldurulabilir alan listesi (fillable) ile çalışır.

## Oturum ve kimlik
5. Parola: password_hash(PASSWORD_ARGON2ID), min 8 karakter; giriş sonrası ve yetki değişiminde session_regenerate_id(true).
6. Cookie: HttpOnly, Secure (HTTPS'te), SameSite=Lax, strict mode, boşta zaman aşımı + mutlak ömür.
7. Giriş denemeleri login_attempts'e yazılır; IP+e-posta bazlı kilit (örn. 5 hata/15 dk). Giriş/şifre sıfırlama mesajları kullanıcı varlığını sızdırmaz.
8. Şifre sıfırlama/e-posta doğrulama token'ları rastgele (random_bytes), DB'de yalnızca SHA-256 hash'i, tek kullanımlık, süreli.

## Yetki
9. Her yönetim uç noktasında permission kontrolü (middleware). Varsayılan: reddet.
10. IDOR: kullanıcıya ait kayıt (sipariş, adres, sepet) her sorguda user_id ile sınırlanır.
11. Yönetici (manager) işlemleri de audit_logs'a yazılır: kim, hangi kayıt, eski/yeni değer, IP, zaman. Silme = soft delete.

## İstek bütünlüğü
12. CSRF: tüm POST/PUT/DELETE'lerde token (form alanı veya X-CSRF-Token). İSTİSNA yalnızca PayTR bildirim URL'si (onun yerine hash doğrulaması).
13. Güvenlik başlıkları: Content-Security-Policy (satır içi script yok), X-Content-Type-Options: nosniff, X-Frame-Options: SAMEORIGIN, Referrer-Policy, Permissions-Policy; HTTPS'te HSTS.
14. Hız sınırı: giriş, kayıt, şifre sıfırlama, kupon deneme, arama uç noktalarında.

## Ödeme ve sipariş
15. Tutar, kargo, indirim, kupon HER ZAMAN sunucuda yeniden hesaplanır; istemci fiyatı yok sayılır.
16. PayTR bildirimi: hash doğrulanmadan hiçbir işlem yapılmaz; merchant_oid ile sipariş bulunur; işlem idempotent (aynı bildirim tekrar gelse çift işlem olmaz); doğrulanınca düz metin "OK" döner. Tutar siparişle eşleşmiyorsa işlem reddedilir ve loglanır.
17. Kart verisi alınmaz/saklanmaz/loglanmaz. Gizli anahtarlar yalnızca .env'de.

## Dosya yükleme
18. Uzantı + gerçek MIME (finfo) + boyut + görsel doğrulama (getimagesize); rastgele dosya adı; yükleme klasöründe PHP çalıştırma kapalı (.htaccess); orijinal ad güvenilmez.

## Genel
19. Hatalar kullanıcıya gösterilmez (display_errors=0), storage/logs'a yazılır; loglarda parola/token/kart verisi YOK.
20. .env ve storage web kökünün dışında/erişim dışında; dizin listeleme kapalı; yalnızca public/ dışarı açık.