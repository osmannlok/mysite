# Klasör Yapısı ve Ad Alanları

Kök: C:\xampp\htdocs\yoresel-market  (Apache DocumentRoot = .../yoresel-market/public, VirtualHost: yoresel.local)
Ad alanı: App\  →  app/  (composer.json PSR-4)

yoresel-market/
  app/
    Core/            Config, Database, Router, Request, Response, Session, Csrf, Validator, View, Auth, Logger
    Middleware/      AuthMiddleware, PermissionMiddleware, CsrfMiddleware, RateLimitMiddleware
    Models/          BaseModel + tablo başına model
    Services/        iş mantığı (AuthService, CartService, OrderService, StockService, CouponService, ShippingCalculator, MailService, ...)
    Services/Payment/  PaymentGatewayInterface, PayTRGateway, BankTransferGateway, CodGateway, PaymentService
    Controllers/Front/   vitrin ve müşteri controller'ları
    Controllers/Admin/   yönetim paneli controller'ları
    Helpers/         functions.php (e(), url(), asset(), csrf_field(), money(), old(), flash())
  config/            app.php, database.php, paytr.php (değerler .env'den)
  database/          01_auth_rbac.sql ... 06_seed.sql
  public/            index.php, .htaccess, assets/{css,js,img}, uploads/ (+ kendi .htaccess)
  resources/views/   layouts/, partials/, front/, account/, admin/, emails/, errors/
  routes/            web.php, admin.php
  storage/           logs/, cache/ (web'den erişilemez)
  .env  .env.example  composer.json

Kurallar: Controller'lar Front/Admin ayrı; view yolları noktalı ad ile çağrılır (örn. view('front.cart.index', $data)); admin route'ları /yonetim önekiyle; hesap alanı /hesabim.