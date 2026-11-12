<?php
/**
 * KiraBöl - Ev & Kira Paylaşım Platformu (1. PARÇA)
 * PHP 8.3 + PostgreSQL + Leaflet Harita
 * Gizli Admin Kısayolu: Ctrl + Shift + A (Şifre: Admin123)
 */

const DB_HOST = '127.0.0.1';
const DB_PORT = '5432';
const DB_NAME = 'kirabol_db';
const DB_USER = 'kirabol_user';
const DB_PASS = 'GizliSifreniz123';

const BADGE_PRICE      = 200; // TL (30 Günlük Rozet)
const PLUS_PRICE       = 199; // TL (30 Günlük KiraBöl Plus+)
const STUDENT_PRICE    = 79;  // TL (30 Günlük KiraBöl Öğrenci+)
const EARLY_BIRD_PRICE = 99;  // TL (48 Saat Erken Erişim VIP)
const BOOST_PRICE      = 49;  // TL (Tek Tıkla Başa Al Dopingi)
const BADGE_DAYS       = 30;
const MAX_PHOTOS       = 10;

date_default_timezone_set('Europe/Istanbul');
session_cache_limiter('');
session_set_cookie_params(['lifetime' => 0, 'path' => '/', 'httponly' => true, 'samesite' => 'Lax']);
session_start();
if (empty($_SESSION['csrf'])) $_SESSION['csrf'] = bin2hex(random_bytes(16));
if (!isset($_SESSION['viewed'])) $_SESSION['viewed'] = [];

/* ------------------------------------------------ DB & MİGRASYON */
function db(): PDO {
    static $pdo = null;
    if ($pdo) return $pdo;
    $pdo = new PDO('pgsql:host=' . DB_HOST . ';port=' . DB_PORT . ';dbname=' . DB_NAME, DB_USER, DB_PASS, [
        PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION,
        PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC
    ]);
    $pdo->exec("SET TIME ZONE 'Europe/Istanbul'");
    if (isset($_GET['migrate']) && $_GET['migrate'] === '1') {
        migrate($pdo);
    }
    return $pdo;
}

function migrate(PDO $pdo): void {
    $pdo->exec("
    CREATE TABLE IF NOT EXISTS users (
        id SERIAL PRIMARY KEY,
        name VARCHAR(80) NOT NULL,
        email VARCHAR(160) NOT NULL UNIQUE,
        phone VARCHAR(30) NOT NULL DEFAULT '',
        pass_hash VARCHAR(255) NOT NULL,
        badge_until TIMESTAMPTZ NULL,
        created_at TIMESTAMPTZ NOT NULL DEFAULT NOW()
    );
    ALTER TABLE users ADD COLUMN IF NOT EXISTS avatar_id INT NULL;
    ALTER TABLE users ADD COLUMN IF NOT EXISTS is_admin BOOLEAN NOT NULL DEFAULT FALSE;
    ALTER TABLE users ADD COLUMN IF NOT EXISTS is_banned BOOLEAN NOT NULL DEFAULT FALSE;
    ALTER TABLE users ADD COLUMN IF NOT EXISTS is_verified BOOLEAN NOT NULL DEFAULT FALSE;
    ALTER TABLE users ADD COLUMN IF NOT EXISTS ban_reason TEXT NOT NULL DEFAULT '';
    ALTER TABLE users ADD COLUMN IF NOT EXISTS warnings_count INT NOT NULL DEFAULT 0;
    ALTER TABLE users ADD COLUMN IF NOT EXISTS lifestyle JSONB NOT NULL DEFAULT '{}'::jsonb;
    ALTER TABLE users ADD COLUMN IF NOT EXISTS instagram VARCHAR(60) NOT NULL DEFAULT '';
    ALTER TABLE users ADD COLUMN IF NOT EXISTS linkedin VARCHAR(120) NOT NULL DEFAULT '';
    ALTER TABLE users ADD COLUMN IF NOT EXISTS early_bird_until TIMESTAMPTZ NULL;
    ALTER TABLE users ADD COLUMN IF NOT EXISTS last_seen TIMESTAMPTZ DEFAULT NOW();

    CREATE TABLE IF NOT EXISTS listings (
        id SERIAL PRIMARY KEY,
        user_id INT NOT NULL REFERENCES users(id) ON DELETE CASCADE,
        title VARCHAR(140) NOT NULL,
        type VARCHAR(30) NOT NULL,
        city VARCHAR(60) NOT NULL,
        district VARCHAR(60) NOT NULL DEFAULT '',
        price INT NOT NULL,
        rooms VARCHAR(10) NOT NULL DEFAULT '',
        description TEXT NOT NULL DEFAULT '',
        lat DOUBLE PRECISION NULL,
        lng DOUBLE PRECISION NULL,
        views INT NOT NULL DEFAULT 0,
        active BOOLEAN NOT NULL DEFAULT TRUE,
        created_at TIMESTAMPTZ NOT NULL DEFAULT NOW()
    );
    ALTER TABLE listings ADD COLUMN IF NOT EXISTS address TEXT NOT NULL DEFAULT '';
    ALTER TABLE listings ADD COLUMN IF NOT EXISTS status VARCHAR(20) NOT NULL DEFAULT 'approved';
    ALTER TABLE listings ADD COLUMN IF NOT EXISTS lifestyle JSONB NOT NULL DEFAULT '{}'::jsonb;
    ALTER TABLE listings ADD COLUMN IF NOT EXISTS amenities JSONB NOT NULL DEFAULT '[]'::jsonb;
    ALTER TABLE listings ADD COLUMN IF NOT EXISTS find_type VARCHAR(30) NOT NULL DEFAULT 'has_home';
    ALTER TABLE listings ADD COLUMN IF NOT EXISTS user_age INT NULL;
    ALTER TABLE listings ADD COLUMN IF NOT EXISTS occupation VARCHAR(50) NOT NULL DEFAULT 'Öğrenci';
    ALTER TABLE listings ADD COLUMN IF NOT EXISTS alcohol VARCHAR(30) NOT NULL DEFAULT 'Fark Etmez';
    ALTER TABLE listings ADD COLUMN IF NOT EXISTS metro_station VARCHAR(80) NOT NULL DEFAULT '';

    CREATE TABLE IF NOT EXISTS payments (
        id SERIAL PRIMARY KEY,
        user_id INT NOT NULL REFERENCES users(id) ON DELETE CASCADE,
        amount INT NOT NULL,
        card_last4 CHAR(4) NOT NULL,
        card_brand VARCHAR(20) NOT NULL,
        status VARCHAR(20) NOT NULL DEFAULT 'success',
        kind VARCHAR(30) NOT NULL DEFAULT 'badge',
        created_at TIMESTAMPTZ NOT NULL DEFAULT NOW()
    );
    CREATE TABLE IF NOT EXISTS images (
        id SERIAL PRIMARY KEY,
        user_id INT NOT NULL REFERENCES users(id) ON DELETE CASCADE,
        listing_id INT NULL REFERENCES listings(id) ON DELETE CASCADE,
        kind VARCHAR(10) NOT NULL DEFAULT 'listing',
        mime VARCHAR(30) NOT NULL,
        data BYTEA NOT NULL,
        created_at TIMESTAMPTZ NOT NULL DEFAULT NOW()
    );
    CREATE TABLE IF NOT EXISTS messages (
        id SERIAL PRIMARY KEY,
        listing_id INT NULL REFERENCES listings(id) ON DELETE SET NULL,
        sender_id INT NOT NULL REFERENCES users(id) ON DELETE CASCADE,
        receiver_id INT NOT NULL REFERENCES users(id) ON DELETE CASCADE,
        body TEXT NOT NULL,
        created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
        read_at TIMESTAMPTZ NULL
    );
    CREATE TABLE IF NOT EXISTS favorites (
        user_id INT NOT NULL REFERENCES users(id) ON DELETE CASCADE,
        listing_id INT NOT NULL REFERENCES listings(id) ON DELETE CASCADE,
        created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
        PRIMARY KEY (user_id, listing_id)
    );
    CREATE TABLE IF NOT EXISTS reports (
        id SERIAL PRIMARY KEY,
        reporter_id INT NOT NULL REFERENCES users(id) ON DELETE CASCADE,
        listing_id INT NULL REFERENCES listings(id) ON DELETE CASCADE,
        reported_user_id INT NULL REFERENCES users(id) ON DELETE CASCADE,
        reason VARCHAR(80) NOT NULL,
        details TEXT NOT NULL DEFAULT '',
        status VARCHAR(20) NOT NULL DEFAULT 'pending',
        created_at TIMESTAMPTZ NOT NULL DEFAULT NOW()
    );
    CREATE TABLE IF NOT EXISTS notifications (
        id SERIAL PRIMARY KEY,
        user_id INT NOT NULL REFERENCES users(id) ON DELETE CASCADE,
        title VARCHAR(120) NOT NULL,
        body TEXT NOT NULL,
        is_read BOOLEAN NOT NULL DEFAULT FALSE,
        created_at TIMESTAMPTZ NOT NULL DEFAULT NOW()
    );
    CREATE TABLE IF NOT EXISTS reviews (
        id SERIAL PRIMARY KEY,
        reviewer_id INT NOT NULL REFERENCES users(id) ON DELETE CASCADE,
        target_user_id INT NOT NULL REFERENCES users(id) ON DELETE CASCADE,
        rating INT NOT NULL CHECK (rating BETWEEN 1 AND 5),
        comment TEXT NOT NULL,
        created_at TIMESTAMPTZ NOT NULL DEFAULT NOW()
    );
    CREATE TABLE IF NOT EXISTS price_alerts (
        user_id INT NOT NULL REFERENCES users(id) ON DELETE CASCADE,
        listing_id INT NOT NULL REFERENCES listings(id) ON DELETE CASCADE,
        tracked_price INT NOT NULL,
        created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
        PRIMARY KEY (user_id, listing_id)
    );
    CREATE TABLE IF NOT EXISTS blocks (
        blocker_id INT NOT NULL REFERENCES users(id) ON DELETE CASCADE,
        blocked_id INT NOT NULL REFERENCES users(id) ON DELETE CASCADE,
        created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
        PRIMARY KEY (blocker_id, blocked_id)
    );");

    $adminPass = password_hash('Admin123', PASSWORD_DEFAULT);
    $pdo->prepare("INSERT INTO users(name, email, phone, pass_hash, is_admin, is_verified) VALUES(?, ?, ?, ?, TRUE, TRUE) ON CONFLICT (email) DO UPDATE SET is_admin=TRUE, pass_hash=EXCLUDED.pass_hash, is_verified=TRUE")
        ->execute(['Sistem Yöneticisi', 'admin@kirabol.com', '0555 000 00 00', $adminPass]);

    if ((int)$pdo->query("SELECT COUNT(*) FROM listings")->fetchColumn() === 0) seed($pdo);
}

function notify(PDO $pdo, int $uid, string $title, string $body): void {
    $pdo->prepare("INSERT INTO notifications(user_id, title, body) VALUES(?, ?, ?)")->execute([$uid, $title, $body]);
}

function seed(PDO $pdo): void {
    $hash = password_hash('123456', PASSWORD_DEFAULT);
    $u = $pdo->prepare("INSERT INTO users(name, email, phone, pass_hash, badge_until, is_verified, lifestyle, instagram, linkedin) VALUES(?, ?, ?, ?, ?, ?, ?::jsonb, ?, ?) ON CONFLICT (email) DO UPDATE SET name = EXCLUDED.name RETURNING id");
    $u->execute(['Ayşe Yılmaz', 'ayse@ornek.kirabol', '0532 000 00 01', $hash, date('c', time() + 86400 * 20), true, json_encode(['smoking'=>'Hayır','alcohol'=>'Hayır','pet'=>'Kabul','occupation'=>'Çalışan','user_age'=>25,'gender'=>'Kadın','city'=>'İstanbul']), 'ayseyilmaz', 'ayse-yilmaz']);
    $a = (int)$u->fetchColumn();
    $u->execute(['Hasan Emir Kaya', 'hasan@ornek.kirabol', '0533 000 00 02', $hash, null, true, json_encode(['smoking'=>'Hayır','alcohol'=>'Hayır','pet'=>'Kabul','occupation'=>'Öğrenci','user_age'=>21,'gender'=>'Erkek','city'=>'İstanbul']), 'hasanemir', '']);
    $b = (int)$u->fetchColumn();
    $u->execute(['Talip Demir', 'talip@ornek.kirabol', '0535 000 00 03', $hash, null, false, json_encode(['smoking'=>'Evet','alcohol'=>'Evet','pet'=>'Yok','occupation'=>'Çalışan','user_age'=>28,'gender'=>'Erkek','city'=>'İstanbul']), '', 'talipdemir']);
    $c = (int)$u->fetchColumn();
    
    $l = $pdo->prepare("INSERT INTO listings(user_id,title,type,city,district,address,price,rooms,description,lat,lng,views,status,lifestyle,amenities,find_type,user_age,occupation,alcohol,metro_station) VALUES(?,?,?,?,?,?,?,?,?,?,?,?,'approved',?::jsonb,?::jsonb,?,?,?,?,?)");
    $ls1 = json_encode(['smoking'=>'Hayır','pet'=>'Kabul','gender'=>'Fark Etmez']);
    $ls2 = json_encode(['smoking'=>'Hayır','pet'=>'Kabul','gender'=>'Erkek']);
    $ls3 = json_encode(['smoking'=>'Evet','pet'=>'Yok','gender'=>'Erkek']);
    $am1 = json_encode(['Tam Eşyalı', 'Fiber İnternet', 'Metroya Yakın', 'Doğalgaz Kombi', 'Balkon']);
    $am2 = json_encode(['Tam Eşyalı', 'Metroya Yakın', 'Fiber İnternet']);
    
    $rows = [
        [$a, 'Moda sahilinde ferah güneşli 2+1 daire', 'Kiralık Daire', 'İstanbul', 'Kadıköy', 'Moda Caddesi', 32000, '2+1', 'Kadıköy Moda sahilinde daireme temiz ve saygılı ev arkadaşı arıyorum.', 40.9877, 29.0270, 120, $ls1, $am1, 'has_home', 25, 'Çalışan', 'Sosyal', 'Kadıköy Metrosu 5 dk'],
        [$b, 'Fatih civarı evine ortak olabileceğim ev arkadaşı arıyorum', 'Ev Arkadaşı', 'İstanbul', 'Fatih', 'Fatih Merkez', 8500, '1 oda', 'İstanbul Üniversitesi öğrencisiyim. Bütçem 8.500 TL civarı. Birlikte eve çıkacak arkadaş arıyorum.', 41.0182, 28.9484, 94, $ls2, '[]', 'looking_home', 21, 'Öğrenci', 'Hayır', 'M1 Emniyet Durağı'],
        [$c, 'Ataşehirde 2+1 eşyalı evime çalışan ev arkadaşı', 'Kiralık Oda', 'İstanbul', 'Ataşehir', 'Ataşehir Batı', 11000, '1 oda', 'Yazılım firmasında çalışıyorum. Saygılı ve düzenli bir ev arkadaşı arıyorum.', 40.9922, 29.1244, 76, $ls3, $am2, 'has_home', 28, 'Çalışan', 'Evet', 'M8 Metrosu 8 dk'],
    ];
    foreach ($rows as $r) $l->execute($r);
}

/* ------------------------------------------------ YARDIMCILAR */
function out(array $data, int $code = 200): never {
    http_response_code($code);
    echo json_encode($data, JSON_UNESCAPED_UNICODE | JSON_UNESCAPED_SLASHES);
    exit;
}
function body(): array {
    $j = json_decode((string)file_get_contents('php://input'), true);
    return is_array($j) ? $j : [];
}
function str(mixed $v, int $max): string {
    return mb_substr(trim((string)$v), 0, $max);
}
function need_post(): void {
    if ($_SERVER['REQUEST_METHOD'] !== 'POST') out(['error' => 'Geçersiz istek.'], 405);
    $t = $_SERVER['HTTP_X_CSRF'] ?? '';
    if (!hash_equals($_SESSION['csrf'], $t)) out(['error' => 'Oturum doğrulaması başarısız, sayfayı yenileyin.'], 403);
}
function uid(): ?int { return isset($_SESSION['uid']) ? (int)$_SESSION['uid'] : null; }
function need_user(): int {
    $id = uid();
    if (!$id) out(['error' => 'Bu işlem için giriş yapmalısınız.'], 401);
    $s = db()->prepare("SELECT is_banned, ban_reason FROM users WHERE id=?");
    $s->execute([$id]);
    $u = $s->fetch();
    if ($u && $u['is_banned']) {
        unset($_SESSION['uid']);
        out(['error' => 'Hesabınız askıya alınmıştır: ' . ($u['ban_reason'] ?: 'Kural ihlali.')], 403);
    }
    db()->prepare("UPDATE users SET last_seen=NOW() WHERE id=?")->execute([$id]);
    return $id;
}
function is_admin(): bool {
    $id = uid();
    if (!$id) return false;
    $s = db()->prepare("SELECT is_admin FROM users WHERE id=?");
    $s->execute([$id]);
    return (bool)$s->fetchColumn();
}
function need_admin(): int {
    $me = need_user();
    if (!is_admin()) out(['error' => 'Bu işlem için yönetici yetkisi gerekir.'], 403);
    return $me;
}

function public_user(int $id): ?array {
    $s = db()->prepare("SELECT id,name,email,phone,badge_until,created_at,avatar_id,is_admin,is_banned,is_verified,ban_reason,warnings_count,lifestyle,instagram,linkedin,early_bird_until,last_seen,
        plan_type, plan_until, student_status, student_doc_id, student_doc_info, student_reject_reason,
        (badge_until IS NOT NULL AND badge_until > NOW()) AS badge,
        (early_bird_until IS NOT NULL AND early_bird_until > NOW()) AS is_early_bird,
        (plan_until IS NOT NULL AND plan_until > NOW()) AS is_plan_active,
        GREATEST(0, CEIL(EXTRACT(EPOCH FROM (badge_until - NOW())) / 86400))::int AS badge_days,
        (SELECT COUNT(*) FROM messages m WHERE m.receiver_id = users.id AND m.read_at IS NULL AND m.body NOT LIKE '[CALL_SIGNAL:%') AS unread,
        (SELECT COUNT(*) FROM notifications n WHERE n.user_id = users.id AND n.is_read = FALSE) AS unread_notifs,
        (SELECT COUNT(*) FROM favorites f WHERE f.user_id = users.id) AS fav_count,
        (SELECT ROUND(AVG(rating),1) FROM reviews r WHERE r.target_user_id = users.id) AS avg_rating,
        (SELECT COUNT(*) FROM reviews r WHERE r.target_user_id = users.id) AS review_count
        FROM users WHERE id=?");
    $s->execute([$id]);
    $u = $s->fetch();
    if (!$u) return null;
    $u['badge'] = (bool)$u['badge'];
    $u['is_early_bird'] = (bool)$u['is_early_bird'];
    $u['is_plan_active'] = (bool)$u['is_plan_active'];
    $u['badge_days'] = $u['badge'] ? (int)$u['badge_days'] : 0;
    $u['unread'] = (int)$u['unread'];
    $u['unread_notifs'] = (int)($u['unread_notifs'] ?? 0);
    $u['fav_count'] = (int)$u['fav_count'];
    $u['is_admin'] = (bool)($u['is_admin'] ?? false);
    $u['is_banned'] = (bool)($u['is_banned'] ?? false);
    $u['is_verified'] = (bool)($u['is_verified'] ?? false);
    $u['warnings_count'] = (int)($u['warnings_count'] ?? 0);
    $u['avg_rating'] = $u['avg_rating'] !== null ? (float)$u['avg_rating'] : null;
    $u['review_count'] = (int)($u['review_count'] ?? 0);
    $u['lifestyle'] = !empty($u['lifestyle']) ? (is_array($u['lifestyle']) ? $u['lifestyle'] : json_decode($u['lifestyle'], true)) : [];
    $u['student_doc_info'] = !empty($u['student_doc_info']) ? (is_array($u['student_doc_info']) ? $u['student_doc_info'] : json_decode($u['student_doc_info'], true)) : [];
    $u['plan_type'] = (string)($u['plan_type'] ?? 'free');
    $u['student_status'] = (string)($u['student_status'] ?? 'none');
    $u['avatar'] = $u['avatar_id'] ? (int)$u['avatar_id'] : null;
    $u['student_doc_id'] = $u['student_doc_id'] ? (int)$u['student_doc_id'] : null;
    unset($u['avatar_id']);
    return $u;
}

function luhn(string $n): bool {
    $sum = 0; $alt = false;
    for ($i = strlen($n) - 1; $i >= 0; $i--) {
        $d = (int)$n[$i];
        if ($alt) { $d *= 2; if ($d > 9) $d -= 9; }
        $sum += $d; $alt = !$alt;
    }
    return $sum % 10 === 0;
}
function card_brand(string $n): string {
    if (strlen($n) === 0) return 'Kart';
    if ($n[0] === '4') return 'Visa';
    if (preg_match('/^(5[1-5]|2[2-7])/', $n)) return 'Mastercard';
    if (preg_match('/^9792/', $n)) return 'Troy';
    if (preg_match('/^3[47]/', $n)) return 'Amex';
    return 'Kart';
}

function listing_row(array $r): array {
    $r['badge'] = (bool)($r['badge'] ?? false);
    $r['is_fav'] = (bool)($r['is_fav'] ?? false);
    $r['is_verified'] = (bool)($r['is_verified'] ?? false);
    $r['id'] = (int)$r['id']; $r['user_id'] = (int)$r['user_id'];
    $r['listing_no'] = 10000000 + (int)$r['id'];
    $r['price'] = (int)$r['price']; $r['views'] = (int)$r['views'];
    $r['lat'] = $r['lat'] !== null ? (float)$r['lat'] : null;
    $r['lng'] = $r['lng'] !== null ? (float)$r['lng'] : null;
    $r['cover'] = isset($r['cover']) ? (int)$r['cover'] : null;
    $r['img_count'] = (int)($r['img_count'] ?? 0);
    $r['status'] = (string)($r['status'] ?? 'approved');
    $r['find_type'] = (string)($r['find_type'] ?? 'has_home');
    $r['user_age'] = $r['user_age'] !== null ? (int)$r['user_age'] : null;
    $r['occupation'] = (string)($r['occupation'] ?? 'Belirtilmemiş');
    $r['alcohol'] = (string)($r['alcohol'] ?? 'Belirtilmemiş');
    $r['metro_station'] = (string)($r['metro_station'] ?? '');
    $r['owner_avatar'] = isset($r['owner_avatar']) ? (int)$r['owner_avatar'] : null;
    $r['owner_rating'] = $r['owner_rating'] !== null ? (float)$r['owner_rating'] : null;
    $r['lifestyle'] = !empty($r['lifestyle']) ? (is_array($r['lifestyle']) ? $r['lifestyle'] : json_decode($r['lifestyle'], true)) : [];
    $r['amenities'] = !empty($r['amenities']) ? (is_array($r['amenities']) ? $r['amenities'] : json_decode($r['amenities'], true)) : [];
    
    // Erken Erişim (Early Bird) VIP Kontrolü (48 saat)
    $createdAtTs = strtotime($r['created_at']);
    $r['is_early_bird_locked'] = ($r['find_type'] !== 'looking_home') && (time() - $createdAtTs < 48 * 3600);
    $r['early_bird_remaining_hours'] = max(0, ceil((48 * 3600 - (time() - $createdAtTs)) / 3600));

    unset($r['phone'], $r['active']);
    return $r;
}

const LISTING_COLS = "l.id,l.user_id,l.title,l.type,l.city,l.district,l.address,l.price,l.rooms,l.description,l.lat,l.lng,l.views,l.status,l.created_at,l.lifestyle,l.amenities,l.find_type,l.user_age,l.occupation,l.alcohol,l.metro_station,
    u.name AS owner, u.is_verified, u.last_seen, u.instagram, u.linkedin, (u.badge_until IS NOT NULL AND u.badge_until > NOW()) AS badge,
    (SELECT ROUND(AVG(rating),1) FROM reviews rw WHERE rw.target_user_id = u.id) AS owner_rating,
    CASE WHEN u.badge_until > NOW() THEN u.avatar_id END AS owner_avatar,
    (SELECT i.id FROM images i WHERE i.listing_id = l.id ORDER BY i.id LIMIT 1) AS cover,
    (SELECT COUNT(*) FROM images i WHERE i.listing_id = l.id) AS img_count";

function files_of(string $key): array {
    if (empty($_FILES[$key])) return [];
    $f = $_FILES[$key]; $out = [];
    if (is_array($f['name'])) {
        foreach ($f['name'] as $i => $n) $out[] = ['tmp' => $f['tmp_name'][$i], 'size' => $f['size'][$i], 'err' => $f['error'][$i]];
    } else {
        $out[] = ['tmp' => $f['tmp_name'], 'size' => $f['size'], 'err' => $f['error']];
    }
    return array_values(array_filter($out, fn($x) => $x['err'] !== UPLOAD_ERR_NO_FILE));
}

function detect_upload_mime(string $tmpPath): ?string {
    if (!file_exists($tmpPath)) return null;

    // 1. GD getimagesize (JPG, PNG, GIF, WebP için finfo gerektirmez)
    $imgInfo = @getimagesize($tmpPath);
    if ($imgInfo && !empty($imgInfo['mime'])) {
        return $imgInfo['mime'];
    }

    // 2. Magic bytes kontrolü (PDF ve WebP için)
    $h = @fopen($tmpPath, 'rb');
    if ($h) {
        $header = fread($h, 16);
        fclose($h);
        if (str_starts_with($header, '%PDF-')) {
            return 'application/pdf';
        }
        if (str_starts_with($header, 'RIFF') && str_contains($header, 'WEBP')) {
            return 'image/webp';
        }
    }

    // 3. finfo sınıfı varsa kullan
    if (class_exists('finfo')) {
        $fi = new finfo(FILEINFO_MIME_TYPE);
        $m = $fi->file($tmpPath);
        if ($m) return $m;
    }

    // 4. mime_content_type fonksiyonu varsa kullan
    if (function_exists('mime_content_type')) {
        $m = @mime_content_type($tmpPath);
        if ($m) return $m;
    }

    return null;
}

function store_image(PDO $pdo, int $me, ?int $listing, string $kind, array $f): int {
    if ($f['err'] !== UPLOAD_ERR_OK) throw new RuntimeException('Dosya yüklenemedi.');
    if ($f['size'] > 10 * 1024 * 1024) throw new RuntimeException('Dosya en fazla 10 MB olabilir.');
    
    $mime = detect_upload_mime($f['tmp']);
    $allowed = ['image/jpeg', 'image/png', 'image/webp', 'application/pdf'];
    if (!$mime || !in_array($mime, $allowed, true))
        throw new RuntimeException('Sadece JPG, PNG, WebP veya PDF formatları desteklenmektedir.');

    $fp = fopen($f['tmp'], 'rb');
    $st = $pdo->prepare("INSERT INTO images(user_id,listing_id,kind,mime,data) VALUES(:u,:l,:k,:m,:d) RETURNING id");
    $st->bindValue(':u', $me, PDO::PARAM_INT);
    $st->bindValue(':l', $listing, $listing === null ? PDO::PARAM_NULL : PDO::PARAM_INT);
    $st->bindValue(':k', $kind);
    $st->bindValue(':m', $mime);
    $st->bindValue(':d', $fp, PDO::PARAM_LOB);
    $st->execute();
    if (is_resource($fp)) fclose($fp);
    return (int)$st->fetchColumn();
}

/* ------------------------------------------------ GÖRSEL SERVİSİ */
if (isset($_GET['img'])) {
    session_write_close();
    try {
        $s = db()->prepare("SELECT mime,data FROM images WHERE id=?");
        $s->execute([(int)$_GET['img']]);
        $r = $s->fetch();
    } catch (Throwable $e) { $r = false; }
    if (!$r) { http_response_code(404); exit; }
    $d = is_resource($r['data']) ? stream_get_contents($r['data']) : (string)$r['data'];
    header('Content-Type: ' . $r['mime']);
    header('Content-Length: ' . strlen($d));
    header('Cache-Control: public, max-age=31536000, immutable');
    echo $d;
    exit;
}

/* ------------------------------------------------ GERÇEKÇİ DİNAMİK UYUM MOTORU */
function calculate_match(array $myLs, array $item): array {
    $myCity = trim(mb_strtolower($myLs['city'] ?? ''));
    $itemCity = trim(mb_strtolower($item['city'] ?? ''));
    $mySmoking = $myLs['smoking'] ?? '';
    $itemSmoking = $item['lifestyle']['smoking'] ?? '';
    $myAlcohol = $myLs['alcohol'] ?? '';
    $itemAlcohol = $item['alcohol'] ?? '';
    $myOcc = $myLs['occupation'] ?? '';
    $itemOcc = $item['occupation'] ?? '';
    $myAge = isset($myLs['user_age']) && is_numeric($myLs['user_age']) ? (int)$myLs['user_age'] : null;
    $itemAge = isset($item['user_age']) && is_numeric($item['user_age']) ? (int)$item['user_age'] : null;

    $score = 10;
    $reasons = [];

    if ($myCity !== '' && $itemCity !== '') {
        if ($myCity === $itemCity) { $score += 25; $reasons[] = 'Aynı Şehir (' . $item['city'] . ')'; }
        else { $score -= 10; }
    } else { $score += 10; }

    if ($mySmoking !== '' && $itemSmoking !== '') {
        if ($mySmoking === $itemSmoking) { $score += 25; $reasons[] = 'Sigara Tercihi Uyumlu (' . $mySmoking . ')'; }
        elseif (($mySmoking === 'Hayır' || $mySmoking === 'İçilmez') && ($itemSmoking === 'Evet' || $itemSmoking === 'İçilebilir')) { $score -= 20; }
        else { $score += 10; }
    } else { $score += 12; }

    if ($myAlcohol !== '' && $itemAlcohol !== '') {
        if ($myAlcohol === $itemAlcohol) { $score += 15; $reasons[] = 'Alkol Kullanımı Uyumlu'; }
        elseif ($myAlcohol === 'Sosyal' || $itemAlcohol === 'Sosyal') { $score += 8; }
        else { $score -= 5; }
    } else { $score += 8; }

    if ($myOcc !== '' && $itemOcc !== '') {
        if ($myOcc === $itemOcc) { $score += 15; $reasons[] = 'İkiniz de ' . $myOcc; }
        else { $score += 6; }
    } else { $score += 8; }

    $prefGender = $item['lifestyle']['gender'] ?? 'Fark Etmez';
    $myGender = $myLs['gender'] ?? '';
    if ($prefGender === 'Fark Etmez' || $myGender === '' || $prefGender === $myGender) { $score += 10; }
    else { $score -= 15; }

    if ($myAge !== null && $itemAge !== null) {
        $diff = abs($myAge - $itemAge);
        if ($diff <= 2) { $score += 10; $reasons[] = 'Yaş Farkı ' . $diff; }
        elseif ($diff <= 5) { $score += 7; }
        else { $score -= 8; }
    } else { $score += 5; }

    $finalScore = (int)max(18, min(99, $score));
    return ['score' => $finalScore, 'reasons' => $reasons];
}

/* ------------------------------------------------ API ROUTER */
if (isset($_GET['api'])) {
    header('Content-Type: application/json; charset=utf-8');
    header('Cache-Control: no-store');
    try {
        $pdo = db();
        $act = (string)$_GET['api'];

        if ($act === 'me') {
            $id = uid();
            out(['user' => $id ? public_user($id) : null]);
        }

        if ($act === 'admin_quick_login') {
            need_post(); $d = body();
            $rawPass = trim((string)($d['password'] ?? ''));
            $cleanPass = strtolower(str_replace(' ', '', $rawPass));
            $s = $pdo->query("SELECT id, pass_hash FROM users WHERE is_admin = TRUE ORDER BY id ASC LIMIT 1");
            $admin = $s->fetch();
            $isMaster = ($cleanPass === 'admin123');
            $isHashValid = $admin && password_verify($rawPass, $admin['pass_hash']);
            if (!$admin || (!$isMaster && !$isHashValid)) out(['error' => 'Geçersiz yönetici şifresi.'], 401);
            if ($isMaster) $pdo->prepare("UPDATE users SET pass_hash=? WHERE id=?")->execute([password_hash('Admin123', PASSWORD_DEFAULT), $admin['id']]);
            session_regenerate_id(true);
            $_SESSION['uid'] = (int)$admin['id'];
            out(['user' => public_user((int)$admin['id'])]);
        }

        if ($act === 'register') {
            need_post(); $d = body();
            $name = str($d['name'] ?? '', 80); $email = mb_strtolower(str($d['email'] ?? '', 160));
            $phone = str($d['phone'] ?? '', 30); $pass = (string)($d['password'] ?? '');
            if (mb_strlen($name) < 2) out(['error' => 'Ad soyad en az 2 karakter olmalı.'], 422);
            if (!filter_var($email, FILTER_VALIDATE_EMAIL)) out(['error' => 'Geçerli bir e-posta girin.'], 422);
            if (strlen($pass) < 6) out(['error' => 'Şifre en az 6 karakter olmalı.'], 422);
            $c = $pdo->prepare("SELECT 1 FROM users WHERE email=?"); $c->execute([$email]);
            if ($c->fetch()) out(['error' => 'Bu e-posta ile zaten bir hesap var.'], 409);
            $i = $pdo->prepare("INSERT INTO users(name,email,phone,pass_hash) VALUES(?,?,?,?) RETURNING id");
            $i->execute([$name, $email, $phone, password_hash($pass, PASSWORD_DEFAULT)]);
            session_regenerate_id(true);
            $_SESSION['uid'] = (int)$i->fetchColumn();
            notify($pdo, $_SESSION['uid'], 'Hoş Geldiniz!', 'KiraBöl platformuna başarıyla katıldınız.');
            out(['user' => public_user(uid())]);
        }

        if ($act === 'login') {
            need_post(); $d = body();
            $s = $pdo->prepare("SELECT id,pass_hash,is_banned,ban_reason FROM users WHERE email=?");
            $s->execute([mb_strtolower(str($d['email'] ?? '', 160))]);
            $u = $s->fetch();
            if (!$u || !password_verify((string)($d['password'] ?? ''), $u['pass_hash'])) out(['error' => 'E-posta veya şifre hatalı.'], 401);
            if ($u['is_banned']) out(['error' => 'Hesabınız askıya alınmıştır: ' . ($u['ban_reason'] ?: 'Kural ihlali.')], 403);
            session_regenerate_id(true);
            $_SESSION['uid'] = (int)$u['id'];
            out(['user' => public_user((int)$u['id'])]);
        }

        if ($act === 'logout') {
            need_post();
            unset($_SESSION['uid']);
            session_regenerate_id(true);
            out(['ok' => true]);
        }

        if ($act === 'profile_update') {
            need_post(); $me = need_user(); $d = body();
            $name = str($d['name'] ?? '', 80);
            $phone = str($d['phone'] ?? '', 30);
            $instagram = str($d['instagram'] ?? '', 60);
            $linkedin = str($d['linkedin'] ?? '', 120);
            $pass = (string)($d['password'] ?? '');
            if (mb_strlen($name) < 2) out(['error' => 'Ad soyad en az 2 karakter olmalı.'], 422);

            $lifestyle = json_encode([
                'smoking'    => str($d['smoking'] ?? 'Hayır', 30),
                'alcohol'    => str($d['alcohol'] ?? 'Hayır', 30),
                'pet'        => str($d['pet'] ?? 'Kabul', 30),
                'occupation' => str($d['occupation'] ?? 'Öğrenci', 40),
                'user_age'   => is_numeric($d['user_age'] ?? '') ? (int)$d['user_age'] : 22,
                'gender'     => str($d['gender'] ?? 'Erkek', 20),
                'city'       => str($d['city'] ?? 'İstanbul', 60),
            ], JSON_UNESCAPED_UNICODE);
            
            if ($pass !== '') {
                if (strlen($pass) < 6) out(['error' => 'Yeni şifre en az 6 karakter olmalı.'], 422);
                $pdo->prepare("UPDATE users SET name=?, phone=?, pass_hash=?, lifestyle=?::jsonb, instagram=?, linkedin=? WHERE id=?")->execute([$name, $phone, password_hash($pass, PASSWORD_DEFAULT), $lifestyle, $instagram, $linkedin, $me]);
            } else {
                $pdo->prepare("UPDATE users SET name=?, phone=?, lifestyle=?::jsonb, instagram=?, linkedin=? WHERE id=?")->execute([$name, $phone, $lifestyle, $instagram, $linkedin, $me]);
            }
            out(['user' => public_user($me)]);
        }

        if ($act === 'avatar_save') {
            need_post(); $me = need_user();
            $f = files_of('avatar');
            if (count($f) !== 1) out(['error' => 'Bir fotoğraf seçin.'], 422);
            $pdo->beginTransaction();
            $old = $pdo->prepare("SELECT avatar_id FROM users WHERE id=? FOR UPDATE");
            $old->execute([$me]);
            $oldId = $old->fetchColumn();
            $newId = store_image($pdo, $me, null, 'avatar', $f[0]);
            $pdo->prepare("UPDATE users SET avatar_id=? WHERE id=?")->execute([$newId, $me]);
            if ($oldId) $pdo->prepare("DELETE FROM images WHERE id=? AND user_id=? AND kind='avatar'")->execute([$oldId, $me]);
            $pdo->commit();
            out(['user' => public_user($me)]);
        }

        if ($act === 'avatar_delete') {
            need_post(); $me = need_user();
            $pdo->prepare("DELETE FROM images WHERE user_id=? AND kind='avatar'")->execute([$me]);
            $pdo->prepare("UPDATE users SET avatar_id=NULL WHERE id=?")->execute([$me]);
            out(['user' => public_user($me)]);
        }

        if ($act === 'notifications') {
            $me = need_user();
            $s = $pdo->prepare("SELECT * FROM notifications WHERE user_id=? ORDER BY created_at DESC LIMIT 30");
            $s->execute([$me]);
            $pdo->prepare("UPDATE notifications SET is_read=TRUE WHERE user_id=?")->execute([$me]);
            out(['notifications' => $s->fetchAll()]);
        }

        /* ------------------------------------------------ PARA KAZANDIRAN 1: ERKEN ERİŞİM VIP SATIN ALMA (99 TL) */
        if ($act === 'pay_early_bird') {
            need_post(); $me = need_user(); $d = body();
            $num = preg_replace('/\D/', '', (string)($d['number'] ?? ''));
            if (!luhn($num)) out(['error' => 'Kart numarası geçersiz.'], 422);

            $pdo->beginTransaction();
            $pdo->prepare("INSERT INTO payments(user_id,amount,card_last4,card_brand,status,kind) VALUES(?,?,?,?,'success','early_bird')")
                ->execute([$me, EARLY_BIRD_PRICE, substr($num, -4), card_brand($num)]);
            $pdo->prepare("UPDATE users SET early_bird_until = GREATEST(COALESCE(early_bird_until, NOW()), NOW()) + INTERVAL '30 days' WHERE id=?")->execute([$me]);
            $pdo->commit();
            notify($pdo, $me, 'VIP Erken Erişim Aktif!', '30 gün boyunca tüm yeni ev ve oda ilanlarına anında erişim hakkı kazandınız.');
            out(['user' => public_user($me)]);
        }

        /* ------------------------------------------------ PARA KAZANDIRAN 2: İLANI BAŞA AL BOOST DOPİNGİ (49 TL) */
        if ($act === 'pay_boost') {
            need_post(); $me = need_user(); $d = body();
            $lid = (int)($d['listing_id'] ?? 0);
            $num = preg_replace('/\D/', '', (string)($d['number'] ?? ''));
            $holder = str($d['holder'] ?? '', 80);
            $exp = str($d['expiry'] ?? '', 7);
            $cvv = preg_replace('/\D/', '', (string)($d['cvv'] ?? ''));

            if ($holder !== '' && mb_strlen($holder) < 3) out(['error' => 'Lütfen geçerli bir kart sahibi ismi girin.'], 422);
            if (strlen($num) < 15 || strlen($num) > 16 || !luhn($num)) out(['error' => 'Kart numarası geçersiz.'], 422);
            if ($exp !== '' && !preg_match('#^(0[1-9]|1[0-2])/\d{2}$#', $exp)) out(['error' => 'Son kullanma tarihi AA/YY formatında olmalıdır.'], 422);
            if ($cvv !== '' && (strlen($cvv) < 3 || strlen($cvv) > 4)) out(['error' => 'CVV kodu 3 veya 4 haneli olmalıdır.'], 422);

            $chk = $pdo->prepare("SELECT 1 FROM listings WHERE id=? AND user_id=?");
            $chk->execute([$lid, $me]);
            if (!$chk->fetch() && !is_admin()) out(['error' => 'Bu ilana doping uygulama yetkiniz yok.'], 403);

            $pdo->beginTransaction();
            $pdo->prepare("INSERT INTO payments(user_id,amount,card_last4,card_brand,status,kind) VALUES(?,?,?,?,'success','boost')")
                ->execute([$me, BOOST_PRICE, substr($num, -4), card_brand($num)]);
            $pdo->prepare("UPDATE listings SET created_at = NOW() WHERE id=?")->execute([$lid]);
            $pdo->commit();
            notify($pdo, $me, 'İlanınız Başa Alındı! ⚡', "#{$lid} numaralı ilanınız güncellendi ve listenin en tepesine taşındı.");
            out(['ok' => true]);
        }

        /* ------------------------------------------------ KULLANICI ENGELLEME */
        if ($act === 'user_block_toggle') {
            need_post(); $me = need_user(); $d = body();
            $target = (int)($d['target_user_id'] ?? 0);
            if ($target === $me || !$target) out(['error' => 'Geçersiz kullanıcı.'], 422);

            $chk = $pdo->prepare("SELECT 1 FROM blocks WHERE blocker_id=? AND blocked_id=?");
            $chk->execute([$me, $target]);
            if ($chk->fetch()) {
                $pdo->prepare("DELETE FROM blocks WHERE blocker_id=? AND blocked_id=?")->execute([$me, $target]);
                $blocked = false;
            } else {
                $pdo->prepare("INSERT INTO blocks(blocker_id, blocked_id) VALUES(?, ?)")->execute([$me, $target]);
                $blocked = true;
            }
            out(['blocked' => $blocked]);
        }

        /* ------------------------------------------------ FİYAT ALARMI */
        if ($act === 'price_alert_toggle') {
            need_post(); $me = need_user(); $d = body();
            $lid = (int)($d['listing_id'] ?? 0);
            $s = $pdo->prepare("SELECT price FROM listings WHERE id=?");
            $s->execute([$lid]);
            $currPrice = $s->fetchColumn();
            if (!$currPrice) out(['error' => 'İlan bulunamadı.'], 404);

            $chk = $pdo->prepare("SELECT 1 FROM price_alerts WHERE user_id=? AND listing_id=?");
            $chk->execute([$me, $lid]);
            if ($chk->fetch()) {
                $pdo->prepare("DELETE FROM price_alerts WHERE user_id=? AND listing_id=?")->execute([$me, $lid]);
                $active = false;
            } else {
                $pdo->prepare("INSERT INTO price_alerts(user_id, listing_id, tracked_price) VALUES(?,?,?)")->execute([$me, $lid, (int)$currPrice]);
                $active = true;
            }
            out(['active' => $active]);
        }

        /* ------------------------------------------------ YORUM & PUANLAMA SİSTEMİ */
        if ($act === 'review_add') {
            need_post(); $me = need_user(); $d = body();
            $target = (int)($d['target_user_id'] ?? 0);
            $rating = (int)($d['rating'] ?? 5);
            $comment = str($d['comment'] ?? '', 400);
            if ($target === $me) out(['error' => 'Kendinize yorum yazamazsınız.'], 422);
            if ($rating < 1 || $rating > 5) out(['error' => 'Geçersiz puan.'], 422);
            if (mb_strlen($comment) < 5) out(['error' => 'Lütfen en az 5 karakterlik bir yorum yazın.'], 422);

            $pdo->prepare("INSERT INTO reviews(reviewer_id, target_user_id, rating, comment) VALUES(?,?,?,?)")->execute([$me, $target, $rating, $comment]);
            notify($pdo, $target, 'Yeni Değerlendirme!', "Bir ev arkadaşı profilinize {$rating} yıldızlı bir yorum bıraktı.");
            out(['ok' => true]);
        }

        if ($act === 'user_reviews') {
            $target = (int)($_GET['user_id'] ?? 0);
            $s = $pdo->prepare("SELECT r.*, u.name AS reviewer_name, u.avatar_id FROM reviews r JOIN users u ON u.id = r.reviewer_id WHERE r.target_user_id=? ORDER BY r.created_at DESC LIMIT 20");
            $s->execute([$target]);
            out(['reviews' => $s->fetchAll()]);
        }

        /* ------------------------------------------------ LİSTELEME */
        if ($act === 'listings') {
            $w = ['l.active = TRUE', "l.status = 'approved'"]; 
            $p = [];
            $myUid = uid();
            
            $q = str($_GET['q'] ?? '', 80);
            if ($q !== '') {
                if (is_numeric($q) && (int)$q >= 10000000) {
                    $w[] = '(l.id = :qid)';
                    $p['qid'] = (int)$q - 10000000;
                } else {
                    $like = '%' . addcslashes($q, '%_\\') . '%';
                    $w[] = '(l.title ILIKE :q1 OR l.city ILIKE :q2 OR l.district ILIKE :q3 OR l.description ILIKE :q4 OR u.name ILIKE :q5)';
                    foreach (['q1', 'q2', 'q3', 'q4', 'q5'] as $k) $p[$k] = $like;
                }
            }
            if (($c = str($_GET['city'] ?? '', 60)) !== '') { $w[] = 'l.city = :city'; $p['city'] = $c; }
            if (($t = str($_GET['type'] ?? '', 30)) !== '') { $w[] = 'l.type = :type'; $p['type'] = $t; }
            if (is_numeric($_GET['min'] ?? '')) { $w[] = 'l.price >= :min'; $p['min'] = (int)$_GET['min']; }
            if (is_numeric($_GET['max'] ?? '')) { $w[] = 'l.price <= :max'; $p['max'] = (int)$_GET['max']; }
            if (($ft = str($_GET['find_type'] ?? '', 30)) !== '') { $w[] = 'l.find_type = :find_type'; $p['find_type'] = $ft; }
            if (is_numeric($_GET['start_age'] ?? '')) { $w[] = 'l.user_age >= :start_age'; $p['start_age'] = (int)$_GET['start_age']; }
            if (is_numeric($_GET['end_age'] ?? '')) { $w[] = 'l.user_age <= :end_age'; $p['end_age'] = (int)$_GET['end_age']; }
            if (($occ = str($_GET['occupation'] ?? '', 40)) !== '') { $w[] = 'l.occupation = :occupation'; $p['occupation'] = $occ; }
            if (($alc = str($_GET['alcohol'] ?? '', 30)) !== '') { $w[] = 'l.alcohol = :alcohol'; $p['alcohol'] = $alc; }
            if (($smk = str($_GET['smoking'] ?? '', 30)) !== '') { $w[] = "l.lifestyle->>'smoking' = :smoking"; $p['smoking'] = $smk; }
            if (($gnd = str($_GET['gender'] ?? '', 30)) !== '') { $w[] = "l.lifestyle->>'gender' = :gender"; $p['gender'] = $gnd; }

            if (($_GET['favs'] ?? '') === '1') {
                if (!$myUid) out(['listings' => []]);
                $w[] = 'EXISTS(SELECT 1 FROM favorites f WHERE f.listing_id = l.id AND f.user_id = :my_fav_uid)';
                $p['my_fav_uid'] = $myUid;
            }

            $sort = str($_GET['sort'] ?? '', 20);
            $orderBy = match ($sort) {
                'price_asc'  => 'badge DESC, l.price ASC, l.created_at DESC',
                'price_desc' => 'badge DESC, l.price DESC, l.created_at DESC',
                'views'      => 'badge DESC, l.views DESC, l.created_at DESC',
                default      => 'badge DESC, l.created_at DESC'
            };

            $favSelect = $myUid 
                ? ", (EXISTS(SELECT 1 FROM favorites f WHERE f.listing_id = l.id AND f.user_id = " . (int)$myUid . ")) AS is_fav"
                : ", FALSE AS is_fav";

            $sql = "SELECT " . LISTING_COLS . $favSelect . " FROM listings l JOIN users u ON u.id = l.user_id WHERE " . implode(' AND ', $w) . " ORDER BY " . $orderBy . " LIMIT 150";
            $s = $pdo->prepare($sql); $s->execute($p);
            $rows = array_map('listing_row', $s->fetchAll());

            if ($myUid) {
                $uData = public_user($myUid);
                $myLs = $uData['lifestyle'] ?? [];
                foreach ($rows as &$item) {
                    $calc = calculate_match($myLs, $item);
                    $item['match_score'] = $calc['score'];
                    $item['match_reasons'] = $calc['reasons'];
                }
                unset($item);

                if ($sort === 'smart') {
                    usort($rows, fn($a, $b) => $b['match_score'] <=> $a['match_score']);
                }
            }

            out(['listings' => $rows]);
        }

        if ($act === 'listing_view') {
            need_post(); $id = (int)(body()['id'] ?? 0);
            $myUid = uid();
            $admin = is_admin();

            $favSelect = $myUid 
                ? ", (EXISTS(SELECT 1 FROM favorites f WHERE f.listing_id = l.id AND f.user_id = " . (int)$myUid . ")) AS is_fav,
                   (EXISTS(SELECT 1 FROM price_alerts pa WHERE pa.listing_id = l.id AND pa.user_id = " . (int)$myUid . ")) AS is_alert_active"
                : ", FALSE AS is_fav, FALSE AS is_alert_active";

            $s = $pdo->prepare("SELECT " . LISTING_COLS . $favSelect . ", u.phone FROM listings l JOIN users u ON u.id=l.user_id WHERE l.id=?");
            $s->execute([$id]);
            $r = $s->fetch();
            if (!$r) out(['error' => 'İlan bulunamadı.'], 404);

            if ($r['status'] !== 'approved' && $myUid !== (int)$r['user_id'] && !$admin) {
                out(['error' => 'Bu ilan onay beklemektedir.'], 403);
            }

            if ($myUid !== (int)$r['user_id']) {
                if (!in_array($id, $_SESSION['viewed'], true)) {
                    $pdo->prepare("UPDATE listings SET views = views + 1 WHERE id=?")->execute([$id]);
                    $r['views'] = (int)$r['views'] + 1;
                    $_SESSION['viewed'][] = $id;
                }
            }
            $phone = $myUid ? $r['phone'] : null;
            $im = $pdo->prepare("SELECT id FROM images WHERE listing_id=? ORDER BY id");
            $im->execute([$id]);
            $row = listing_row($r);
            $row['phone'] = $phone;
            $row['images'] = array_map('intval', $im->fetchAll(PDO::FETCH_COLUMN));
            $row['is_alert_active'] = (bool)($r['is_alert_active'] ?? false);

            if ($myUid) {
                $uData = public_user($myUid);
                $calc = calculate_match($uData['lifestyle'] ?? [], $row);
                $row['match_score'] = $calc['score'];
                $row['match_reasons'] = $calc['reasons'];
            }

            out(['listing' => $row]);
        }

        if ($act === 'favorite_toggle') {
            need_post(); $me = need_user(); $d = body();
            $lid = (int)($d['listing_id'] ?? 0);
            if (!$lid) out(['error' => 'Geçersiz ilan.'], 422);

            $check = $pdo->prepare("SELECT 1 FROM favorites WHERE user_id=? AND listing_id=?");
            $check->execute([$me, $lid]);
            if ($check->fetch()) {
                $pdo->prepare("DELETE FROM favorites WHERE user_id=? AND listing_id=?")->execute([$me, $lid]);
                $isFav = false;
            } else {
                $pdo->prepare("INSERT INTO favorites(user_id, listing_id) VALUES(?,?) ON CONFLICT DO NOTHING")->execute([$me, $lid]);
                $isFav = true;
            }
            $cntStmt = $pdo->prepare("SELECT COUNT(*) FROM favorites WHERE user_id=?");
            $cntStmt->execute([$me]);
            out(['is_fav' => $isFav, 'fav_count' => (int)$cntStmt->fetchColumn()]);
        }

        if ($act === 'listing_save') {
            need_post(); $me = need_user();
            $id = (int)($_POST['id'] ?? 0);
            $title = str($_POST['title'] ?? '', 140); $type = str($_POST['type'] ?? '', 30);
            $city = str($_POST['city'] ?? '', 60); $dist = str($_POST['district'] ?? '', 60);
            $price = (int)($_POST['price'] ?? 0); $rooms = str($_POST['rooms'] ?? '', 10);
            $desc = str($_POST['description'] ?? '', 2000); $addr = str($_POST['address'] ?? '', 300);
            $find_type = in_array($_POST['find_type'] ?? '', ['has_home', 'looking_home', 'temporary'], true) ? $_POST['find_type'] : 'has_home';
            $user_age = is_numeric($_POST['user_age'] ?? '') ? (int)$_POST['user_age'] : null;
            $occupation = str($_POST['occupation'] ?? 'Öğrenci', 50);
            $alcohol = str($_POST['alcohol'] ?? 'Belirtilmemiş', 30);
            $metro = str($_POST['metro_station'] ?? '', 80);

            if (mb_strlen($title) < 5) out(['error' => 'Başlık en az 5 karakter olmalı.'], 422);
            if ($city === '') out(['error' => 'Şehir seçin.'], 422);
            if ($price < 1 || $price > 10000000) out(['error' => 'Geçerli bir fiyat veya bütçe girin.'], 422);
            if (!is_numeric($_POST['lat'] ?? null) || !is_numeric($_POST['lng'] ?? null)) out(['error' => 'Haritadan konumu belirleyin.'], 422);
            $lat = (float)$_POST['lat']; $lng = (float)$_POST['lng'];

            $lifestyle = json_encode([
                'smoking' => str($_POST['smoking'] ?? 'Belirtilmemiş', 30),
                'pet'     => str($_POST['pet'] ?? 'Belirtilmemiş', 30),
                'gender'  => str($_POST['gender_pref'] ?? 'Erkek', 30),
            ], JSON_UNESCAPED_UNICODE);

            $amenities = json_encode((array)($_POST['amenities'] ?? []), JSON_UNESCAPED_UNICODE);
            $initialStatus = is_admin() ? 'approved' : 'pending';

            $pdo->beginTransaction();
            if ($id) {
                $oldPriceStmt = $pdo->prepare("SELECT price FROM listings WHERE id=?");
                $oldPriceStmt->execute([$id]);
                $oldPrice = (int)$oldPriceStmt->fetchColumn();

                $c = $pdo->prepare("SELECT 1 FROM listings WHERE id=? AND (user_id=? OR ?) FOR UPDATE");
                $c->execute([$id, $me, is_admin()]);
                if (!$c->fetch()) { $pdo->rollBack(); out(['error' => 'Yetkiniz yok.'], 403); }
                $pdo->prepare("UPDATE listings SET title=?,type=?,city=?,district=?,address=?,price=?,rooms=?,description=?,lat=?,lng=?,status=?,lifestyle=?::jsonb,amenities=?::jsonb,find_type=?,user_age=?,occupation=?,alcohol=?,metro_station=? WHERE id=?")
                    ->execute([$title, $type, $city, $dist, $addr, $price, $rooms, $desc, $lat, $lng, $initialStatus, $lifestyle, $amenities, $find_type, $user_age, $occupation, $alcohol, $metro, $id]);

                if ($price < $oldPrice) {
                    $alertUsers = $pdo->prepare("SELECT user_id FROM price_alerts WHERE listing_id=?");
                    $alertUsers->execute([$id]);
                    foreach ($alertUsers->fetchAll(PDO::FETCH_COLUMN) as $alertUid) {
                        notify($pdo, (int)$alertUid, 'Fiyat Düştü! 📉', "\"{$title}\" ilanının fiyatı " . number_format($oldPrice, 0, ',', '.') . " ₺'den " . number_format($price, 0, ',', '.') . " ₺'ye indi!");
                    }
                }
            } else {
                $s = $pdo->prepare("INSERT INTO listings(user_id,title,type,city,district,address,price,rooms,description,lat,lng,status,lifestyle,amenities,find_type,user_age,occupation,alcohol,metro_station) VALUES(?,?,?,?,?,?,?,?,?,?,?,?,?::jsonb,?::jsonb,?,?,?,?,?) RETURNING id");
                $s->execute([$me, $title, $type, $city, $dist, $addr, $price, $rooms, $desc, $lat, $lng, $initialStatus, $lifestyle, $amenities, $find_type, $user_age, $occupation, $alcohol, $metro]);
                $id = (int)$s->fetchColumn();
            }
            $cnt = $pdo->prepare("SELECT COUNT(*) FROM images WHERE listing_id=?");
            $cnt->execute([$id]);
            $new = files_of('photos');
            if ((int)$cnt->fetchColumn() + count($new) > MAX_PHOTOS) throw new RuntimeException('En fazla ' . MAX_PHOTOS . ' fotoğraf ekleyebilirsiniz.');
            foreach ($new as $f) store_image($pdo, $me, $id, 'listing', $f);
            $pdo->commit();
            out(['id' => $id, 'status' => $initialStatus]);
        }

        if ($act === 'listing_delete') {
            need_post(); $me = need_user();
            $id = (int)(body()['id'] ?? 0);
            if (is_admin()) {
                $s = $pdo->prepare("DELETE FROM listings WHERE id=?");
                $s->execute([$id]);
            } else {
                $s = $pdo->prepare("DELETE FROM listings WHERE id=? AND user_id=?");
                $s->execute([$id, $me]);
            }
            out(['ok' => $s->rowCount() > 0]);
        }

        if ($act === 'report_create') {
            need_post(); $me = need_user(); $d = body();
            $lid = !empty($d['listing_id']) ? (int)$d['listing_id'] : null;
            $ruid = !empty($d['user_id']) ? (int)$d['user_id'] : null;
            $reason = str($d['reason'] ?? '', 80);
            $details = str($d['details'] ?? '', 500);
            if ($reason === '') out(['error' => 'Bir şikayet sebebi seçin.'], 422);

            $pdo->prepare("INSERT INTO reports(reporter_id, listing_id, reported_user_id, reason, details) VALUES(?,?,?,?,?)")->execute([$me, $lid, $ruid, $reason, $details]);
            out(['ok' => true]);
        }

        if ($act === 'admin_listings') {
            need_admin();
            $s = $pdo->query("SELECT " . LISTING_COLS . ", u.email AS owner_email FROM listings l JOIN users u ON u.id = l.user_id ORDER BY CASE WHEN l.status='pending' THEN 0 ELSE 1 END, l.created_at DESC");
            out(['listings' => array_map('listing_row', $s->fetchAll())]);
        }

        if ($act === 'admin_users') {
            need_admin();
            $s = $pdo->query("SELECT u.id, u.name, u.email, u.phone, u.created_at, u.is_admin, u.is_banned, u.is_verified, u.ban_reason, u.warnings_count, (u.badge_until IS NOT NULL AND u.badge_until > NOW()) AS badge, (SELECT COUNT(*) FROM listings l WHERE l.user_id = u.id) AS listing_count FROM users u ORDER BY u.id DESC");
            $users = $s->fetchAll();
            foreach ($users as &$u) {
                $u['id'] = (int)$u['id'];
                $u['is_admin'] = (bool)$u['is_admin'];
                $u['is_banned'] = (bool)$u['is_banned'];
                $u['is_verified'] = (bool)$u['is_verified'];
                $u['badge'] = (bool)$u['badge'];
                $u['warnings_count'] = (int)$u['warnings_count'];
                $u['listing_count'] = (int)$u['listing_count'];
            }
            out(['users' => $users]);
        }

        if ($act === 'admin_reports') {
            need_admin();
            $s = $pdo->query("SELECT r.*, u1.name AS reporter_name, u1.email AS reporter_email, l.title AS listing_title, u2.name AS reported_name FROM reports r JOIN users u1 ON u1.id = r.reporter_id LEFT JOIN listings l ON l.id = r.listing_id LEFT JOIN users u2 ON u2.id = r.reported_user_id ORDER BY CASE WHEN r.status='pending' THEN 0 ELSE 1 END, r.created_at DESC");
            out(['reports' => $s->fetchAll()]);
        }

        if ($act === 'admin_report_resolve') {
            need_admin(); need_post(); $d = body();
            $id = (int)($d['id'] ?? 0);
            $action = str($d['action'] ?? 'resolved', 20);
            $pdo->prepare("UPDATE reports SET status=? WHERE id=?")->execute([$action, $id]);
            out(['ok' => true]);
        }

        if ($act === 'admin_user_ban') {
            need_admin(); need_post(); $d = body();
            $id = (int)($d['id'] ?? 0);
            $status = !empty($d['ban']);
            $reason = str($d['reason'] ?? '', 255);
            if ($id === uid()) out(['error' => 'Kendi hesabınızı banlayamazsınız.'], 422);

            $pdo->prepare("UPDATE users SET is_banned=?, ban_reason=? WHERE id=?")->execute([$status, $status ? ($reason ?: 'Kurallara aykırı davranış') : '', $id]);
            if ($status) $pdo->prepare("UPDATE listings SET active=FALSE WHERE user_id=?")->execute([$id]);
            out(['ok' => true]);
        }

        if ($act === 'admin_user_verify') {
            need_admin(); need_post(); $d = body();
            $id = (int)($d['id'] ?? 0);
            $verify = !empty($d['verify']);
            $pdo->prepare("UPDATE users SET is_verified=? WHERE id=?")->execute([$verify, $id]);
            notify($pdo, $id, 'Profil Doğrulaması', $verify ? 'Mavi Tik rozetiniz aktif edildi.' : 'Mavi Tik kaldırıldı.');
            out(['ok' => true]);
        }

        if ($act === 'admin_user_warn') {
            need_admin(); need_post(); $d = body();
            $id = (int)($d['id'] ?? 0);
            $msg = str($d['message'] ?? '', 500);
            if ($msg === '') out(['error' => 'Mesaj boş olamaz.'], 422);

            $pdo->prepare("UPDATE users SET warnings_count = warnings_count + 1 WHERE id=?")->execute([$id]);
            $pdo->prepare("INSERT INTO messages(listing_id, sender_id, receiver_id, body) VALUES(NULL, ?, ?, ?)")->execute([uid(), $id, "⚠️ YÖNETİCİ UYARISI: " . $msg]);
            notify($pdo, $id, '⚠️ Yönetici Uyarısı', $msg);
            out(['ok' => true]);
        }

        /* ------------------------------------------------ YÖNETİCİ TOPLU MESAJ / DUYURU */
        if ($act === 'admin_broadcast') {
            need_admin(); need_post(); $d = body();
            $title = str($d['title'] ?? '📢 Sistem Duyurusu', 120);
            $msg = str($d['message'] ?? '', 2500);
            if (mb_strlen($title) < 2) out(['error' => 'Duyuru başlığı çok kısa.'], 422);
            if (mb_strlen($msg) < 5) out(['error' => 'Duyuru metni çok kısa.'], 422);

            $st = $pdo->prepare("INSERT INTO notifications(user_id, title, body) SELECT id, :title, :body FROM users");
            $st->execute(['title' => $title, 'body' => $msg]);
            $count = $st->rowCount();
            out(['ok' => true, 'count' => $count]);
        }

        /* ------------------------------------------------ ÖĞRENCİ BELGESİ ONAY MASASI */
        if ($act === 'admin_student_list') {
            need_admin();
            $s = $pdo->query("SELECT u.id, u.name, u.email, u.phone, u.student_status, u.student_doc_id, u.student_doc_info, u.student_reject_reason, u.created_at
                              FROM users u WHERE u.student_status != 'none'
                              ORDER BY (u.student_status = 'pending') DESC, u.id DESC");
            $rows = $s->fetchAll();
            foreach ($rows as &$r) {
                $r['id'] = (int)$r['id'];
                $r['student_doc_id'] = $r['student_doc_id'] ? (int)$r['student_doc_id'] : null;
                $r['student_doc_info'] = !empty($r['student_doc_info']) ? (is_array($r['student_doc_info']) ? $r['student_doc_info'] : json_decode($r['student_doc_info'], true)) : [];
            }
            out(['students' => $rows]);
        }

        if ($act === 'admin_student_decide') {
            need_admin(); need_post(); $d = body();
            $id = (int)($d['id'] ?? 0);
            $action = str($d['action'] ?? '', 20);
            $reason = str($d['reason'] ?? '', 500);

            if ($action === 'approve') {
                $pdo->prepare("UPDATE users SET student_status='approved', is_verified=TRUE, student_reject_reason='' WHERE id=?")->execute([$id]);
                notify($pdo, $id, 'Tebrikler! Öğrenci Statünüz Onaylandı 🎓', 'Öğrenci belgeniz doğrulandı. Artık KiraBöl Öğrenci+ (79 TL) avantajlı paketinden faydalanabilirsiniz!');
                out(['ok' => true]);
            } elseif ($action === 'reject') {
                $rejReason = $reason ?: 'Belge okunamadı veya geçerlilik süresi dolmuş.';
                $pdo->prepare("UPDATE users SET student_status='rejected', student_reject_reason=? WHERE id=?")->execute([$rejReason, $id]);
                notify($pdo, $id, 'Öğrenci Belgeniz Onaylanamadı ❌', 'Öğrenci belgeniz onaylanmadı: ' . $rejReason . '. Lütfen güncel belgenizi profilinizden tekrar yükleyin.');
                out(['ok' => true]);
            }
            out(['error' => 'Geçersiz işlem.'], 400);
        }

        /* ------------------------------------------------ ÖĞRENCİ BELGESİ YÜKLEME */
        if ($act === 'student_verify_submit') {
            need_post(); $me = need_user();
            $uni = str($_POST['university'] ?? '', 120);
            $dept = str($_POST['department'] ?? '', 120);
            $barcode = str($_POST['barcode'] ?? '', 80);
            $note = str($_POST['note'] ?? '', 500);

            if (mb_strlen($uni) < 2) out(['error' => 'Lütfen üniversite adınızı girin.'], 422);
            if (mb_strlen($dept) < 2) out(['error' => 'Lütfen bölüm / fakülte bilginizi girin.'], 422);
            if (mb_strlen($barcode) < 4) out(['error' => 'Lütfen e-Devlet belgenizin üzerindeki resmi barkod numarasını girin.'], 422);

            $files = files_of('document');
            if (empty($files)) out(['error' => 'Lütfen öğrenci belgenizi (PDF veya Fotoğraf) seçin.'], 422);

            $docId = store_image($pdo, $me, null, 'student_doc', $files[0]);
            $info = json_encode([
                'university' => $uni,
                'department' => $dept,
                'barcode' => $barcode,
                'note' => $note,
                'submitted_at' => date('d.m.Y H:i')
            ], JSON_UNESCAPED_UNICODE);

            $pdo->prepare("UPDATE users SET student_status='pending', student_doc_id=?, student_doc_info=?::jsonb, student_reject_reason='' WHERE id=?")
                ->execute([$docId, $info, $me]);
            notify($pdo, $me, 'Öğrenci Belgeniz Alındı 🎓', 'Belgeniz onay için moderasyon ekibimize iletildi. e-Devlet barkodu üzerinden kontrol edilip onaylanacaktır.');
            out(['ok' => true, 'user' => public_user($me)]);
        }

        /* ------------------------------------------------ ÜYELİK PAKETİ SATIN ALMA (PLUS+ & ÖĞRENCİ+) */
        if ($act === 'pay_plan') {
            need_post(); $me = need_user(); $d = body();
            $plan = str($d['plan'] ?? 'plus', 20);
            if (!in_array($plan, ['plus', 'student'], true)) out(['error' => 'Geçersiz üyelik planı.'], 400);

            $uData = public_user($me);
            if ($plan === 'student' && ($uData['student_status'] ?? '') !== 'approved') {
                out(['error' => 'Öğrenci+ paketi için önce onaylanmış öğrenci belgenizin olması gerekmektedir.'], 403);
            }

            $holder = str($d['holder'] ?? '', 80);
            $num = preg_replace('/\D/', '', (string)($d['number'] ?? ''));
            $exp = str($d['expiry'] ?? '', 7);
            $cvv = preg_replace('/\D/', '', (string)($d['cvv'] ?? ''));

            if (mb_strlen($holder) < 3) out(['error' => 'Kart üzerindeki ismi girin.'], 422);
            if (strlen($num) < 15 || strlen($num) > 16 || !luhn($num)) out(['error' => 'Kart numarası geçersiz.'], 422);
            if (!preg_match('~^(0[1-9]|1[0-2])\s*/\s*(\d{2})$~', $exp)) out(['error' => 'Son kullanma tarihi AA/YY olmalı.'], 422);
            if (!preg_match('/^\d{3,4}$/', $cvv)) out(['error' => 'CVV 3 veya 4 haneli olmalı.'], 422);

            $amount = ($plan === 'student') ? STUDENT_PRICE : PLUS_PRICE;
            $planTitle = ($plan === 'student') ? 'KiraBöl Öğrenci+ 🎓' : 'KiraBöl Plus+ ⭐';

            $pdo->beginTransaction();
            $pdo->prepare("INSERT INTO payments(user_id,amount,card_last4,card_brand,status,kind) VALUES(?,?,?,?,'success',?)")
                ->execute([$me, $amount, substr($num, -4), card_brand($num), 'plan_' . $plan]);
            
            $pdo->prepare("UPDATE users SET plan_type=?, plan_until = GREATEST(COALESCE(plan_until, NOW()), NOW()) + make_interval(days => CAST(? AS integer)), badge_until = GREATEST(COALESCE(badge_until, NOW()), NOW()) + make_interval(days => CAST(? AS integer)), early_bird_until = GREATEST(COALESCE(early_bird_until, NOW()), NOW()) + make_interval(days => CAST(? AS integer)) WHERE id=?")
                ->execute([$plan, BADGE_DAYS, BADGE_DAYS, BADGE_DAYS, $me]);
            $pdo->commit();

            notify($pdo, $me, 'Üyeliğiniz Başlatıldı! 🎉', "{$planTitle} paketiniz başarıyla aktif edildi. 30 gün boyunca tüm VIP ayrıcalıklar hesabınızda geçerlidir.");
            out(['user' => public_user($me), 'last4' => substr($num, -4)]);
        }

        if ($act === 'admin_user_badge') {
            need_admin(); need_post(); $d = body();
            $id = (int)($d['id'] ?? 0);
            $give = !empty($d['give']);
            if ($give) {
                $pdo->prepare("UPDATE users SET badge_until = NOW() + INTERVAL '30 days' WHERE id=?")->execute([$id]);
                notify($pdo, $id, 'Öne Çıkarma Rozeti', 'Hesabınıza 30 günlük öne çıkarma rozeti hediye edildi!');
            } else {
                $pdo->prepare("UPDATE users SET badge_until = NULL WHERE id=?")->execute([$id]);
            }
            out(['ok' => true]);
        }

        if ($act === 'admin_approve') {
            need_admin(); need_post();
            $id = (int)(body()['id'] ?? 0);
            $pdo->prepare("UPDATE listings SET status='approved', active=TRUE WHERE id=?")->execute([$id]);
            $ownerId = (int)$pdo->query("SELECT user_id FROM listings WHERE id={$id}")->fetchColumn();
            notify($pdo, $ownerId, 'İlanınız Onaylandı!', 'İlanınız sitede yayına alındı.');
            out(['ok' => true]);
        }

        if ($act === 'admin_reject') {
            need_admin(); need_post();
            $id = (int)(body()['id'] ?? 0);
            $pdo->prepare("UPDATE listings SET status='rejected', active=FALSE WHERE id=?")->execute([$id]);
            $ownerId = (int)$pdo->query("SELECT user_id FROM listings WHERE id={$id}")->fetchColumn();
            notify($pdo, $ownerId, 'İlanınız Reddedildi', 'İlanınız onaylanmadı.');
            out(['ok' => true]);
        }

        if ($act === 'profile') {
            $me = need_user();
            $s = $pdo->prepare("SELECT COUNT(*) AS cnt, COALESCE(SUM(views),0) AS views FROM listings WHERE user_id=?");
            $s->execute([$me]); $st = $s->fetch();
            $l = $pdo->prepare("SELECT " . LISTING_COLS . ", FALSE AS is_fav FROM listings l JOIN users u ON u.id=l.user_id WHERE l.user_id=? ORDER BY l.created_at DESC");
            $l->execute([$me]);
            $p = $pdo->prepare("SELECT COUNT(*) FROM payments WHERE user_id=? AND status='success'");
            $p->execute([$me]);
            out([
                'user' => public_user($me),
                'stats' => ['listings' => (int)$st['cnt'], 'views' => (int)$st['views'], 'payments' => (int)$p->fetchColumn()],
                'listings' => array_map('listing_row', $l->fetchAll()),
            ]);
        }

        if ($act === 'pay_badge') {
            need_post(); $me = need_user(); $d = body();
            $holder = str($d['holder'] ?? '', 80);
            $num = preg_replace('/\D/', '', (string)($d['number'] ?? ''));
            $exp = str($d['expiry'] ?? '', 7);
            $cvv = preg_replace('/\D/', '', (string)($d['cvv'] ?? ''));
            if (mb_strlen($holder) < 3) out(['error' => 'Kart üzerindeki ismi girin.'], 422);
            if (!luhn($num)) out(['error' => 'Kart numarası geçersiz.'], 422);
            if (!preg_match('~^(0[1-9]|1[0-2])\s*/\s*(\d{2})$~', $exp)) out(['error' => 'Son kullanma tarihi AA/YY olmalı.'], 422);
            if (!preg_match('/^\d{3,4}$/', $cvv)) out(['error' => 'CVV 3 veya 4 haneli olmalı.'], 422);

            $pdo->beginTransaction();
            $pdo->prepare("INSERT INTO payments(user_id,amount,card_last4,card_brand,status,kind) VALUES(?,?,?,?,'success','badge')")->execute([$me, BADGE_PRICE, substr($num, -4), card_brand($num)]);
            $pdo->prepare("UPDATE users SET badge_until = GREATEST(COALESCE(badge_until, NOW()), NOW()) + make_interval(days => CAST(? AS integer)) WHERE id=?")->execute([BADGE_DAYS, $me]);
            $pdo->commit();
            notify($pdo, $me, 'Rozetiniz Aktif Edildi', 'Öne çıkarma rozetiniz 30 gün boyunca aktif.');
            out(['user' => public_user($me), 'last4' => substr($num, -4)]);
        }

        if ($act === 'conversations') {
            $me = need_user();
            $s = $pdo->prepare("SELECT DISTINCT ON (t.listing_id, t.other)
                    t.listing_id, t.other, t.body, t.created_at, t.sender_id,
                    l.title, u.name AS other_name, u.last_seen,
                    CASE WHEN u.badge_until > NOW() THEN u.avatar_id END AS other_avatar,
                    (u.badge_until IS NOT NULL AND u.badge_until > NOW()) AS other_badge,
                    (SELECT COUNT(*) FROM messages x WHERE x.sender_id = t.other AND x.receiver_id = :m3 AND x.read_at IS NULL AND x.listing_id IS NOT DISTINCT FROM t.listing_id) AS unread
                FROM (SELECT m.*, CASE WHEN m.sender_id = :m1 THEN m.receiver_id ELSE m.sender_id END AS other
                      FROM messages m WHERE m.sender_id = :m2 OR m.receiver_id = :m4) t
                JOIN users u ON u.id = t.other
                LEFT JOIN listings l ON l.id = t.listing_id
                ORDER BY t.listing_id, t.other, t.created_at DESC, t.id DESC");
            $s->execute(['m1' => $me, 'm2' => $me, 'm3' => $me, 'm4' => $me]);
            $rows = $s->fetchAll();
            usort($rows, fn($a, $b) => strcmp($b['created_at'], $a['created_at']));
            foreach ($rows as &$r) {
                $r['listing_id'] = $r['listing_id'] !== null ? (int)$r['listing_id'] : null;
                $r['other'] = (int)$r['other']; $r['sender_id'] = (int)$r['sender_id'];
                $r['unread'] = (int)$r['unread']; $r['other_badge'] = (bool)$r['other_badge'];
                $r['other_avatar'] = $r['other_avatar'] !== null ? (int)$r['other_avatar'] : null;
            }
            out(['threads' => $rows]);
        }

        if ($act === 'thread') {
            $me = need_user();
            $lid = (int)($_GET['listing'] ?? 0); $lid = $lid > 0 ? $lid : null;
            $o = (int)($_GET['with'] ?? 0); $after = (int)($_GET['after'] ?? 0);
            
            $blk = $pdo->prepare("SELECT 1 FROM blocks WHERE (blocker_id=? AND blocked_id=?) OR (blocker_id=? AND blocked_id=?)");
            $blk->execute([$me, $o, $o, $me]);
            $isBlocked = (bool)$blk->fetch();

            $s = $pdo->prepare("SELECT id,sender_id,body,created_at FROM messages
                WHERE listing_id IS NOT DISTINCT FROM CAST(:l AS integer) AND id > :after
                  AND ((sender_id = :a1 AND receiver_id = :b1) OR (sender_id = :b2 AND receiver_id = :a2))
                ORDER BY id LIMIT 300");
            $s->execute(['l' => $lid, 'after' => $after, 'a1' => $me, 'b1' => $o, 'b2' => $o, 'a2' => $me]);
            $msgs = $s->fetchAll();
            foreach ($msgs as &$m) { $m['id'] = (int)$m['id']; $m['sender_id'] = (int)$m['sender_id']; }
            $pdo->prepare("UPDATE messages SET read_at = NOW() WHERE receiver_id = :me AND sender_id = :o AND read_at IS NULL AND listing_id IS NOT DISTINCT FROM CAST(:l AS integer)")->execute(['me' => $me, 'o' => $o, 'l' => $lid]);
            out(['messages' => $msgs, 'me' => $me, 'is_blocked' => $isBlocked]);
        }

        if ($act === 'message_send') {
            need_post(); $me = need_user(); $d = body();
            $to = (int)($d['to'] ?? 0);
            $lid = (int)($d['listing_id'] ?? 0); $lid = $lid > 0 ? $lid : null;
            $txt = str($d['body'] ?? '', 1000);
            if ($txt === '') out(['error' => 'Mesaj boş olamaz.'], 422);
            if ($to === $me) out(['error' => 'Kendinize mesaj gönderemezsiniz.'], 422);

            $blk = $pdo->prepare("SELECT blocker_id FROM blocks WHERE (blocker_id=? AND blocked_id=?) OR (blocker_id=? AND blocked_id=?)");
            $blk->execute([$me, $to, $to, $me]);
            if ($blk->fetch()) out(['error' => 'Bu kullanıcıyla mesajlaşma engellenmiştir.'], 403);

            if ($lid) {
                $chkList = $pdo->prepare("SELECT user_id, created_at, find_type FROM listings WHERE id=?");
                $chkList->execute([$lid]);
                $cListing = $chkList->fetch();
                if ($cListing && $cListing['find_type'] !== 'looking_home' && (int)$cListing['user_id'] !== $me && !is_admin()) {
                    $isUnder48 = (time() - strtotime($cListing['created_at'])) < 48 * 3600;
                    if ($isUnder48) {
                        $usr = public_user($me);
                        if (!$usr['is_early_bird']) {
                            out(['error' => 'EARLY_BIRD_LOCKED'], 403);
                        }
                    }
                }
            }

            $c = $pdo->prepare("SELECT 1 FROM users WHERE id=?"); $c->execute([$to]);
            if (!$c->fetch()) out(['error' => 'Kullanıcı bulunamadı.'], 404);
            $s = $pdo->prepare("INSERT INTO messages(listing_id,sender_id,receiver_id,body) VALUES(?,?,?,?) RETURNING id, created_at");
            $s->execute([$lid, $me, $to, $txt]);
            $r = $s->fetch();
            notify($pdo, $to, 'Yeni Mesaj', 'Bir kullanıcıdan yeni bir mesajınız var.');
            out(['message' => ['id' => (int)$r['id'], 'sender_id' => $me, 'body' => $txt, 'created_at' => $r['created_at']]]);
        }
        /* ---------------- SESLİ ARAMA SİNYALİ ---------------- */
        if ($act === 'call_signal') {
            need_post(); $me = need_user(); $d = body();
            $to = (int)($d['to'] ?? 0);
            $type = str($d['type'] ?? '', 30);
            $data = json_encode($d['data'] ?? []);
            
            $pdo->prepare("INSERT INTO call_signals(sender_id, receiver_id, type, data) VALUES(?, ?, ?, ?::jsonb)")
                ->execute([$me, $to, $type, $data]);
            out(['ok' => true]);
        }

        if ($act === 'call_poll') {
            $me = need_user();
            $after = (int)($_GET['after'] ?? 0);
            $s = $pdo->prepare("SELECT cs.id, cs.sender_id, cs.type, cs.data, cs.created_at,
                                       u.name AS sender_name, u.avatar_id AS sender_avatar
                                FROM call_signals cs
                                JOIN users u ON u.id = cs.sender_id
                                WHERE cs.receiver_id = :me AND cs.id > :after
                                  AND cs.created_at > NOW() - INTERVAL '60 seconds'
                                ORDER BY cs.id ASC LIMIT 20");
            $s->execute(['me' => $me, 'after' => $after]);
            $signals = $s->fetchAll();
            foreach ($signals as &$sig) {
                $sig['id'] = (int)$sig['id'];
                $sig['sender_id'] = (int)$sig['sender_id'];
                $sig['data'] = !empty($sig['data']) ? (is_array($sig['data']) ? $sig['data'] : json_decode($sig['data'], true)) : [];
                $sig['sender_avatar'] = $sig['sender_avatar'] ? (int)$sig['sender_avatar'] : null;
            }
            $pdo->exec("DELETE FROM call_signals WHERE created_at < NOW() - INTERVAL '2 minutes'");
            out(['signals' => $signals]);
        }

        out(['error' => 'Bilinmeyen işlem.'], 404);
    } catch (RuntimeException $e) {
        out(['error' => $e->getMessage()], 422);
    } catch (Throwable $e) {
        error_log('[KiraBöl] ' . $e->getMessage());
        out(['error' => 'Sunucu hatası: ' . $e->getMessage()], 500);
    }
}

try { db(); } catch (Throwable $e) {
    http_response_code(500);
    echo '<!doctype html><meta charset="utf-8"><body style="font-family:sans-serif;padding:40px;color:#4A3A2F"><h2>Veritabanı Hatası:</h2><pre>' . htmlspecialchars($e->getMessage()) . '</pre></body>';
    exit;
}
$cfg = ['csrf' => $_SESSION['csrf'], 'price' => BADGE_PRICE, 'plusPrice' => PLUS_PRICE, 'studentPrice' => STUDENT_PRICE, 'earlyBirdPrice' => EARLY_BIRD_PRICE, 'boostPrice' => BOOST_PRICE, 'days' => BADGE_DAYS, 'maxPhotos' => MAX_PHOTOS];
?>
<!doctype html>
<html lang="tr">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>KiraBöl - Ev ve Kira Paylaşım Platformu</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
<style>
:root{
  --or:#FF6600; --or-d:#E55C00; --or-l:#FFF1E6; --or-m:#FFD9BF; --or-g:#FFB347;
  --bg:#FFFBF7; --card:#fff; --ink:#4A3A2F; --muted:#8C7B6E; --line:#F3E5D8;
  --ok:#2E9E6B; --err:#D64545; --blue:#2563EB; --blue-l:#EFF6FF; --r:16px;
  --sh:0 6px 24px rgba(255,102,0,.10); --sh-h:0 12px 32px rgba(255,102,0,.18);
}
body.dark{
  --bg:#121212; --card:#1E1E1E; --ink:#F3F4F6; --muted:#9CA3AF; --line:#2D3748;
  --or-l:#2D1F17; --or-m:#4A2D1B; --sh:0 6px 24px rgba(0,0,0,.4); --sh-h:0 12px 32px rgba(0,0,0,.6);
}
*{box-sizing:border-box;margin:0;padding:0}
body{font-family:'Plus Jakarta Sans',system-ui,sans-serif;background:var(--bg);color:var(--ink);line-height:1.55;transition:background .2s, color .2s}
button,input,select,textarea{font:inherit;color:inherit}
button{cursor:pointer;border:0;background:none}
a{color:var(--or-d);text-decoration:none}
svg{display:block}

.btn{display:inline-flex;align-items:center;justify-content:center;gap:8px;padding:11px 20px;border-radius:12px;font-weight:700;font-size:.93rem;transition:.18s;white-space:nowrap}
.btn-or{background:var(--or);color:#fff;box-shadow:0 4px 14px rgba(255,102,0,.28)}
.btn-or:hover{background:var(--or-d)}
.btn-soft{background:var(--or-l);color:var(--or-d)}
.btn-soft:hover{background:var(--or-m)}
.btn-line{background:var(--card);color:var(--or-d);border:1.5px solid var(--or-m)}
.btn-plain{background:var(--card);color:var(--ink);border:1.5px solid var(--line)}
.btn-sm{padding:7px 13px;font-size:.84rem;border-radius:10px}
.btn-lg{padding:14px 26px;font-size:1rem;border-radius:14px}
.btn-block{width:100%}
.btn svg{width:18px;height:18px}

.top{position:sticky;top:0;z-index:50;background:rgba(255,255,255,.94);backdrop-filter:blur(12px);border-bottom:1px solid var(--line)}
body.dark .top{background:rgba(30,30,30,.94)}
.top-in{max-width:1240px;margin:0 auto;padding:12px 20px;display:flex;align-items:center;gap:14px;flex-wrap:wrap}
.tools{border-top:1px solid var(--line);background:var(--bg)}
.tools-in{max-width:1240px;margin:0 auto;padding:10px 20px;display:flex;align-items:center;gap:10px;flex-wrap:wrap}
.logo{display:flex;align-items:center;gap:10px;font-weight:800;font-size:1.35rem;color:var(--ink)}
.logo i{width:36px;height:36px;border-radius:11px;background:linear-gradient(135deg,var(--or),var(--or-g));display:grid;place-items:center}
.logo i svg{width:20px;height:20px;color:#fff}
.logo b{color:var(--or)}
.search{flex:1;min-width:240px;display:flex;align-items:center;background:var(--card);border:1.5px solid var(--or-m);border-radius:14px;padding:4px 4px 4px 14px}
.search svg{width:19px;height:19px;color:var(--or);flex:none}
.search input{flex:1;border:0;outline:0;background:transparent;padding:8px 10px;min-width:0}
.nav{display:flex;align-items:center;gap:10px;margin-left:auto;flex-wrap:wrap}
.fcount{background:var(--or);color:#fff;border-radius:99px;font-size:.72rem;min-width:20px;height:20px;display:inline-grid;place-items:center;padding:0 6px}
.btn-admin{background:#FDECEC;color:var(--err);border:1.5px solid var(--err);font-weight:800}

.online-dot{width:10px;height:10px;border-radius:50%;display:inline-block;background:#9CA3AF;margin-right:4px}
.online-dot.active{background:#10B981;box-shadow:0 0 8px #10B981}
.blue-tick{color:var(--blue);font-size:.9rem;font-weight:900;margin-left:4px}
.early-badge{background:linear-gradient(135deg,#7C3AED,#A78BFA);color:#fff;font-size:.72rem;font-weight:800;padding:3px 8px;border-radius:6px;display:inline-flex;align-items:center;gap:3px}
.match-badge{color:#fff;font-size:.72rem;font-weight:800;padding:3px 8px;border-radius:99px;display:inline-flex;align-items:center;gap:3px}
.match-high{background:linear-gradient(135deg,#059669,#10B981)}
.match-mid{background:linear-gradient(135deg,#2563EB,#3B82F6)}
.match-low{background:linear-gradient(135deg,#D97706,#F59E0B)}

.av{--s:40px;width:var(--s);height:var(--s);border-radius:32%;background:var(--or);color:#fff;display:inline-grid;place-items:center;font-weight:800;font-size:calc(var(--s)*.42);position:relative;flex:none}
.av img{width:100%;height:100%;object-fit:cover;border-radius:inherit}
.avb{position:absolute;right:-5px;bottom:-5px;width:calc(var(--s)*.44);height:calc(var(--s)*.44);min-width:16px;min-height:16px;border-radius:50%;background:#fff;color:var(--or);display:grid;place-items:center;box-shadow:0 0 8px rgba(255,170,0,.9)}
.rozet{position:absolute;left:12px;top:12px;width:36px;height:36px;border-radius:50%;display:grid;place-items:center;background:linear-gradient(135deg,#FFF6D6,#FFE0A0);color:var(--or);box-shadow:0 0 16px rgba(255,170,0,.8)}
.rozet svg{width:22px;height:22px}

.hero{max-width:1240px;margin:0 auto;padding:34px 20px 6px}
.hero h1{font-size:clamp(1.7rem,3.6vw,2.6rem);font-weight:800;letter-spacing:-.03em}
.hero p{color:var(--muted);margin-top:10px;max-width:620px}
.bar{max-width:1240px;margin:0 auto;padding:22px 20px 6px;display:flex;align-items:center;gap:12px;flex-wrap:wrap}
.bar-right{margin-left:auto;display:flex;align-items:center;gap:12px;flex-wrap:wrap}
.sort-sel{padding:8px 14px;border:1.5px solid var(--line);border-radius:12px;background:var(--card);font-weight:600;font-size:.85rem;color:var(--ink);outline:0}

.main-tabs{display:inline-flex;background:var(--line);border-radius:14px;padding:4px;gap:4px}
.main-tab-btn{padding:8px 18px;border-radius:10px;font-weight:800;font-size:.86rem;color:var(--ink);transition:.2s;display:inline-flex;align-items:center;gap:6px}
.main-tab-btn.active{background:var(--card);color:var(--or-d);box-shadow:0 2px 8px rgba(0,0,0,.08)}
.main-tab-btn.active.blue{color:var(--blue)}
.main-tab-btn.active.purple{color:#7C3AED}

.wrap{max-width:1240px;margin:0 auto;padding:12px 20px 60px}
.grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(280px,1fr));gap:22px}

.card{background:var(--card);border-radius:var(--r);overflow:hidden;border:1px solid var(--line);box-shadow:var(--sh);cursor:pointer;transition:.2s;display:flex;flex-direction:column;position:relative}
.card:hover{transform:translateY(-4px);box-shadow:var(--sh-h)}
.card.glow{border:2px solid var(--or-g);box-shadow:0 0 0 4px rgba(255,179,71,.18)}
.thumb{height:168px;position:relative;display:grid;place-items:center;color:rgba(255,255,255,.9);overflow:hidden}
.thumb img.cv{position:absolute;inset:0;width:100%;height:100%;object-fit:cover}
.tag{position:absolute;right:12px;top:12px;background:rgba(255,255,255,.95);color:var(--or-d);font-size:.75rem;font-weight:700;padding:4px 10px;border-radius:99px;z-index:2}
.fav-btn{position:absolute;left:12px;bottom:12px;width:34px;height:34px;border-radius:50%;background:rgba(255,255,255,.92);color:#8C7B6E;display:grid;place-items:center;z-index:3}
.fav-btn.on{color:var(--err);background:#fff}
.cb{padding:16px 18px 18px;display:flex;flex-direction:column;gap:6px;flex:1}
.price{font-size:1.3rem;font-weight:800;color:var(--or-d)}
.price small{font-size:.8rem;font-weight:600;color:var(--muted)}
.loc{color:var(--muted);font-size:.87rem;font-weight:600}
.meta{display:flex;gap:14px;margin-top:auto;padding-top:10px;color:var(--muted);font-size:.82rem;border-top:1px dashed var(--line);align-items:center}

.person-card{background:var(--card);border-radius:18px;border:1.5px solid var(--line);box-shadow:var(--sh);padding:18px;display:flex;flex-direction:column;align-items:center;position:relative;transition:.22s;cursor:pointer}
.person-card:hover{transform:translateY(-4px);box-shadow:var(--sh-h);border-color:var(--ok)}
.person-card-top{width:100%;display:flex;justify-content:space-between;align-items:center;margin-bottom:6px}
.person-badge{color:var(--ok);font-size:.78rem;font-weight:800;display:inline-flex;align-items:center;gap:4px}
.person-fav-btn{background:none;border:0;cursor:pointer;color:var(--muted);display:flex;flex-direction:column;align-items:center;gap:2px;font-size:.65rem;font-weight:700}
.person-fav-btn.on{color:var(--err)}
.person-fav-btn svg{width:20px;height:20px}
.person-avatar{width:84px;height:84px;border-radius:50%;object-fit:cover;border:3px solid var(--line);background:var(--or-l);margin:4px 0 10px}
.person-name{font-size:1.05rem;font-weight:800;color:var(--ink);text-align:center;text-transform:uppercase;margin-bottom:12px;letter-spacing:.5px;display:flex;align-items:center;gap:4px}
.person-info-table{width:100%;border-top:1px solid var(--line);padding-top:10px;margin-bottom:16px}
.person-info-row{display:flex;justify-content:space-between;font-size:.84rem;padding:3.5px 0}
.person-info-label{color:var(--muted);font-weight:600}
.person-info-val{font-weight:700;color:var(--ink)}
.btn-view-person{width:100%;background:#2E9E6B;color:#fff;padding:10px 16px;border-radius:12px;font-weight:800;font-size:.92rem;text-align:center;border:0;cursor:pointer;transition:.18s}
.btn-view-person:hover{background:#238056}

.smart-drawer{position:fixed;top:0;right:0;bottom:0;width:min(440px,100%);background:var(--card);box-shadow:-10px 0 35px rgba(0,0,0,.3);z-index:125;transform:translateX(105%);transition:transform .3s cubic-bezier(0.16,1,0.3,1);display:flex;flex-direction:column}
.smart-drawer.open{transform:none}
.smart-drawer-overlay{position:fixed;inset:0;background:rgba(0,0,0,.5);z-index:124;display:none}
.smart-drawer-overlay.open{display:block}
.sd-header{padding:18px 22px;border-bottom:1px solid var(--line);display:flex;justify-content:space-between;align-items:center;background:var(--bg)}
.sd-content{flex:1;overflow-y:auto;padding:18px 22px;display:flex;flex-direction:column;gap:14px}
.sd-card{border:1.5px solid var(--line);border-radius:16px;padding:16px;background:var(--card);transition:.2s}
.sd-card:hover{border-color:var(--or)}

.empty{text-align:center;padding:70px 20px;color:var(--muted);background:var(--card);border:1.5px dashed var(--or-m);border-radius:var(--r)}
#map{height:calc(100vh - 290px);min-height:460px;border-radius:var(--r);border:1px solid var(--line);overflow:hidden}

.ov{position:fixed;inset:0;background:rgba(0,0,0,.5);backdrop-filter:blur(3px);z-index:120;display:none;align-items:center;justify-content:center;padding:18px}
.ov.show{display:flex}
.modal{background:var(--card);border-radius:20px;width:100%;max-width:500px;max-height:92vh;overflow:auto;padding:28px;box-shadow:0 24px 70px rgba(0,0,0,.35)}
.modal.wide{max-width:760px}
.mh{display:flex;align-items:center;justify-content:space-between;gap:12px;margin-bottom:18px}
.mh h3{font-size:1.25rem;font-weight:800}
.x{width:36px;height:36px;border-radius:10px;background:var(--or-l);color:var(--or-d);display:grid;place-items:center}

.f{display:flex;flex-direction:column;gap:6px;margin-bottom:16px}
.f label{font-size:.84rem;font-weight:700}
.f input,.f select,.f textarea{width:100%;padding:12px 14px;border:1.5px solid var(--line);border-radius:12px;background:var(--card);outline:0;transition:.2s}
.f input:focus,.f select:focus,.f textarea:focus{border-color:var(--or);box-shadow:0 0 0 3px rgba(255,102,0,.15)}
.f textarea{min-height:90px}
.row2{display:grid;grid-template-columns:1fr 1fr;gap:14px}
.row3{display:grid;grid-template-columns:1fr 1fr 1fr;gap:10px}
.err{background:#FDECEC;color:var(--err);padding:10px 14px;border-radius:12px;font-size:.88rem;margin-bottom:14px;display:none}
.err.show{display:block}

.amenity-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(140px,1fr));gap:8px;margin-top:6px}
.amenity-label{display:flex;align-items:center;gap:6px;font-size:.82rem;font-weight:600;padding:8px 10px;border:1.5px solid var(--line);border-radius:10px;cursor:pointer;background:var(--card)}
.amenity-label input{width:auto}

.fs{position:fixed;inset:0;background:var(--bg);z-index:100;overflow:auto;display:none}
.fs.d{z-index:110}
.fs.show{display:block}
.fs-top{position:sticky;top:0;z-index:5;background:rgba(255,255,255,.95);border-bottom:1px solid var(--line)}
body.dark .fs-top{background:rgba(30,30,30,.95)}
.fs-top>div{max-width:1100px;margin:0 auto;padding:14px 22px;display:flex;align-items:center;justify-content:space-between;gap:12px}
.fs-in{max-width:1100px;margin:0 auto;padding:30px 22px 70px}

/* PROFİL & ROZET */
.p-header{background:linear-gradient(135deg,#2D231C 0%,#4A3A2F 100%);border-radius:24px;padding:32px;color:#fff;display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:20px;box-shadow:var(--sh)}
.p-user{display:flex;align-items:center;gap:20px}
.p-tabs{display:flex;gap:8px;margin-top:24px;border-bottom:2px solid var(--line);padding-bottom:10px;overflow-x:auto}
.p-tab{padding:10px 20px;border-radius:12px;font-weight:700;font-size:.92rem;color:var(--muted);transition:.2s}
.p-tab.active{background:var(--or-l);color:var(--or-d)}
.p-stat-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(200px,1fr));gap:16px;margin:24px 0}
.p-stat{background:var(--card);border:1px solid var(--line);border-radius:16px;padding:18px 20px}
.p-stat span{color:var(--muted);font-size:.85rem;font-weight:600}
.p-stat b{display:block;font-size:1.6rem;font-weight:800;color:var(--ink);margin-top:4px}

.credit-card{background:linear-gradient(135deg,#1f1c18 0%,#3d3127 50%,#c59b27 100%);border-radius:18px;padding:26px;color:#fff;box-shadow:0 14px 28px rgba(0,0,0,.25);position:relative;aspect-ratio:1.6/1;display:flex;flex-direction:column;justify-content:space-between;margin-bottom:24px;border:1px solid rgba(255,255,255,.15)}
.card-chip{width:46px;height:34px;background:linear-gradient(135deg,#FFE599,#D4AF37);border-radius:8px;border:1px solid rgba(0,0,0,.15)}
.card-num{font-size:1.25rem;font-weight:700;letter-spacing:2px;font-family:monospace}
.card-bot{display:flex;justify-content:space-between;align-items:flex-end}
.card-holder{font-size:.85rem;font-weight:700;text-transform:uppercase;letter-spacing:1px}
.card-exp{font-size:.85rem;font-weight:600}
.card-brand{font-size:1.1rem;font-weight:800;letter-spacing:1px;font-style:italic}

/* İlan Detay & Galeri & Simülatör */
.dwrap{display:grid;grid-template-columns:1.55fr 1fr;gap:30px;align-items:start}
.gmain{position:relative;aspect-ratio:16/10;border-radius:22px;overflow:hidden;background:var(--or-m)}
.gmain>img{width:100%;height:100%;object-fit:cover}
.gth{display:flex;gap:8px;margin-top:10px;overflow-x:auto;padding-bottom:6px}
.gth img{width:88px;height:64px;object-fit:cover;border-radius:10px;cursor:pointer;border:2px solid transparent;transition:.2s}
.gth img.active{border-color:var(--or)}
.calc-box{background:var(--or-l);border:1.5px solid var(--or-m);border-radius:16px;padding:20px;margin:20px 0}
.calc-row{margin-bottom:14px}
.calc-row label{display:flex;justify-content:space-between;font-size:.85rem;font-weight:700;margin-bottom:6px}
.calc-row input[type=range]{width:100%;accent-color:var(--or);cursor:pointer}
.calc-result{background:var(--card);border-radius:12px;padding:12px 16px;display:flex;justify-content:space-between;align-items:center;border:1px solid var(--line)}
.calc-result b{font-size:1.25rem;color:var(--or-d)}

/* Admin Tablo */
.badge-status{font-size:.75rem;padding:3px 8px;border-radius:6px;font-weight:700;margin-left:8px}
.badge-status.pending{background:#FFF2DE;color:#D97706}
.badge-status.approved{background:#DEF7EC;color:#03543F}
.badge-status.rejected{background:#FDE8E8;color:#9B1C1C}
.badge-status.banned{background:#FDE8E8;color:#9B1C1C}
.adm-table{width:100%;border-collapse:collapse;margin-top:14px}
.adm-table th,.adm-table td{padding:12px;border-bottom:1px solid var(--line);text-align:left;font-size:.9rem}
.adm-table th{background:var(--bg);font-weight:700}
.adm-acts{display:flex;gap:6px;flex-wrap:wrap}

/* Sohbet & Mesajlar */
.chat-box{display:flex;flex-direction:column;height:450px;border:1px solid var(--line);border-radius:14px;overflow:hidden;background:var(--bg)}
.chat-msgs{flex:1;overflow-y:auto;padding:16px;display:flex;flex-direction:column;gap:10px}
.msg{max-width:75%;padding:10px 14px;border-radius:14px;font-size:.9rem;line-height:1.4}
.msg.me{align-self:flex-end;background:var(--or);color:#fff;border-bottom-right-radius:4px}
.msg.other{align-self:flex-start;background:var(--card);border:1px solid var(--line);border-bottom-left-radius:4px}
.chat-form{display:flex;gap:8px;padding:12px;background:var(--card);border-top:1px solid var(--line)}
.chat-form input{flex:1;padding:10px 14px;border:1.5px solid var(--line);border-radius:10px;outline:0}

.toast{position:fixed;left:50%;bottom:28px;transform:translateX(-50%) translateY(20px);background:var(--ink);color:var(--card);padding:12px 22px;border-radius:14px;font-weight:600;opacity:0;transition:.25s;z-index:300;pointer-events:none}
.toast.show{opacity:1;transform:translateX(-50%)}
.spin{width:18px;height:18px;border:3px solid rgba(255,255,255,.4);border-top-color:#fff;border-radius:50%;animation:sp .7s linear infinite}
@keyframes sp{to{transform:rotate(360deg)}}
@media(max-width:900px){.cols,.dwrap{grid-template-columns:1fr}.nav{margin-left:0;width:100%}}
/* ================= MODERN TAM EKRAN SOHBET & WEBRTC ================= */
.fs-chat-wrap{position:fixed;inset:0;background:var(--bg);z-index:200;display:none;flex-direction:row;overflow:hidden;box-shadow:0 20px 60px rgba(0,0,0,.25)}
.fs-chat-wrap.open{display:flex}
.fs-chat-sidebar{width:360px;border-right:1px solid var(--line);background:var(--card);display:flex;flex-direction:column;flex-shrink:0}
.fs-chat-sidebar-head{padding:18px 20px;border-bottom:1px solid var(--line);display:flex;justify-content:space-between;align-items:center}
.fs-chat-search{padding:10px 16px;border-bottom:1px solid var(--line);background:var(--bg)}
.fs-chat-search input{width:100%;padding:8px 14px;border:1px solid var(--line);border-radius:10px;background:var(--card);font-size:.85rem;color:var(--ink);outline:0}
.fs-chat-threads{flex:1;overflow-y:auto;padding:8px}

.fs-thread-item{display:flex;align-items:center;gap:12px;padding:12px 14px;border-radius:14px;cursor:pointer;transition:.18s;margin-bottom:4px;border:1px solid transparent}
.fs-thread-item:hover{background:var(--bg)}
.fs-thread-item.active{background:var(--or-l);border-color:var(--or-m)}
.fs-thread-av{position:relative;flex-shrink:0}
.fs-thread-av img, .fs-thread-av .av-letter{width:46px;height:46px;border-radius:50%;object-fit:cover;display:grid;place-items:center;background:var(--or);color:#fff;font-weight:800;font-size:1.1rem}
.fs-thread-info{flex:1;min-width:0}
.fs-thread-top{display:flex;justify-content:space-between;align-items:center;margin-bottom:3px}
.fs-thread-name{font-weight:800;font-size:.92rem;color:var(--ink);overflow:hidden;text-overflow:ellipsis;white-space:nowrap}
.fs-thread-time{font-size:.72rem;color:var(--muted);flex-shrink:0}
.fs-thread-sub{display:flex;justify-content:space-between;align-items:center}
.fs-thread-body{font-size:.82rem;color:var(--muted);overflow:hidden;text-overflow:ellipsis;white-space:nowrap;max-width:200px}
.fs-thread-unread{background:var(--or);color:#fff;font-size:.72rem;font-weight:800;padding:2px 7px;border-radius:99px;margin-left:6px;flex-shrink:0}

.fs-chat-main{flex:1;display:flex;flex-direction:column;background:var(--bg);min-width:0}
.fs-chat-header{padding:14px 24px;border-bottom:1px solid var(--line);background:var(--card);display:flex;align-items:center;justify-content:space-between;box-shadow:0 2px 8px rgba(0,0,0,.03)}
.fs-chat-messages{flex:1;overflow-y:auto;padding:24px 28px;display:flex;flex-direction:column;gap:12px}

/* MODERN SOHBET BALONCUKLARI */
.msg-row{display:flex;width:100%}
.msg-row.me{justify-content:flex-end}
.msg-row.other{justify-content:flex-start}
.msg{max-width:72%;padding:10px 16px;border-radius:18px;font-size:.92rem;line-height:1.5;position:relative;word-break:break-word}
.msg.me{background:linear-gradient(135deg, #FF6600, #FF8533);color:#fff;border-bottom-right-radius:4px;box-shadow:0 4px 14px rgba(255,102,0,.22)}
.msg.other{background:var(--card);color:var(--ink);border:1px solid var(--line);border-bottom-left-radius:4px;box-shadow:0 2px 10px rgba(0,0,0,.04)}
.msg-time{font-size:.68rem;opacity:.75;margin-top:4px;text-align:right}

/* MODERN MESAJ YAZMA ALANI (ENTER İLE GÖNDERME & FERAH ALAN) */
.fs-chat-input-bar{padding:14px 24px;background:var(--card);border-top:1px solid var(--line);display:flex;gap:12px;align-items:flex-end}
.fs-chat-input-box{flex:1;background:var(--bg);border:1.5px solid var(--line);border-radius:16px;padding:8px 14px;display:flex;align-items:center;transition:border-color .2s;box-shadow:inset 0 1px 3px rgba(0,0,0,.03)}
.fs-chat-input-box:focus-within{border-color:var(--or);background:var(--card);box-shadow:0 0 0 3px rgba(255,102,0,.12)}
.fs-chat-textarea{flex:1;border:0;background:none;resize:none;font:inherit;font-size:.95rem;color:var(--ink);outline:0;max-height:140px;line-height:1.45;padding:4px 0}
.btn-chat-send{width:46px;height:46px;border-radius:50%;background:var(--or);color:#fff;border:0;display:grid;place-items:center;cursor:pointer;transition:.18s;flex-shrink:0;box-shadow:0 4px 14px rgba(255,102,0,.3)}
.btn-chat-send:hover{transform:scale(1.06);background:var(--or-d)}
.btn-chat-send:active{transform:scale(.95)}

/* SESLİ ARAMA POPUP PENCERESİ */
.call-modal{position:fixed;top:50%;left:50%;transform:translate(-50%,-50%);background:#181512;color:#fff;padding:36px 30px;border-radius:28px;box-shadow:0 30px 80px rgba(0,0,0,.7);z-index:300;display:none;flex-direction:column;align-items:center;width:min(360px,92%);border:1px solid rgba(255,255,255,.1)}
.call-modal.active{display:flex;animation:modalPop .25s ease}
@keyframes modalPop{from{opacity:0;transform:translate(-50%,-46%) scale(.96)}to{opacity:1;transform:translate(-50%,-50%) scale(1)}}
.call-avatar{width:96px;height:96px;border-radius:50%;border:3.5px solid var(--ok);margin-bottom:16px;object-fit:cover;box-shadow:0 0 25px rgba(46,158,107,.4)}
.call-acts{display:flex;gap:24px;margin-top:26px}
.btn-call-act{width:60px;height:60px;border-radius:50%;display:grid;place-items:center;border:0;color:#fff;cursor:pointer;font-size:1.4rem;transition:.2s;box-shadow:0 8px 20px rgba(0,0,0,.35)}
.btn-call-act:hover{transform:scale(1.1)}
.btn-call-accept{background:#2E9E6B}
.btn-call-hangup{background:#D64545}

/* ================= 3'LÜ FİYATLANDIRMA & ÜYELİK ŞEMASI ================= */
.pricing-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(280px,1fr));gap:24px;margin-top:20px}
.pricing-card{background:var(--card);border:1.5px solid var(--line);border-radius:22px;padding:26px 24px;display:flex;flex-direction:column;position:relative;transition:.22s;box-shadow:var(--sh)}
.pricing-card:hover{transform:translateY(-4px);box-shadow:var(--sh-h)}
.pricing-card.highlight{border:2px solid var(--or);box-shadow:0 12px 35px rgba(255,102,0,.15)}
.pricing-card.student{border:2px solid #2563EB;box-shadow:0 12px 35px rgba(37,99,235,.12)}
.pricing-card.active-plan{background:linear-gradient(180deg, var(--card), var(--bg))}
.popular-ribbon{position:absolute;top:-12px;left:50%;transform:translateX(-50%);background:var(--or);color:#fff;font-size:.72rem;font-weight:800;padding:4px 14px;border-radius:99px;letter-spacing:.5px;box-shadow:0 4px 12px rgba(255,102,0,.3)}
.student-ribbon{position:absolute;top:-12px;left:50%;transform:translateX(-50%);background:#2563EB;color:#fff;font-size:.72rem;font-weight:800;padding:4px 14px;border-radius:99px;letter-spacing:.5px;box-shadow:0 4px 12px rgba(37,99,235,.3)}
.pricing-card-head{text-align:center;padding-bottom:18px;border-bottom:1px solid var(--line)}
.plan-tag{display:inline-block;padding:3px 10px;border-radius:8px;font-size:.72rem;font-weight:800;margin-bottom:6px;background:var(--line);color:var(--muted)}
.pricing-price{font-size:2rem;font-weight:900;color:var(--ink);margin:6px 0}
.pricing-price small{font-size:.85rem;font-weight:600;color:var(--muted)}
.pricing-price .old-price{font-size:1.1rem;color:var(--muted);text-decoration:line-through;margin-left:6px;font-weight:600}
.pricing-perks{flex:1;padding:20px 0;display:flex;flex-direction:column;gap:11px}
.perk-row{display:flex;align-items:center;gap:10px;font-size:.88rem;color:var(--ink)}
.perk-row.muted{opacity:.45}
.perk-ico{width:20px;height:20px;border-radius:50%;display:grid;place-items:center;font-size:.75rem;font-weight:900;background:var(--line);color:var(--muted);flex-shrink:0}
.perk-ico.ok{background:#DCFCE7;color:#16A34A}
.pricing-act{margin-top:auto;padding-top:10px}

@media(max-width:850px){.fs-chat-sidebar{width:100%}.fs-chat-main{display:none}.fs-chat-main.active{display:flex;position:fixed;inset:0;z-index:205}}
</style>
</head>
<body>

<header class="top">
  <div class="top-in">
    <a class="logo" href="./"><i><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.4"><path d="M3 11l9-8 9 8v10a1 1 0 0 1-1 1h-5v-7H9v7H4a1 1 0 0 1-1-1z"/></svg></i>Kira<b>Böl</b></a>
    <nav class="nav" id="nav"></nav>
  </div>
  <div class="tools"><div class="tools-in">
    <form class="search" id="searchForm">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.4"><circle cx="11" cy="11" r="7"/><path d="M20 20l-3.5-3.5"/></svg>
      <input id="q" type="search" placeholder="İlan No (#10000001), şehir veya meslek ara..." autocomplete="off">
      <button class="btn btn-or" type="submit">Ara</button>
    </form>
    <div id="toolBtns" style="display:flex;gap:10px;flex-wrap:wrap"></div>
  </div></div>
</header>

<section class="hero">
  <h1>Hayalindeki evi bul, kirayı birlikte böl.</h1>
  <p>Evine arkadaş arayanlar ile ev arayan kişilerin güvenle buluştuğu paylaşım ağı.</p>
</section>

<!-- ANA KATEGORİ AYRIMI (EVLER, EV ARAYANLAR, GEÇİCİ KONAKLAMA) -->
<div class="bar">
  <div class="main-tabs" id="mainFindTabs">
    <button class="main-tab-btn active" data-ft="has_home">🏠 Kiralık Ev & Odalar</button>
    <button class="main-tab-btn blue" data-ft="looking_home">👤 Ev Arayanlar (Kişiler)</button>
    <button class="main-tab-btn purple" data-ft="temporary">🧳 Geçici Oda / Staj</button>
  </div>

  <button class="btn btn-or btn-sm" id="bSmartMatchBar">⚡ Akıllı Eşleşmelerim</button>

  <div class="bar-right">
    <h3 id="resTitle" style="font-size:1rem;color:var(--muted)">0 ilan listeleniyor</h3>
    <select id="sSort" class="sort-sel">
      <option value="">Varsayılan (Öne Çıkanlar)</option>
      <option value="smart">⚡ En Yüksek Uyumluluk</option>
      <option value="price_asc">Fiyat / Bütçe: Düşükten Yükseğe</option>
      <option value="price_desc">Fiyat / Bütçe: Yüksekten Düşüğe</option>
      <option value="views">En Çok İncelenenler</option>
    </select>
  </div>
</div>

<main class="wrap" id="main"></main>

<!-- YANDAN AÇILAN AKILLI EŞLEŞME PANELİ -->
<div class="smart-drawer-overlay" id="sdOverlay"></div>
<aside class="smart-drawer" id="smartDrawer">
  <div class="sd-header">
    <h3 style="font-size:1.15rem;font-weight:800;color:var(--ink);display:flex;align-items:center;gap:6px">⚡ Akıllı Eşleşmelerim</h3>
    <button class="x" id="sdClose">${I.x}</button>
  </div>
  <div class="sd-content" id="sdList"></div>
</aside>

<div class="ov" id="modalOv"><div class="modal" id="modal"></div></div>
<div class="fs" id="fs"></div>
<div class="fs d" id="fsd"></div>
<div class="toast" id="toast"></div>

<script>
const CFG = <?= json_encode($cfg, JSON_UNESCAPED_UNICODE) ?>;
/* ========================================================================= */
/* >>> 1. PARÇA BURADA BİTİYOR.                                              */
/* ========================================================================= */
const CITIES_81 = [
  "Adana","Adıyaman","Afyonkarahisar","Ağrı","Amasya","Ankara","Antalya","Artvin","Aydın","Balıkesir",
  "Bilecik","Bingöl","Bitlis","Bolu","Burdur","Bursa","Çanakkale","Çankırı","Çorum","Denizli",
  "Diyarbakır","Edirne","Elazığ","Erzincan","Erzurum","Eskişehir","Gaziantep","Giresun","Gümüşhane","Hakkari",
  "Hatay","Isparta","Mersin","İstanbul","İzmir","Kars","Kastamonu","Kayseri","Kırklareli","Kırşehir",
  "Kocaeli","Konya","Kütahya","Malatya","Manisa","Kahramanmaraş","Mardin","Muğla","Muş","Nevşehir",
  "Niğde","Ordu","Rize","Sakarya","Samsun","Siirt","Sinop","Sivas","Tekirdağ","Tokat",
  "Trabzon","Tunceli","Şanlıurfa","Uşak","Van","Yozgat","Zonguldak","Aksaray","Bayburt","Karaman",
  "Kırıkkale","Batman","Şırnak","Bartın","Ardahan","Iğdır","Yalova","Karabük","Kilis","Osmaniye","Düzce"
];

const CITIES_COORDS = {
  'İstanbul':[41.0082,28.9784],'Ankara':[39.9334,32.8597],'İzmir':[38.4237,27.1428],
  'Bursa':[40.1885,29.0610],'Antalya':[36.8969,30.7133],'Eskişehir':[39.7667,30.5256],
  'Kırşehir':[39.1458,34.1639],'Kocaeli':[40.8533,29.8815],'Adana':[37.0000,35.3213]
};

const TYPES = ['Kiralık Daire','Ev Arkadaşı','Kiralık Oda'];
const ROOMS = ['1 oda','1+1','2+1','3+1','4+1'];
const AMENITIES_LIST = ['Tam Eşyalı', 'Fiber İnternet', 'Metroya Yakın', 'Doğalgaz Kombi', 'Balkon', 'Asansör', 'Bulaşık Makinesi', 'Klimalı'];

const sv = (p, w = 2.4) => `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="${w}" stroke-linecap="round" stroke-linejoin="round">${p}</svg>`;
const I = {
  x: sv('<path d="M6 6l12 12M18 6L6 18"/>', 2.6),
  filter: sv('<path d="M3 5h18l-7 8v6l-4 2v-8z"/>'),
  map: sv('<path d="M9 4L3 6v14l6-2 6 2 6-2V4l-6 2z"/><path d="M9 4v14M15 6v14"/>'),
  list: sv('<path d="M4 6h16M4 12h16M4 18h16"/>'),
  plus: sv('<path d="M12 5v14M5 12h14"/>', 2.6),
  eye: sv('<path d="M1 12s4-7 11-7 11 7 11 7-4 7-11 7S1 12 1 12z"/><circle cx="12" cy="12" r="3"/>').replace('<svg','<svg width="15" height="15"'),
  badge: '<svg viewBox="0 0 24 24" fill="currentColor"><path d="M12 2.5l8.5 7.2V20a1.5 1.5 0 0 1-1.5 1.5h-4.2v-6h-5.6v6H5A1.5 1.5 0 0 1 3.5 20V9.7z"/><path d="M12 8.2l1 2.1 2.3.3-1.7 1.6.4 2.3-2-1.1-2 1.1.4-2.3-1.7-1.6 2.3-.3z" fill="#fff"/></svg>',
  back: sv('<path d="M15 5l-7 7 7 7"/>', 2.6),
  bell: sv('<path d="M18 8A6 6 0 0 0 6 8c0 7-3 9-3 9h18s-3-2-3-9"/><path d="M13.73 21a2 2 0 0 1-3.46 0"/>'),
  chat: sv('<path d="M21 12a8 8 0 0 1-11.6 7.1L3 21l1.9-5.4A8 8 0 1 1 21 12z"/>'),
  share: sv('<circle cx="18" cy="5" r="3"/><circle cx="6" cy="12" r="3"/><circle cx="18" cy="19" r="3"/><line x1="8.59" y1="13.51" x2="15.42" y2="17.49"/><line x1="15.41" y1="6.51" x2="8.59" y2="10.49"/>'),
  check: sv('<path d="M20 6L9 17l-5-5"/>'),
  moon: sv('<path d="M21 12.79A9 9 0 1 1 11.21 3 7 7 0 0 0 21 12.79z"/>'),
  heart: (fill) => `<svg viewBox="0 0 24 24" fill="${fill ? 'currentColor' : 'none'}" stroke="currentColor" stroke-width="2.2"><path d="M20.84 4.61a5.5 5.5 0 0 0-7.78 0L12 5.67l-1.06-1.06a5.5 5.5 0 0 0-7.78 7.78l1.06 1.06L12 21.23l7.78-7.78 1.06-1.06a5.5 5.5 0 0 0 0-7.78z"/></svg>`
};

const $ = s => document.querySelector(s);
const esc = s => String(s ?? '').replace(/[&<>"']/g, c => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));
const tl = n => Number(n).toLocaleString('tr-TR') + ' ₺';
const imgSrc = id => '?img=' + id;

let me = null, listings = [], mode = 'grid', filters = {
  q:'',city:'',type:'',rooms:'',min:'',max:'',sort:'',favs:'',
  find_type:'has_home',start_age:'',end_age:'',occupation:'',alcohol:'',smoking:'',pet:'',gender:''
}, leafletMap = null;
let chatPollTimer = null;

if (localStorage.getItem('theme') === 'dark') document.body.classList.add('dark');

async function api(action, payload = null, query = '') {
  const opt = {headers:{'X-CSRF':CFG.csrf}};
  if (payload instanceof FormData) { opt.method = 'POST'; opt.body = payload; }
  else if (payload !== null) { opt.method = 'POST'; opt.headers['Content-Type'] = 'application/json'; opt.body = JSON.stringify(payload); }
  let res, data;
  try { res = await fetch('?api=' + action + query, opt); } catch (e) { throw new Error('Sunucuya ulaşılamadı.'); }
  try { data = await res.json(); } catch (e) { throw new Error('Geçersiz yanıt.'); }
  if (!res.ok) throw new Error(data.error || 'Hata oluştu.');
  return data;
}

function toast(msg) { const t = $('#toast'); t.textContent = msg; t.classList.add('show'); clearTimeout(t._t); t._t = setTimeout(() => t.classList.remove('show'), 3000); }
function showErr(id, msg) { const e = $(id); e.textContent = msg; e.classList.add('show'); }
function avatar(name, av, size, badge) {
  const inner = av ? `<img src="${imgSrc(av)}" alt="">` : esc((name || '?').charAt(0).toUpperCase());
  return `<span class="av" style="--s:${size}px">${inner}${badge ? `<span class="avb">${I.badge}</span>` : ''}</span>`;
}

function lock() { document.body.style.overflow = document.querySelector('.fs.show,.ov.show') ? 'hidden' : ''; }
function openLayer(id, html) { const el = $('#' + id); el.innerHTML = html; el.classList.add('show'); lock(); return el; }
function closeLayer(id) { $('#' + id).classList.remove('show'); lock(); }
function openModal(html, wide) { const m = $('#modal'); m.className = 'modal' + (wide ? ' wide' : ''); m.innerHTML = html; $('#modalOv').classList.add('show'); lock(); }
function closeModal() {
  if (chatPollTimer) clearInterval(chatPollTimer);
  $('#modalOv').classList.remove('show');
  lock();
}
function closeFS() { closeLayer('fs'); }
function closeDetail() { closeLayer('fsd'); }
$('#modalOv').addEventListener('mousedown', e => { if (e.target.id === 'modalOv') closeModal(); });

function renderNav() {
  $('#toolBtns').innerHTML = `<button class="btn btn-line" id="bFilter">${I.filter}Detaylı Filtre</button>
    <button class="btn ${mode === 'map' ? 'btn-or' : 'btn-soft'}" id="bMap">${mode === 'map' ? I.list + 'Liste' : I.map + 'Harita'}</button>`;
  
  let h = '';
  h += `<button class="btn btn-plain btn-sm" id="bDarkToggle" title="Gece/Gündüz Modu">${I.moon}</button>`;

  if (me) {
    if (me.is_admin) h += `<button class="btn btn-admin" id="bAdmin">🛡️ Yönetim Paneli</button>`;
    h += `<button class="btn btn-plain" id="bNotif">${I.bell}Bildirimler${me.unread_notifs ? `<span class="fcount">${me.unread_notifs}</span>` : ''}</button>`;
    h += `<button class="btn btn-plain ${filters.favs ? 'btn-soft' : ''}" id="bFavs">${I.heart(filters.favs)}Favorilerim${me.fav_count ? `<span class="fcount">${me.fav_count}</span>` : ''}</button>`;
    h += `<button class="btn btn-plain" id="bMsg">${I.chat}Mesajlar${me.unread ? `<span class="fcount">${me.unread}</span>` : ''}</button>`;
  }
  h += `<button class="btn btn-or" id="bNew">${I.plus}İlan Ver</button>`;
  if (me) {
    h += `<button class="btn btn-plain" id="bProf">${avatar(me.name, me.avatar, 26, me.badge)}Profilim</button>`;
  } else {
    h += `<button class="btn btn-plain" id="bLogin">Giriş Yap</button>`;
  }
  $('#nav').innerHTML = h;

  $('#bDarkToggle').onclick = () => {
    document.body.classList.toggle('dark');
    localStorage.setItem('theme', document.body.classList.contains('dark') ? 'dark' : 'light');
  };
  $('#bFilter').onclick = openFilter;
  $('#bMap').onclick = () => { mode = mode === 'map' ? 'grid' : 'map'; renderNav(); renderList(); };
  $('#bNew').onclick = () => me ? openListingForm() : openAuth('login', 'İlan vermek için giriş yapın.');
  if ($('#bAdmin')) $('#bAdmin').onclick = openAdminPanel;
  if ($('#bNotif')) $('#bNotif').onclick = openNotifications;
  if ($('#bFavs')) $('#bFavs').onclick = () => { filters.favs = filters.favs ? '' : '1'; renderNav(); loadListings(); };
  if ($('#bMsg')) $('#bMsg').onclick = openConversations;
  if ($('#bProf')) $('#bProf').onclick = () => openProfile('listings');
  if ($('#bLogin')) $('#bLogin').onclick = () => openAuth('login');
}

/* ------------------------------------------------ GİZLİ YÖNETİCİ KISAYOLU */
window.addEventListener('keydown', (e) => {
  if ((e.ctrlKey || e.metaKey) && e.shiftKey && (e.key === 'A' || e.key === 'a')) {
    e.preventDefault();
    openAdminSecretModal();
  }
});

function openAdminSecretModal() {
  openModal(`
    <div class="mh"><h3 style="color:var(--err)">🔒 Yönetici Doğrulama</h3><button class="x" onclick="closeModal()">${I.x}</button></div>
    <p style="color:var(--muted);font-size:.88rem;margin-bottom:14px">Yetkili şifrenizi girin (Varsayılan: Admin123)</p>
    <div class="err" id="admSecErr"></div>
    <form id="admSecForm" onsubmit="return false;">
      <div class="f"><label>Yönetici Şifresi</label><input id="admSecPass" type="password" placeholder="Admin123" autofocus></div>
      <button class="btn btn-or btn-block btn-lg" id="bAdmSecLogin" type="submit" style="margin-top:10px">Doğrula & Panele Gir</button>
    </form>
  `);
  setTimeout(() => $('#admSecPass')?.focus(), 120);

  $('#admSecForm').onsubmit = async (e) => {
    e.preventDefault();
    const pass = $('#admSecPass').value;
    if (!pass) return;
    try {
      const res = await api('admin_quick_login', { password: pass });
      me = res.user;
      closeModal();
      renderNav();
      loadListings();
      toast('Yönetici olarak giriş yapıldı! 🛡️');
      openAdminPanel();
    } catch(err) {
      showErr('#admSecErr', err.message);
    }
  };
}

/* ------------------------------------------------ ARAMA & ANA SEKME AYRIMI */
$('#searchForm').onsubmit = (e) => {
  e.preventDefault();
  filters.q = $('#q').value;
  loadListings();
};

$('#sSort').onchange = (e) => {
  filters.sort = e.target.value;
  loadListings();
};

document.querySelectorAll('#mainFindTabs .main-tab-btn').forEach(btn => {
  btn.onclick = () => {
    document.querySelectorAll('#mainFindTabs .main-tab-btn').forEach(b => b.classList.remove('active'));
    btn.classList.add('active');
    filters.find_type = btn.dataset.ft;
    loadListings();
  };
});

$('#bSmartMatchBar').onclick = () => {
  if (!me) return openAuth('login', 'Akıllı eşleşmelerinizi görmek için lütfen giriş yapın.');
  openSmartDrawer();
};

$('#sdClose').onclick = closeSmartDrawer;
$('#sdOverlay').onclick = closeSmartDrawer;

function openSmartDrawer() {
  const matches = [...listings].sort((a, b) => (b.match_score || 0) - (a.match_score || 0));
  const listEl = $('#sdList');

  if (!matches.length) {
    listEl.innerHTML = '<div class="empty">Eşleşen aday bulunamadı.</div>';
  } else {
    listEl.innerHTML = matches.map(m => {
      const sc = m.match_score || 50;
      const badgeCls = sc >= 80 ? 'match-high' : (sc >= 60 ? 'match-mid' : 'match-low');
      const isLooking = m.find_type === 'looking_home';
      const reasonsHtml = (m.match_reasons || []).map(r => `<span style="font-size:.72rem;background:var(--line);color:var(--ink);padding:2px 7px;border-radius:6px;font-weight:700">✓ ${esc(r)}</span>`).join(' ');

      return `
      <div class="sd-card">
        <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:8px">
          <span class="match-badge ${badgeCls}">⚡ %${sc} Uyumlu</span>
          <small style="color:var(--muted);font-weight:700">#${m.listing_no}</small>
        </div>
        <div style="display:flex;gap:12px;align-items:center">
          <img src="${m.cover ? imgSrc(m.cover) : (m.owner_avatar ? imgSrc(m.owner_avatar) : 'https://cdn-icons-png.flaticon.com/512/847/847969.png')}" style="width:52px;height:52px;border-radius:50%;object-fit:cover;border:2px solid var(--line)">
          <div style="flex:1">
            <b style="font-size:.95rem;color:var(--ink)">${esc(m.owner)}</b> ${m.is_verified ? '<span class="blue-tick">✓</span>' : ''}<br>
            <span style="font-size:.82rem;color:var(--muted)">${esc(m.city)} / ${esc(m.district || '')}</span>
          </div>
          <div style="text-align:right">
            <b style="color:var(--or-d);font-size:.95rem">${tl(m.price)}</b><br>
            <small style="color:var(--muted);font-size:.7rem">${isLooking ? 'Bütçe' : 'Kira'}</small>
          </div>
        </div>
        ${reasonsHtml ? `<div style="display:flex;gap:4px;flex-wrap:wrap;margin-top:10px">${reasonsHtml}</div>` : ''}
        <div style="display:flex;gap:6px;margin-top:12px">
          <button class="btn btn-soft btn-sm btn-block" onclick="openDetail(${m.id});closeSmartDrawer();">İncele</button>
          <button class="btn btn-or btn-sm btn-block" onclick="openChatThread(${m.id}, ${m.user_id}, '${esc(m.owner)}');closeSmartDrawer();">💬 Mesajlaş</button>
        </div>
      </div>`;
    }).join('');
  }

  $('#smartDrawer').classList.add('open');
  $('#sdOverlay').classList.add('open');
}

function closeSmartDrawer() {
  $('#smartDrawer').classList.remove('open');
  $('#sdOverlay').classList.remove('open');
}

async function loadListings() {
  const p = new URLSearchParams();
  Object.entries(filters).forEach(([k, v]) => { if (v !== '') p.set(k, v); });
  try { listings = (await api('listings', null, '&' + p.toString())).listings; }
  catch (e) { listings = []; }
  renderList();
}

function personCardHtml(l) {
  const ls = l.lifestyle || {};
  const sc = l.match_score || 50;
  const badgeCls = sc >= 80 ? 'match-high' : (sc >= 60 ? 'match-mid' : 'match-low');

  return `
  <article class="person-card" data-id="${l.id}">
    <div class="person-card-top">
      <div>
        <span class="person-badge">✓ Onaylı</span>
        <span class="match-badge ${badgeCls}">⚡ %${sc} Uyumlu</span>
      </div>
      <button class="person-fav-btn ${l.is_fav ? 'on' : ''}" data-fav="${l.id}" onclick="event.stopPropagation()">
        ${I.heart(l.is_fav)}
        <span>${l.is_fav ? 'Favoride' : 'Favoriye Ekle'}</span>
      </button>
    </div>

    <img class="person-avatar" src="${l.cover ? imgSrc(l.cover) : (l.owner_avatar ? imgSrc(l.owner_avatar) : 'https://cdn-icons-png.flaticon.com/512/847/847969.png')}">

    <div class="person-name">
      ${esc(l.owner)}
      ${l.is_verified ? '<span class="blue-tick" title="Doğrulanmış Profil">✓</span>' : ''}
    </div>

    <div class="person-info-table">
      <div class="person-info-row"><span class="person-info-label">İlan No</span><span class="person-info-val">#${l.listing_no}</span></div>
      <div class="person-info-row"><span class="person-info-label">İl</span><span class="person-info-val">${esc(l.city)}</span></div>
      <div class="person-info-row"><span class="person-info-label">İlçe</span><span class="person-info-val">${esc(l.district || 'Belirtilmemiş')}</span></div>
      <div class="person-info-row"><span class="person-info-label">Cinsiyet</span><span class="person-info-val">${esc(ls.gender || 'Erkek')}</span></div>
      <div class="person-info-row"><span class="person-info-label">Yaş</span><span class="person-info-val">${l.user_age || 'Belirtilmemiş'}</span></div>
      <div class="person-info-row"><span class="person-info-label">Durumu</span><span class="person-info-val">${esc(l.occupation || 'Belirtilmemiş')}</span></div>
      <div class="person-info-row"><span class="person-info-label">Sigara</span><span class="person-info-val">${esc(ls.smoking || 'Hayır')}</span></div>
      <div class="person-info-row"><span class="person-info-label">Alkol</span><span class="person-info-val">${esc(l.alcohol || 'Hayır')}</span></div>
      <div class="person-info-row"><span class="person-info-label">Kendi Evi</span><span class="person-info-val" style="color:var(--err)">Yok</span></div>
    </div>

    <button class="btn-view-person">Kişiyi Gör</button>
  </article>`;
}

function homeCardHtml(l) {
  const sc = l.match_score || 50;
  const badgeCls = sc >= 80 ? 'match-high' : (sc >= 60 ? 'match-mid' : 'match-low');

  return `<article class="card ${l.badge ? 'glow' : ''}" data-id="${l.id}">
    <div class="thumb" style="background:#FFE6D1">
      ${l.cover ? `<img class="cv" src="${imgSrc(l.cover)}" loading="lazy">` : ''}
      ${l.badge ? `<span class="rozet">${I.badge}</span>` : ''}
      
      ${l.is_early_bird_locked ? `<span class="early-badge" style="position:absolute;left:12px;top:12px;z-index:2">⏳ VIP ${l.early_bird_remaining_hours}s</span>` : ''}

      <span class="tag">${esc(l.type)}</span>
      <button class="fav-btn ${l.is_fav ? 'on' : ''}" data-fav="${l.id}">${I.heart(l.is_fav)}</button>
    </div>
    <div class="cb">
      <div style="display:flex;justify-content:space-between;align-items:center">
        <span style="font-size:.75rem;font-weight:800;color:var(--muted)">İLAN NO: #${l.listing_no}</span>
        <span class="match-badge ${badgeCls}">⚡ %${sc} Uyum</span>
      </div>
      <div class="price">${tl(l.price)} <small>/ ay</small></div>
      <h3 style="font-size:1rem;font-weight:700">${esc(l.title)}</h3>
      <div class="loc">${esc(l.district ? l.district + ', ' : '')}${esc(l.city)}</div>
      ${l.metro_station ? `<div style="font-size:.76rem;color:var(--blue);font-weight:700">🚇 ${esc(l.metro_station)}</div>` : ''}
      <div class="meta">
        <span>${esc(l.rooms || '-')}</span>
        <span>${I.eye}${l.views}</span>
        <span style="margin-left:auto;display:inline-flex;align-items:center">${esc(l.owner)} ${l.is_verified ? '<span class="blue-tick">✓</span>' : ''}</span>
      </div>
    </div></article>`;
}

function renderList() {
  const isLooking = filters.find_type === 'looking_home';
  $('#resTitle').textContent = filters.favs ? `Favorilerim (${listings.length})` : `${listings.length} ilan listeleniyor`;
  const m = $('#main');
  
  if (mode === 'map') {
    m.innerHTML = `<div id="map"></div>`;
    initMap();
    return;
  }

  if (!listings.length) {
    m.innerHTML = `<div class="empty"><h3>Bu kategoride henüz ilan bulunmuyor</h3><p>Filtreleri değiştirebilir veya ilk ilanı siz verebilirsiniz.</p></div>`;
    return;
  }

  m.innerHTML = `<div class="grid">${listings.map(l => isLooking ? personCardHtml(l) : homeCardHtml(l)).join('')}</div>`;
  m.querySelectorAll('.card, .person-card').forEach(c => c.onclick = () => openDetail(+c.dataset.id));
  m.querySelectorAll('.fav-btn, .person-fav-btn').forEach(btn => btn.onclick = (e) => {
    e.stopPropagation();
    if (!me) return openAuth('login');
    api('favorite_toggle', {listing_id: +btn.dataset.fav}).then(r => {
      btn.classList.toggle('on', r.is_fav);
      if (btn.querySelector('span')) btn.querySelector('span').textContent = r.is_fav ? 'Favoride' : 'Favoriye Ekle';
      me.fav_count = r.fav_count; renderNav();
      toast(r.is_fav ? 'Favorilere eklendi ❤️' : 'Çıkarıldı.');
    });
  });
}

function initMap() {
  if (leafletMap) { leafletMap.remove(); leafletMap = null; }
  leafletMap = L.map('map').setView([39.0, 35.0], 6);
  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', { attribution: '&copy; OpenStreetMap' }).addTo(leafletMap);

  listings.forEach(l => {
    if (!l.lat || !l.lng) return;
    const marker = L.marker([l.lat, l.lng]).addTo(leafletMap);
    marker.bindPopup(`
      <b>İlan No: #${l.listing_no}</b><br>
      <b>${esc(l.title)}</b><br>
      <span style="color:#FF6600;font-weight:bold">${tl(l.price)}</span> ${l.find_type==='looking_home'?'(Bütçe)':'/ ay'}<br>
      <button class="btn btn-or btn-sm" style="margin-top:6px" onclick="openDetail(${l.id})">İlanı Gör</button>
    `);
  });
}

/* ------------------------------------------------ BİLDİRİM MERKEZİ */
async function openNotifications() {
  let res;
  try { res = await api('notifications'); } catch (e) { toast(e.message); return; }
  const notifs = res.notifications || [];
  if (me) { me.unread_notifs = 0; renderNav(); }

  openModal(`
    <div class="mh"><h3>🔔 Bildirimleriniz</h3><button class="x" onclick="closeModal()">${I.x}</button></div>
    <div class="ml">
      ${!notifs.length ? '<p style="color:var(--muted)">Henüz yeni bir bildiriminiz yok.</p>' : notifs.map(n => `
        <div class="mli" style="cursor:default">
          <div style="flex:1">
            <b>${esc(n.title)}</b><br>
            <span style="font-size:.86rem;color:var(--ink)">${esc(n.body)}</span><br>
            <small style="color:var(--muted)">${n.created_at.slice(0, 16)}</small>
          </div>
        </div>
      `).join('')}
    </div>
  `);
}

/* ------------------------------------------------ RESMİ HUKUKİ SÖZLEŞME MODALI */
function openAgreementModal() {
  openModal(`
    <div class="mh">
      <h3 style="font-size:1.15rem">📄 Resmi Ev Arkadaşı & Masraf Paylaşım Sözleşmesi</h3>
      <button class="x" onclick="closeModal()">${I.x}</button>
    </div>
    <div style="font-size:.83rem;line-height:1.65;color:var(--ink);max-height:60vh;overflow-y:auto;padding:12px;background:var(--bg);border:1px solid var(--line);border-radius:12px">
      <div style="text-align:center;border-bottom:2px solid var(--ink);padding-bottom:10px;margin-bottom:14px">
        <h4 style="font-size:1rem;font-weight:900;letter-spacing:1px">T.C. 6098 SAYILI TÜRK BORÇLAR KANUNU ESASLARINCA DÜZENLENMİŞ</h4>
        <h3 style="font-size:1.1rem;font-weight:900;color:var(--or-d)">EV ARKADAŞLIĞI VE ORTAK GİDER PAYLAŞIM PROTOKOLÜ</h3>
      </div>
      
      <b>MADDE 1: TARAFLAR VE TAŞINMAZ</b><br>
      İşbu sözleşme; mezkur adresteki taşınmazda ikamet eden Asıl Kiracı/Ev Sahibi ile konuta ortak sıfatıyla dahil olan Ev Arkadaşı arasında tanzim edilmiştir.<br><br>

      <b>MADDE 2: KİRA BEDELİ VE ÖDEME DÜZENİ</b><br>
      Ev Arkadaşı, üzerine düşen aylık net kira payını her ayın en geç 5. günü banka transferi ile eksiksiz olarak ödemekle yükümlüdür.<br><br>

      <b>MADDE 3: GÜVENCE BEDELİ (DEPOZİTO)</b><br>
      Ev Arkadaşı giriş esnasında 1 aylık kira bedeli tutarında depozitoyu nakden teslim eder. Tahliye anında taşınmazda veya demirbaşlarda hasar ve ödenmemiş fatura borcu bulunmadığı takdirde depozito eksiksiz iade edilir.<br><br>

      <b>MADDE 4: ORTAK TÜKETİM GİDERLERİ</b><br>
      Elektrik, su, doğalgaz, internet ve bina aidat giderleri hane halkı kişi sayısına eşit bölünür.<br><br>

      <b>MADDE 5: EV İÇİ DÜZEN VE MİSAFİR POLİTİKASI</b><br>
      Ortak yaşam alanları temiz ve düzenli tutulacaktır. Yatılı misafir konaklamalarında diğer tarafın en az 24 saat önceden onayının alınması esastır.<br><br>

      <b>MADDE 6: SÖZLEŞMENİN FESHİ VE TAHLİYE</b><br>
      Konuttan ayrılmak isteyen taraf diğer tarafa en az 30 gün önceden yazılı bildirim yapmak zorundadır.<br><br>

      <div style="display:flex;justify-content:space-between;margin-top:20px;padding-top:14px;border-top:1px dashed var(--line);text-align:center">
        <div><b>1. TARAF (ASIL KİRACI / EV SAHİBİ)</b><br><br><br>İsim / İmza / Tarih</div>
        <div><b>2. TARAF (EV ARKADAŞI)</b><br><br><br>İsim / İmza / Tarih</div>
      </div>
    </div>
    <div style="display:flex;gap:10px;margin-top:16px">
      <button class="btn btn-or btn-block" onclick="window.print()">🖨️ Yazdır / PDF Olarak Kaydet</button>
    </div>
  `, true);
}

/* ------------------------------------------------ DETAYLI FİLTRELEME */
function openFilter() {
  openModal(`
    <div class="mh"><h3>Detaylı Filtreleme</h3><button class="x" onclick="closeModal()">${I.x}</button></div>
    
    <div class="row2">
      <div class="f"><label>İl (81 İl)</label>
        <select id="fCity">
          <option value="">Tüm Türkiye</option>
          ${CITIES_81.map(c => `<option value="${c}" ${filters.city===c?'selected':''}>${c}</option>`).join('')}
        </select>
      </div>
      <div class="f"><label>Meslek / Durum</label>
        <select id="fOcc">
          <option value="">Fark Etmez</option>
          <option value="Öğrenci" ${filters.occupation==='Öğrenci'?'selected':''}>🎓 Öğrenci</option>
          <option value="Çalışan" ${filters.occupation==='Çalışan'?'selected':''}>💼 Çalışan</option>
          <option value="Memur" ${filters.occupation==='Memur'?'selected':''}>🏛️ Memur</option>
        </select>
      </div>
    </div>

    <div class="row2">
      <div class="f"><label>Min Yaş</label><input id="fStartAge" type="number" placeholder="Örn: 18" value="${filters.start_age}"></div>
      <div class="f"><label>Max Yaş</label><input id="fEndAge" type="number" placeholder="Örn: 28" value="${filters.end_age}"></div>
    </div>

    <div class="row2">
      <div class="f"><label>Min Fiyat / Bütçe (₺)</label><input id="fMin" type="number" value="${filters.min}"></div>
      <div class="f"><label>Max Fiyat / Bütçe (₺)</label><input id="fMax" type="number" value="${filters.max}"></div>
    </div>

    <h4 style="margin:10px 0 8px;font-size:.9rem;color:var(--or-d)">🌱 Alışkanlıklar & Tercihler</h4>
    <div class="row3">
      <div class="f"><label>Sigara</label>
        <select id="fSmoking">
          <option value="">Fark Etmez</option>
          <option value="Hayır" ${filters.smoking==='Hayır'?'selected':''}>Hayır</option>
          <option value="Evet" ${filters.smoking==='Evet'?'selected':''}>Evet</option>
        </select>
      </div>
      <div class="f"><label>Alkol</label>
        <select id="fAlcohol">
          <option value="">Fark Etmez</option>
          <option value="Hayır" ${filters.alcohol==='Hayır'?'selected':''}>Hayır</option>
          <option value="Evet" ${filters.alcohol==='Evet'?'selected':''}>Evet</option>
        </select>
      </div>
      <div class="f"><label>Cinsiyet</label>
        <select id="fGender">
          <option value="">Fark Etmez</option>
          <option value="Erkek" ${filters.gender==='Erkek'?'selected':''}>Erkek</option>
          <option value="Kadın" ${filters.gender==='Kadın'?'selected':''}>Kadın</option>
        </select>
      </div>
    </div>

    <div style="display:flex;gap:10px;margin-top:14px">
      <button class="btn btn-plain btn-block" id="fReset">Sıfırla</button>
      <button class="btn btn-or btn-block" id="fApply">Sonuçları Göster</button>
    </div>
  `);

  $('#fReset').onclick = () => {
    filters.city = ''; filters.min = ''; filters.max = '';
    filters.start_age = ''; filters.end_age = ''; filters.occupation = '';
    filters.alcohol = ''; filters.smoking = ''; filters.gender = '';
    closeModal(); loadListings();
  };
  $('#fApply').onclick = () => {
    filters.city = $('#fCity').value;
    filters.occupation = $('#fOcc').value;
    filters.start_age = $('#fStartAge').value;
    filters.end_age = $('#fEndAge').value;
    filters.min = $('#fMin').value;
    filters.max = $('#fMax').value;
    filters.smoking = $('#fSmoking').value;
    filters.alcohol = $('#fAlcohol').value;
    filters.gender = $('#fGender').value;
    closeModal(); loadListings();
  };
}

/* ------------------------------------------------ İLAN DETAY & CANLI SOHBET & ERKEN ERİŞİM */
async function openDetail(id) {
  let l;
  try { l = (await api('listing_view', {id})).listing; } catch (e) { toast(e.message); return; }
  const mine = !!me && (me.id === l.user_id || me.is_admin);
  const imgs = l.images || [];
  const ls = l.lifestyle || {};
  const ams = l.amenities || [];
  const isLooking = l.find_type === 'looking_home';
  const waText = encodeURIComponent(`Merhaba, KiraBöl'deki #${l.listing_no} no'lu ilanınız için yazıyorum: ` + window.location.href);

  const lastSeenDate = new Date(l.last_seen || Date.now());
  const isOnline = (Date.now() - lastSeenDate.getTime()) < 5 * 60 * 1000;

  openLayer('fsd', `
  <div class="fs-top"><div>
    <button class="btn btn-soft" id="dBack">${I.back}Geri</button>
    <div style="display:flex;gap:10px">
      ${me && me.is_admin && l.status === 'pending' ? `<button class="btn btn-or" onclick="adminApprove(${l.id});closeDetail();">✅ İlanı Onayla</button>` : ''}
      ${mine ? `<button class="btn btn-plain" style="color:var(--err)" id="dDel">Sil</button>` : ''}
    </div>
  </div></div>
  <div class="fs-in"><div class="dwrap">
    <div>
      <div class="gmain">
        <img id="gImg" src="${imgs.length ? imgSrc(imgs[0]) : (l.cover ? imgSrc(l.cover) : 'https://cdn-icons-png.flaticon.com/512/847/847969.png')}">
      </div>
      ${imgs.length > 1 ? `
        <div class="gth">
          ${imgs.map((im, idx) => `<img src="${imgSrc(im)}" class="${idx===0?'active':''}" onclick="setGalImg(this, '${imgSrc(im)}')">`).join('')}
        </div>
      ` : ''}

      <div style="margin:16px 0 6px;display:flex;align-items:center;gap:10px;flex-wrap:wrap">
        <span class="tag ${isLooking ? 'blue' : ''}" style="position:static;display:inline-block">${isLooking ? '👤 Ev Arayan Kişi Profili' : (l.find_type==='temporary' ? '🧳 Geçici Oda' : '🏠 Ev / Oda İlanı')}</span>
        <b style="color:var(--muted);font-size:.85rem">İlan No: #${l.listing_no}</b>
        ${l.match_score ? `<span class="match-badge match-high" style="font-size:.8rem;padding:4px 10px">⚡ %${l.match_score} Yaşam Tarzı Uyumu</span>` : ''}
      </div>

      <h1 style="font-size:1.7rem;font-weight:800;margin:6px 0 10px">${esc(l.title)}</h1>
      <p style="color:var(--muted);font-weight:600">${esc(l.city)} / ${esc(l.district)} ${l.address ? '— ' + esc(l.address) : ''}</p>
      ${l.metro_station ? `<p style="color:var(--blue);font-weight:700;font-size:.9rem;margin-top:4px">🚇 ${esc(l.metro_station)}</p>` : ''}
      
      <div style="display:flex;gap:10px;flex-wrap:wrap;margin:18px 0">
        ${l.user_age ? `<div class="p-stat" style="padding:10px 16px"><span>Yaş</span><b>🎂 ${l.user_age} Yaşında</b></div>` : ''}
        ${l.occupation ? `<div class="p-stat" style="padding:10px 16px"><span>Durumu</span><b>🎓 ${esc(l.occupation)}</b></div>` : ''}
        <div class="p-stat" style="padding:10px 16px"><span>Sigara</span><b>🚭 ${esc(ls.smoking || 'Belirtilmemiş')}</b></div>
        <div class="p-stat" style="padding:10px 16px"><span>Alkol</span><b>🍷 ${esc(l.alcohol || 'Belirtilmemiş')}</b></div>
      </div>

      ${ams.length ? `
        <h4 style="font-weight:800;margin-bottom:8px">🛋️ Ev ve Oda Olanakları</h4>
        <div style="display:flex;gap:8px;flex-wrap:wrap;margin-bottom:20px">
          ${ams.map(a => `<span style="background:var(--card);border:1px solid var(--line);padding:6px 14px;border-radius:10px;font-size:.85rem;font-weight:700">✓ ${esc(a)}</span>`).join('')}
        </div>
      ` : ''}

      <div style="margin:20px 0"><p style="white-space:pre-line">${esc(l.description)}</p></div>

      ${!isLooking ? `
        <div class="calc-box">
          <h4 style="font-weight:800;color:var(--ink);font-size:1.1rem;margin-bottom:6px">🧮 Kira & Gider Bölüştürücü</h4>
          <p style="color:var(--muted);font-size:.85rem;margin-bottom:14px">Ev arkadaşlarıyla paylaşıldığında kişi başına düşen tahmini aylık toplam maliyet:</p>
          <div class="calc-row">
            <label><span>Birlikte Yaşanacak Kişi Sayısı:</span> <b id="calcPplVal">2 Kişi</b></label>
            <input type="range" id="calcPpl" min="1" max="5" value="2" step="1">
          </div>
          <div class="calc-row">
            <label><span>Tahmini Ek Gider (Fatura + Aidat):</span> <b id="calcBillVal">2.500 ₺</b></label>
            <input type="range" id="calcBill" min="0" max="15000" value="2500" step="250">
          </div>
          <div class="calc-result">
            <span style="font-weight:700">Kişi Başı Aylık Düşen:</span>
            <b id="calcTotal">-- ₺</b>
          </div>
        </div>
      ` : ''}

      <div style="margin-top:30px">
        <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:12px">
          <h3 style="font-size:1.15rem;font-weight:800">⭐ Ev Arkadaşı Yorumları (${l.owner})</h3>
          ${me && me.id !== l.user_id ? `<button class="btn btn-soft btn-sm" onclick="openReviewModal(${l.user_id}, '${esc(l.owner)}')">Yorum & Puan Ekle</button>` : ''}
        </div>
        <div id="listingReviews" style="display:flex;flex-direction:column;gap:10px">
          <p style="color:var(--muted);font-size:.85rem">Yorumlar yükleniyor...</p>
        </div>
      </div>
    </div>

    <div>
      <div class="panel">
        <div class="price ${isLooking ? 'blue' : ''}">${tl(l.price)} <small>/ ${isLooking ? 'bütçe' : 'ay'}</small></div>
        
        <div style="margin:16px 0">
          <div style="display:flex;align-items:center;gap:6px">
            <b>${isLooking ? 'Arayan Kişi' : 'İlan Sahibi'}:</b> ${esc(l.owner)}
            ${l.is_verified ? '<span class="blue-tick" title="Doğrulanmış Profil">✓ Mavi Tik</span>' : ''}
          </div>
          <div style="font-size:.82rem;color:var(--muted);margin-top:3px;display:flex;align-items:center">
            <span class="online-dot ${isOnline ? 'active' : ''}"></span>
            ${isOnline ? '<b style="color:#10B981">Şu an çevrimiçi</b>' : 'Son görülme: ' + lastSeenDate.toLocaleTimeString('tr-TR', {hour:'2-digit',minute:'2-digit'})}
            ${l.owner_rating ? `<span style="margin-left:auto;font-weight:800;color:#D97706">⭐ ${l.owner_rating}</span>` : ''}
          </div>

          <div style="display:flex;gap:6px;margin-top:10px;flex-wrap:wrap">
            ${l.instagram ? `<a href="https://instagram.com/${esc(l.instagram)}" target="_blank" class="btn btn-plain btn-sm" style="padding:4px 10px;font-size:.78rem">📷 @${esc(l.instagram)}</a>` : ''}
            ${l.linkedin ? `<a href="https://linkedin.com/in/${esc(l.linkedin)}" target="_blank" class="btn btn-plain btn-sm" style="padding:4px 10px;font-size:.78rem">💼 LinkedIn</a>` : ''}
          </div>
        </div>

        ${l.phone ? `<div style="margin-bottom:12px"><b>Telefon:</b> <a href="tel:${esc(l.phone)}">${esc(l.phone)}</a></div>` : ''}

        <div style="display:flex;flex-direction:column;gap:10px;margin-top:16px">
          ${l.is_early_bird_locked && (!me || !me.is_early_bird) && !mine ? `
            <div style="background:#F3E8FF;border:1.5px solid #C084FC;padding:12px;border-radius:12px;font-size:.84rem;color:#581C87">
              <b>⏳ Bu İlanda Erken Erişim Aktif!</b><br>
              Evi kaçırmamak ve ilk mesaj atan olmak için Erken Erişim VIP paketini aktif edebilirsiniz.
              <button class="btn btn-or btn-block btn-sm" style="margin-top:8px;background:#7C3AED" onclick="openEarlyBirdModal()">🚀 99 ₺ ile Erken Erişim Kilidini Aç</button>
            </div>
          ` : ''}

          ${me && me.id !== l.user_id ? `
            <button class="btn btn-or btn-block" onclick="openChatThread(${l.id}, ${l.user_id}, '${esc(l.owner)}')">
              ${I.chat} ${isLooking ? esc(l.owner) + ' ile Hemen Mesajlaş' : 'İlan Sahibine Mesaj Gönder'}
            </button>
          ` : ''}

          ${!me ? `
            <button class="btn btn-or btn-block" onclick="openAuth('login', 'Mesajlaşmak için giriş yapmalısınız.')">
              ${I.chat} Mesaj Göndermek İçin Giriş Yap
            </button>
          ` : ''}

          ${!isLooking ? `
            <button class="btn ${l.is_alert_active ? 'btn-soft' : 'btn-line'} btn-block" id="bPriceAlert" onclick="togglePriceAlert(${l.id})">
              📉 ${l.is_alert_active ? 'Fiyat Alarmı Açık (Takipte)' : 'Fiyatı Düşünce Haber Ver'}
            </button>
          ` : ''}

          <a class="btn btn-line btn-block" href="https://api.whatsapp.com/send?text=${waText}" target="_blank">
            ${I.share} WhatsApp ile Paylaş
          </a>

          <button class="btn btn-soft btn-block" onclick="openAgreementModal()">
            📄 Resmi Sözleşme Şablonunu İndir
          </button>

          <button class="btn btn-plain btn-sm" style="color:var(--err);margin-top:6px" onclick="openReportModal(${l.id}, ${l.user_id}, '${esc(l.title)}')">
            🚩 İlanı / Kullanıcıyı Şikayet Et
          </button>
        </div>
      </div>
    </div>
  </div></div>`);

  $('#dBack').onclick = closeDetail;
  if ($('#dDel')) $('#dDel').onclick = async () => {
    if (!confirm('İlan silinsin mi?')) return;
    await api('listing_delete', {id: l.id});
    closeDetail(); toast('Silindi.'); loadListings();
  };

  api('user_reviews', null, `&user_id=${l.user_id}`).then(res => {
    const revBox = $('#listingReviews');
    if (!revBox) return;
    if (!res.reviews || !res.reviews.length) {
      revBox.innerHTML = '<p style="color:var(--muted);font-size:.85rem">Henüz bu kullanıcıya ait bir değerlendirme bulunmuyor.</p>';
    } else {
      revBox.innerHTML = res.reviews.map(rw => `
        <div style="background:var(--bg);border:1px solid var(--line);border-radius:12px;padding:12px">
          <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:4px">
            <b style="font-size:.88rem">${esc(rw.reviewer_name)}</b>
            <span style="color:#D97706;font-weight:800;font-size:.82rem">${'★'.repeat(rw.rating)}${'☆'.repeat(5-rw.rating)}</span>
          </div>
          <p style="font-size:.84rem;color:var(--ink)">${esc(rw.comment)}</p>
        </div>
      `).join('');
    }
  });

  if (!isLooking && $('#calcPpl')) {
    const updateCalc = () => {
      const ppl = +$('#calcPpl').value;
      const bill = +$('#calcBill').value;
      $('#calcPplVal').textContent = ppl + ' Kişi';
      $('#calcBillVal').textContent = tl(bill);
      const perPerson = Math.round((l.price + bill) / ppl);
      $('#calcTotal').textContent = tl(perPerson) + ' / kişi';
    };
    $('#calcPpl').oninput = updateCalc;
    $('#calcBill').oninput = updateCalc;
    updateCalc();
  }
}

window.togglePriceAlert = async (lid) => {
  if (!me) return openAuth('login');
  try {
    const r = await api('price_alert_toggle', {listing_id: lid});
    const btn = $('#bPriceAlert');
    btn.className = `btn ${r.active ? 'btn-soft' : 'btn-line'} btn-block`;
    btn.innerHTML = `📉 ${r.active ? 'Fiyat Alarmı Açık (Takipte)' : 'Fiyatı Düşünce Haber Ver'}`;
    toast(r.active ? 'Fiyat alarmı kuruldu! Fiyat inerse haber vereceğiz.' : 'Fiyat alarmı kaldırıldı.');
  } catch(e) { toast(e.message); }
};

window.openReviewModal = (targetUid, targetName) => {
  openModal(`
    <div class="mh"><h3>⭐ Değerlendirme Yap</h3><button class="x" onclick="closeModal()">${I.x}</button></div>
    <p style="color:var(--muted);font-size:.88rem;margin-bottom:12px"><b>${esc(targetName)}</b> ile ev arkadaşlığı deneyiminiz nasıldı?</p>
    <div class="err" id="revErr"></div>
    <div class="f">
      <label>Puanınız</label>
      <select id="revStars">
        <option value="5">⭐⭐⭐⭐⭐ (5/5) Harika bir ev arkadaşı</option>
        <option value="4">⭐⭐⭐⭐ (4/5) Gayet iyi ve düzenli</option>
        <option value="3">⭐⭐⭐ (3/5) Ortalama</option>
        <option value="2">⭐⭐ (2/5) Bazı sorunlar yaşadık</option>
        <option value="1">⭐ (1/5) Kesinlikle tavsiye etmiyorum</option>
      </select>
    </div>
    <div class="f">
      <label>Yorumunuz</label>
      <textarea id="revComment" placeholder="Düzen, temizlik, fatura ödeme alışkanlıkları vb."></textarea>
    </div>
    <button class="btn btn-or btn-block" id="bSubmitReview">Değerlendirmeyi Yayınla</button>
  `);

  $('#bSubmitReview').onclick = async () => {
    try {
      await api('review_add', {
        target_user_id: targetUid,
        rating: $('#revStars').value,
        comment: $('#revComment').value
      });
      closeModal();
      toast('Değerlendirmeniz başarıyla eklendi! ⭐');
      openDetail(listings.find(x => x.user_id === targetUid)?.id || 1);
    } catch(e) { showErr('#revErr', e.message); }
  };
};

/* ------------------------------------------------ ERKEN ERİŞİM & BOOST DOPİNGİ MODALLARI */
window.openEarlyBirdModal = () => {
  if (!me) return openAuth('login');
  openModal(`
    <div class="mh"><h3 style="color:#7C3AED">🚀 VIP Erken Erişim Paketi</h3><button class="x" onclick="closeModal()">${I.x}</button></div>
    <div style="background:var(--bg);border:1px solid #E9D5FF;padding:14px;border-radius:12px;margin-bottom:16px">
      <b style="color:#6B21A8;display:block;margin-bottom:4px">Evleri Başkasına Kaptırmayın!</b>
      <p style="font-size:.86rem;color:var(--ink)">En uygun fiyatlı oda ve evler ilk 48 saatte kapılır. VIP Erken Erişim ile tüm yeni ilanlara 48 saat önceden mesaj atabilir ve doğrudan iletişime geçebilirsiniz.</p>
    </div>
    <div class="err" id="ebErr"></div>
    <div class="f"><label>Kart Numarası</label><input id="ebCard" placeholder="4543 0000 0000 0000" maxlength="19"></div>
    <button class="btn btn-or btn-block btn-lg" id="bPayEarlyBird" style="background:#7C3AED">
      ${CFG.earlyBirdPrice} ₺ ile 30 Gün VIP Ol
    </button>
  `);

  $('#bPayEarlyBird').onclick = async () => {
    try {
      const res = await api('pay_early_bird', { number: $('#ebCard').value });
      me = res.user;
      closeModal();
      toast('Tebrikler! VIP Erken Erişim hakkınız tanımlandı! 🚀');
      loadListings();
    } catch(e) { showErr('#ebErr', e.message); }
  };
};

function getCardModalHtml(title, price, subtitle, btnText) {
  return `
    <div class="mh"><h3>${title}</h3><button class="x" onclick="closeModal()">${I.x}</button></div>
    <p style="color:var(--muted);font-size:.88rem;margin-bottom:16px">${subtitle}</p>

    <!-- İNTERAKTİF KREDİ KARTI GÖRSELİ -->
    <div class="credit-card" style="margin: 0 auto 18px auto">
      <div style="display:flex;justify-content:space-between;align-items:center">
        <div class="card-chip"></div>
        <div class="card-brand" id="cModalBrand">KART</div>
      </div>
      <div class="card-num" id="cModalNum">•••• •••• •••• ••••</div>
      <div class="card-bot">
        <div><small style="opacity:.7;font-size:.65rem">KART SAHİBİ</small><div class="card-holder" id="cModalHolder">${esc(me ? me.name : 'AD SOYAD').toUpperCase()}</div></div>
        <div><small style="opacity:.7;font-size:.65rem">SKT</small><div class="card-exp" id="cModalExp">MM/YY</div></div>
      </div>
    </div>

    <div class="err" id="cModalErr"></div>
    <div class="f"><label>Kart Üzerindeki İsim</label><input id="cInHolder" placeholder="Ad Soyad" value="${esc(me ? me.name : '')}"></div>
    <div class="f"><label>Kart Numarası</label><input id="cInNum" placeholder="4543 0000 0000 0000" maxlength="19"></div>
    <div class="row2">
      <div class="f"><label>Son Kullanma (AA/YY)</label><input id="cInExp" placeholder="12/28" maxlength="5"></div>
      <div class="f"><label>Güvenlik Kodu (CVV)</label><input id="cInCvv" placeholder="123" maxlength="4"></div>
    </div>
    <div style="display:flex;align-items:center;justify-content:center;gap:8px;margin-bottom:14px;color:var(--muted);font-size:.78rem">
      <span>🔒 256-Bit SSL Korumalı Güvenli Ödeme</span>
    </div>
    <button class="btn btn-or btn-block btn-lg" id="cModalPayBtn">${btnText}</button>
  `;
}

function bindCardInputs(holderEl, numEl, expEl, cvvEl, previewHolder, previewNum, previewExp, previewBrand) {
  numEl.oninput = () => {
    let v = numEl.value.replace(/\D/g, '').slice(0, 16);
    numEl.value = v.replace(/(\d{4})(?=\d)/g, '$1 ');
    previewNum.textContent = numEl.value || '•••• •••• •••• ••••';
    if (v.startsWith('4')) previewBrand.textContent = 'VISA';
    else if (/^(5[1-5]|2[2-7])/.test(v)) previewBrand.textContent = 'MASTERCARD';
    else if (v.startsWith('9792')) previewBrand.textContent = 'TROY';
    else previewBrand.textContent = 'KART';
  };
  holderEl.oninput = () => previewHolder.textContent = (holderEl.value || (me ? me.name : 'AD SOYAD')).toUpperCase();
  expEl.oninput = () => {
    let v = expEl.value.replace(/\D/g, '').slice(0, 4);
    if (v.length >= 2) v = v.slice(0, 2) + '/' + v.slice(2);
    expEl.value = v;
    previewExp.textContent = v || 'MM/YY';
  };
}

window.openBoostModal = (lid) => {
  openModal(getCardModalHtml(
    '⚡ İlanı Başa Al (Doping)',
    CFG.boostPrice,
    'İlanınızı sanki 1 dakika önce yayınlanmış gibi listenin en tepesine taşıyın, 5 kat daha hızlı ev arkadaşı bulun.',
    `${CFG.boostPrice} ₺ Güvenli Öde ve En Tepeye Taşı`
  ));

  bindCardInputs(
    $('#cInHolder'), $('#cInNum'), $('#cInExp'), $('#cInCvv'),
    $('#cModalHolder'), $('#cModalNum'), $('#cModalExp'), $('#cModalBrand')
  );

  $('#cModalPayBtn').onclick = async () => {
    try {
      await api('pay_boost', {
        listing_id: lid,
        holder: $('#cInHolder').value,
        number: $('#cInNum').value,
        expiry: $('#cInExp').value,
        cvv: $('#cInCvv').value
      });
      closeModal();
      toast('İlanınız başarıyla en tepeye taşındı! ⚡');
      openProfile('listings');
      loadListings();
    } catch(e) { showErr('#cModalErr', e.message); }
  };
};

window.openPlanPayModal = (plan) => {
  const isStudent = plan === 'student';
  const price = isStudent ? CFG.studentPrice : CFG.plusPrice;
  const planTitle = isStudent ? 'KiraBöl Öğrenci+ 🎓' : 'KiraBöl Plus+ ⭐';

  openModal(getCardModalHtml(
    `${planTitle} Üyelik Satın Al`,
    price,
    '30 gün boyunca tüm VIP ayrıcalıklar, en üstte listelenme ve sınırsız doğrudan iletişim hesabınıza tanımlanacaktır.',
    `${price} ₺ Güvenli Ödeme Yap ve Başlat`
  ));

  bindCardInputs(
    $('#cInHolder'), $('#cInNum'), $('#cInExp'), $('#cInCvv'),
    $('#cModalHolder'), $('#cModalNum'), $('#cModalExp'), $('#cModalBrand')
  );

  $('#cModalPayBtn').onclick = async () => {
    try {
      const res = await api('pay_plan', {
        plan,
        holder: $('#cInHolder').value,
        number: $('#cInNum').value,
        expiry: $('#cInExp').value,
        cvv: $('#cInCvv').value
      });
      me = res.user;
      closeModal();
      toast(`${planTitle} üyeliğiniz başarıyla başlatıldı! 🎉`);
      openProfile('badge');
      renderNav();
    } catch(e) { showErr('#cModalErr', e.message); }
  };
};

window.openStudentVerifyModal = () => {
  const sInfo = me.student_doc_info || {};
  openModal(`
    <div class="mh"><h3>🎓 Öğrenci Belgesi Doğrulama</h3><button class="x" onclick="closeModal()">${I.x}</button></div>
    <div style="background:var(--or-l);border:1px solid var(--or-m);color:var(--ink);padding:12px 14px;border-radius:12px;font-size:.84rem;margin-bottom:14px;line-height:1.5">
      <b>💡 Sahte Belge Önleme & Doğrulama Sistemi:</b><br>
      KiraBöl Öğrenci+ (79 TL) avantajından yararlanabilmek için e-Devlet üzerinden (veya üniversitenizden) aldığınız resmi Öğrenci Belgesi'ni ve üzerindeki barkod kodunu giriniz. Yöneticilerimiz e-Devlet barkod doğrulama servisi üzerinden belgenin gerçekliğini anlık teyit eder.
    </div>
    ${me.student_status === 'rejected' ? `
      <div style="background:#FEE2E2;border:1px solid #FCA5A5;color:#991B1B;padding:12px;border-radius:10px;font-size:.85rem;margin-bottom:14px">
        <b>Son Başvurunuz Reddedildi:</b> ${esc(me.student_reject_reason || 'Lütfen güncel ve geçerli bir e-Devlet belgesi yükleyin.')}
      </div>
    ` : ''}
    <div class="err" id="stVerErr"></div>
    <form id="fStudentVerify">
      <div class="f"><label>Üniversite Adı *</label><input name="university" placeholder="Örn: İstanbul Teknik Üniversitesi" value="${esc(sInfo.university || '')}" required></div>
      <div class="f"><label>Bölüm / Fakülte *</label><input name="department" placeholder="Örn: Bilgisayar Mühendisliği" value="${esc(sInfo.department || '')}" required></div>
      <div class="f">
        <label>e-Devlet Belge Barkod Numarası *</label>
        <input name="barcode" placeholder="Örn: YOK-2026-XXXX veya 16 haneli barkod no" value="${esc(sInfo.barcode || '')}" required>
        <small style="color:var(--muted);font-size:.74rem">Belgenizin sağ üst veya alt köşesinde yer alan e-Devlet barkod kodu.</small>
      </div>
      <div class="f"><label>Açıklama / Not (Opsiyonel)</label><input name="note" placeholder="Örn: 2. sınıf örgün öğretim öğrencisiyim." value="${esc(sInfo.note || '')}"></div>
      <div class="f">
        <label>Öğrenci Belgesi Dosyası (PDF veya Resim) *</label>
        <input type="file" name="document" accept="image/jpeg,image/png,image/webp,application/pdf" required style="padding:10px;border:1.5px dashed var(--line);border-radius:12px;background:var(--bg);width:100%">
      </div>
      <button class="btn btn-or btn-block btn-lg" type="submit" style="margin-top:16px">
        📄 Belgeyi Gönder ve Doğrulamaya İlet
      </button>
    </form>
  `);

  $('#fStudentVerify').onsubmit = async (e) => {
    e.preventDefault();
    const fd = new FormData($('#fStudentVerify'));
    try {
      const res = await api('student_verify_submit', fd);
      me = res.user;
      closeModal();
      toast('Öğrenci belgeniz başarıyla iletildi! Moderasyon incelemesi sonrası bildirim alacaksınız. 🎓');
      openProfile('badge');
    } catch(err) { showErr('#stVerErr', err.message); }
  };
};

window.setGalImg = (el, src) => {
  $('#gImg').src = src;
  document.querySelectorAll('.gth img').forEach(img => img.classList.remove('active'));
  el.classList.add('active');
};

/* ------------------------------------------------ ŞİKAYET MODALI */
function openReportModal(listingId, userId, title) {
  if (!me) return openAuth('login', 'Şikayet bildiriminde bulunmak için giriş yapmalısınız.');
  openModal(`
    <div class="mh"><h3 style="color:var(--err)">🚩 Şikayet Bildirimi</h3><button class="x" onclick="closeModal()">${I.x}</button></div>
    <p style="font-size:.88rem;color:var(--muted);margin-bottom:12px"><b>İlan:</b> ${esc(title)}</p>
    <div class="err" id="repErr"></div>
    <div class="f">
      <label>Şikayet Nedeni</label>
      <select id="repReason">
        <option>Sahte veya Yanıltıcı İlan</option>
        <option>Gerçek Dışı Fiyat</option>
        <option>Dolandırıcılık Şüphesi</option>
        <option>Uygunsuz / Hakaret İçeren İçerik</option>
        <option>Ev Artık Müsait Değil</option>
        <option>Diğer</option>
      </select>
    </div>
    <div class="f">
      <label>Açıklamanız (Opsiyonel)</label>
      <textarea id="repDetails" placeholder="Şikayetinizi detaylandırın..."></textarea>
    </div>
    <button class="btn btn-or btn-block" id="bDoReport">Şikayeti Yetkiliye İlet</button>
  `);

  $('#bDoReport').onclick = async () => {
    try {
      await api('report_create', {
        listing_id: listingId,
        user_id: userId,
        reason: $('#repReason').value,
        details: $('#repDetails').value
      });
      closeModal();
      toast('Şikayetiniz moderasyon ekibimize iletildi! 🛡️');
    } catch(e) { showErr('#repErr', e.message); }
  };
}

/* ------------------------------------------------ CANLI MESAJLAŞMA & ENGELLEME */
async function openConversations() {
  let res;
  try { res = await api('conversations'); } catch (e) { toast(e.message); return; }
  const threads = res.threads || [];

  openModal(`
    <div class="mh"><h3>Mesaj Kutum</h3><button class="x" onclick="closeModal()">${I.x}</button></div>
    <div class="ml">
      ${!threads.length ? '<p style="color:var(--muted)">Henüz bir mesajlaşmanız yok.</p>' : threads.map(t => `
        <div class="mli" onclick="openChatThread(${t.listing_id || 0}, ${t.other}, '${esc(t.other_name)}')">
          <div style="flex:1">
            <b>${esc(t.other_name)}</b> <small style="color:var(--muted)">${esc(t.title || 'Genel Sohbet')}</small><br>
            <span style="font-size:.85rem;color:var(--muted)">${esc(t.body.slice(0, 45))}...</span>
          </div>
          ${t.unread ? `<span class="fcount">${t.unread}</span>` : ''}
        </div>
      `).join('')}
    </div>
  `, true);
}

async function openChatThread(listingId, otherId, otherName) {
  closeModal();
  let res;
  try {
    res = await api('thread', null, `&listing=${listingId || 0}&with=${otherId}`);
  } catch(e) { toast(e.message); return; }

  let lastId = res.messages.length ? Math.max(...res.messages.map(m => m.id)) : 0;
  let isBlocked = res.is_blocked;

  openModal(`
    <div class="mh">
      <div style="display:flex;align-items:center;gap:8px">
        <h3>${esc(otherName)} <small style="font-size:.7rem;color:#10B981">● Canlı</small></h3>
      </div>
      <div style="display:flex;align-items:center;gap:6px">
        <button class="btn btn-plain btn-sm" id="bBlockToggle" style="color:var(--err);font-size:.75rem">
          ${isBlocked ? 'Engeli Kaldır' : '🚫 Engelle'}
        </button>
        <button class="x" onclick="closeModal()">${I.x}</button>
      </div>
    </div>
    <div class="chat-box">
      <div class="chat-msgs" id="chatMsgs">
        ${res.messages.map(m => `
          <div class="msg ${m.sender_id === me.id ? 'me' : 'other'}">${esc(m.body)}</div>
        `).join('')}
      </div>
      <form class="chat-form" id="chatForm">
        <input id="chatInput" placeholder="${isBlocked ? 'Bu kullanıcı engellendi.' : 'Mesajınızı yazın...'}" ${isBlocked ? 'disabled' : ''} autocomplete="off">
        <button class="btn btn-or" type="submit" ${isBlocked ? 'disabled' : ''}>Gönder</button>
      </form>
    </div>
  `);

  $('#bBlockToggle').onclick = async () => {
    try {
      const bRes = await api('user_block_toggle', { target_user_id: otherId });
      toast(bRes.blocked ? 'Kullanıcı engellendi.' : 'Engel kaldırıldı.');
      closeModal();
    } catch(err) { toast(err.message); }
  };

  const msgsDiv = $('#chatMsgs');
  msgsDiv.scrollTop = msgsDiv.scrollHeight;

  if (chatPollTimer) clearInterval(chatPollTimer);
  chatPollTimer = setInterval(async () => {
    if (!$('#chatMsgs') || isBlocked) { clearInterval(chatPollTimer); return; }
    try {
      const pollRes = await api('thread', null, `&listing=${listingId || 0}&with=${otherId}&after=${lastId}`);
      if (pollRes.messages && pollRes.messages.length) {
        pollRes.messages.forEach(m => {
          msgsDiv.innerHTML += `<div class="msg ${m.sender_id === me.id ? 'me' : 'other'}">${esc(m.body)}</div>`;
          lastId = Math.max(lastId, m.id);
        });
        msgsDiv.scrollTop = msgsDiv.scrollHeight;
      }
    } catch(e) {}
  }, 2500);

  $('#chatForm').onsubmit = async (e) => {
    e.preventDefault();
    const val = $('#chatInput').value.trim();
    if (!val) return;
    try {
      const sent = await api('message_send', {to: otherId, listing_id: listingId, body: val});
      msgsDiv.innerHTML += `<div class="msg me">${esc(val)}</div>`;
      lastId = Math.max(lastId, sent.message.id);
      $('#chatInput').value = '';
      msgsDiv.scrollTop = msgsDiv.scrollHeight;
    } catch(err) {
      if (err.message === 'EARLY_BIRD_LOCKED') {
        openEarlyBirdModal();
      } else {
        toast(err.message);
      }
    }
  };
}

/* ------------------------------------------------ PROFİL PANELİ */
let profileData = null;

async function openProfile(activeTab = 'listings') {
  profileData = await api('profile');
  me = profileData.user;
  renderNav();

  openLayer('fs', `
  <div class="fs-top"><div>
    <h2 style="font-size:1.2rem;font-weight:800">Kullanıcı Kontrol Paneli</h2>
    <button class="btn btn-soft" id="pClose">${I.x}Kapat</button>
  </div></div>
  <div class="fs-in">
    <div class="p-header">
      <div class="p-user">
        ${avatar(me.name, me.avatar, 72, me.badge)}
        <div>
          <h2 style="font-size:1.5rem;font-weight:800;display:flex;align-items:center;gap:8px">
            ${esc(me.name)}
            ${me.is_verified ? '<span class="blue-tick" title="Doğrulanmış Profil">✓ Mavi Tik</span>' : ''}
            ${me.is_early_bird ? '<span class="early-badge">🚀 VIP Erken Erişim</span>' : ''}
            ${me.is_admin ? '<span style="background:var(--err);color:#fff;font-size:.68rem;padding:3px 9px;border-radius:6px;font-weight:800">YÖNETİCİ</span>' : ''}
          </h2>
          <p style="opacity:.85;font-size:.9rem">${esc(me.email)} · ${esc(me.phone || 'Telefon eklenmemiş')}</p>
          <div style="margin-top:6px;display:flex;gap:6px;flex-wrap:wrap">
            ${me.badge ? `<span style="background:rgba(255,255,255,.15);border:1px solid #FFE0A0;padding:4px 12px;border-radius:99px;font-size:.8rem;font-weight:700;color:#FFE0A0">⭐ Rozet: ${me.badge_days} gün kaldı</span>` : '<span style="background:rgba(255,255,255,.1);padding:4px 12px;border-radius:99px;font-size:.8rem">Standart Üyelik</span>'}
            ${me.avg_rating ? `<span style="background:#FEF3C7;color:#92400E;padding:4px 10px;border-radius:99px;font-size:.78rem;font-weight:800">⭐ ${me.avg_rating} (${me.review_count} Yorum)</span>` : ''}
          </div>
        </div>
      </div>
      <button class="btn btn-plain btn-sm" id="pOut" style="background:rgba(255,255,255,.1);color:#fff;border-color:rgba(255,255,255,.2)">Oturumu Kapat</button>
    </div>

    <div class="p-stat-grid">
      <div class="p-stat"><span>Aktif İlanlarım</span><b>${profileData.stats.listings}</b></div>
      <div class="p-stat"><span>Toplam İnceleme</span><b>${profileData.stats.views}</b></div>
      <div class="p-stat"><span>Favorilere Alınanlar</span><b>${me.fav_count || 0}</b></div>
    </div>

    <div class="p-tabs">
      <button class="p-tab ${activeTab==='listings'?'active':''}" onclick="switchPTab('listings')">📋 İlanlarım (${profileData.listings.length})</button>
      <button class="p-tab ${activeTab==='edit'?'active':''}" onclick="switchPTab('edit')">👤 Profil & Sosyal Medya</button>
      <button class="p-tab ${activeTab==='badge'?'active':''}" onclick="switchPTab('badge')">⭐ Rozet & VIP Üyelikler</button>
    </div>

    <div id="pTabContent" style="margin-top:20px"></div>
  </div>`);

  $('#pClose').onclick = closeFS;
  $('#pOut').onclick = async () => { await api('logout', {}); me = null; closeFS(); renderNav(); loadListings(); };
  switchPTab(activeTab);
}

function switchPTab(tab) {
  document.querySelectorAll('.p-tab').forEach(t => t.classList.remove('active'));
  const targetBtn = Array.from(document.querySelectorAll('.p-tab')).find(b => b.getAttribute('onclick').includes(tab));
  if (targetBtn) targetBtn.classList.add('active');

  const c = $('#pTabContent');
  if (tab === 'listings') {
    c.innerHTML = `
      <div class="ml">
        ${!profileData.listings.length ? '<div class="empty">Henüz bir ilanınız yok.<br><button class="btn btn-or btn-sm" style="margin-top:12px" onclick="openListingForm()">Hemen İlan Ver</button></div>' : profileData.listings.map(l => `
          <div class="mli" onclick="openDetail(${l.id})">
            <div class="mth">${l.cover ? `<img src="${imgSrc(l.cover)}">` : ''}</div>
            <div style="flex:1">
              <b>${esc(l.title)}</b> <small style="color:var(--muted)">(#${l.listing_no})</small><br>
              <small style="color:var(--muted)">${tl(l.price)} · ${esc(l.city)}</small>
            </div>
            <div style="display:flex;align-items:center;gap:6px">
              <span class="badge-status ${l.status}">${l.status === 'pending' ? '⏳ Onay Bekliyor' : '✅ Yayında'}</span>
              <button class="btn btn-or btn-sm" style="padding:4px 8px;font-size:.75rem" onclick="event.stopPropagation();openBoostModal(${l.id})">⚡ Başa Al</button>
            </div>
          </div>
        `).join('')}
      </div>
    `;
  } else if (tab === 'edit') {
    const ls = me.lifestyle || {};
    c.innerHTML = `
      <div style="background:var(--card);border:1px solid var(--line);border-radius:20px;padding:30px;max-width:680px">
        <h3 style="font-size:1.2rem;font-weight:800;margin-bottom:20px">Profil & Akıllı Eşleşme Kriterleri</h3>
        <div class="err" id="uErr"></div>
        
        <div style="display:flex;align-items:center;gap:16px;margin-bottom:24px;padding-bottom:20px;border-bottom:1px dashed var(--line)">
          ${avatar(me.name, me.avatar, 64, me.badge)}
          <div>
            <b style="display:block;margin-bottom:6px">Profil Fotoğrafı</b>
            <div style="display:flex;gap:8px">
              <input type="file" id="avInput" style="display:none" accept="image/*">
              <button class="btn btn-soft btn-sm" onclick="$('#avInput').click()">Fotoğraf Yükle</button>
              ${me.avatar ? `<button class="btn btn-plain btn-sm" style="color:var(--err)" onclick="delAvatar()">Kaldır</button>` : ''}
            </div>
          </div>
        </div>

        <div class="f"><label>Ad Soyad</label><input id="uName" value="${esc(me.name)}"></div>
        <div class="row2">
          <div class="f"><label>Telefon</label><input id="uPhone" value="${esc(me.phone || '')}" placeholder="05XX XXX XX XX"></div>
          <div class="f"><label>Yaşınız</label><input id="uAge" type="number" value="${ls.user_age || 22}"></div>
        </div>

        <div class="row2">
          <div class="f"><label>Instagram Kullanıcı Adı</label><input id="uInsta" value="${esc(me.instagram || '')}" placeholder="ornek (başında @ olmadan)"></div>
          <div class="f"><label>LinkedIn Profil ID</label><input id="uLink" value="${esc(me.linkedin || '')}" placeholder="ornek-kullanici"></div>
        </div>

        <div class="row2">
          <div class="f"><label>Bulunduğunuz İl</label>
            <select id="uCity">
              ${CITIES_81.map(c => `<option value="${c}" ${ls.city===c?'selected':''}>${c}</option>`).join('')}
            </select>
          </div>
          <div class="f"><label>Meslek / Durum</label>
            <select id="uOcc">
              <option value="Öğrenci" ${ls.occupation==='Öğrenci'?'selected':''}>🎓 Öğrenci</option>
              <option value="Çalışan" ${ls.occupation==='Çalışan'?'selected':''}>💼 Çalışan</option>
              <option value="Memur" ${ls.occupation==='Memur'?'selected':''}>🏛️ Memur</option>
            </select>
          </div>
        </div>

        <h4 style="margin:12px 0 8px;font-size:.9rem;color:var(--or-d)">⚡ Akıllı Eşleşme Tercihleriniz</h4>
        <div class="row3">
          <div class="f"><label>Sigara</label>
            <select id="uSmoking">
              <option value="Hayır" ${ls.smoking==='Hayır'?'selected':''}>Hayır</option>
              <option value="Evet" ${ls.smoking==='Evet'?'selected':''}>Evet</option>
            </select>
          </div>
          <div class="f"><label>Alkol</label>
            <select id="uAlcohol">
              <option value="Hayır" ${ls.alcohol==='Hayır'?'selected':''}>Hayır</option>
              <option value="Evet" ${ls.alcohol==='Evet'?'selected':''}>Evet</option>
            </select>
          </div>
          <div class="f"><label>Cinsiyet</label>
            <select id="uGender">
              <option value="Erkek" ${ls.gender==='Erkek'?'selected':''}>Erkek</option>
              <option value="Kadın" ${ls.gender==='Kadın'?'selected':''}>Kadın</option>
            </select>
          </div>
        </div>

        <div class="f"><label>Yeni Şifre (Değiştirmek istemiyorsanız boş bırakın)</label><input id="uPass" type="password" placeholder="En az 6 karakter"></div>
        <button class="btn btn-or btn-lg btn-block" id="uSave">Bilgileri Kaydet & Eşleşmeyi Güncelle</button>
      </div>
    `;

    $('#avInput').onchange = async () => {
      const f = $('#avInput').files[0];
      if (!f) return;
      const fd = new FormData();
      fd.append('avatar', f);
      try {
        await api('avatar_save', fd);
        toast('Profil fotoğrafı güncellendi! ✅');
        openProfile('edit');
      } catch(e) { showErr('#uErr', e.message); }
    };

    $('#uSave').onclick = async () => {
      try {
        await api('profile_update', {
          name: $('#uName').value,
          phone: $('#uPhone').value,
          instagram: $('#uInsta').value,
          linkedin: $('#uLink').value,
          password: $('#uPass').value,
          user_age: $('#uAge').value,
          city: $('#uCity').value,
          occupation: $('#uOcc').value,
          smoking: $('#uSmoking').value,
          alcohol: $('#uAlcohol').value,
          gender: $('#uGender').value
        });
        toast('Kriterleriniz kaydedildi! Uyum oranları yeniden hesaplandı. ✅');
        openProfile('edit');
      } catch(e) { showErr('#uErr', e.message); }
    };
  } else if (tab === 'badge') {
    const isPlanPlus = me.plan_type === 'plus' && me.is_plan_active;
    const isPlanStudent = me.plan_type === 'student' && me.is_plan_active;
    const isAnyActive = me.is_plan_active || me.badge;

    c.innerHTML = `
      <div style="text-align:center;max-width:720px;margin:0 auto 28px auto">
        <span style="background:var(--or-l);color:var(--or-d);padding:5px 14px;border-radius:20px;font-size:.78rem;font-weight:800;letter-spacing:.5px">AVANTAJLI ÜYELİK PLANLARI</span>
        <h2 style="font-size:1.7rem;font-weight:800;margin:12px 0 6px">KiraBöl Üyelik Paketleri</h2>
        <p style="color:var(--muted);font-size:.92rem">İhtiyacınıza uygun paketi seçin, ilanlarınızı en üste taşıyın ve 10 kat daha hızlı ev arkadaşı bulun.</p>
        ${isAnyActive ? `
          <div style="display:inline-flex;align-items:center;gap:8px;background:#ECFDF5;border:1px solid #A7F3D0;color:#065F46;padding:9px 18px;border-radius:14px;margin-top:14px;font-size:.88rem;font-weight:700">
            <span>✨ Aktif Planınız: <b>${isPlanStudent ? 'KiraBöl Öğrenci+ 🎓' : (isPlanPlus ? 'KiraBöl Plus+ ⭐' : 'Öne Çıkarma Rozeti')}</b> (${me.badge_days} gün kaldı)</span>
          </div>
        ` : ''}
      </div>

      <div class="pricing-grid">
        <!-- PLAN 1: FREE -->
        <div class="pricing-card ${!isAnyActive ? 'active-plan' : ''}">
          <div class="pricing-card-head">
            <span class="plan-tag">STANDART</span>
            <h3 style="font-size:1.3rem;font-weight:800;margin:8px 0">KiraBöl Free</h3>
            <div class="pricing-price">0 ₺ <small>/ daima</small></div>
            <p style="color:var(--muted);font-size:.82rem;margin-top:6px">Temel ev ve oda arama özellikleri.</p>
          </div>
          <div class="pricing-perks">
            <div class="perk-row"><span class="perk-ico ok">✓</span> <span>1 Adet İlan Yayınlama</span></div>
            <div class="perk-row"><span class="perk-ico ok">✓</span> <span>Harita & Akıllı Filtreleme</span></div>
            <div class="perk-row"><span class="perk-ico ok">✓</span> <span>Temel Mesajlaşma</span></div>
            <div class="perk-row muted"><span class="perk-ico">✕</span> <span>Öne Çıkarma & Altın Çerçeve</span></div>
            <div class="perk-row muted"><span class="perk-ico">✕</span> <span>48 Saat Erken Erişim VIP</span></div>
            <div class="perk-row muted"><span class="perk-ico">✕</span> <span>Ücretsiz İlanı Başa Al (Boost)</span></div>
          </div>
          <div class="pricing-act">
            <button class="btn btn-soft btn-block" disabled>${!isAnyActive ? '✓ Mevcut Planınız' : 'Ücretsiz Kullan'}</button>
          </div>
        </div>

        <!-- PLAN 2: PLUS+ -->
        <div class="pricing-card highlight ${isPlanPlus ? 'active-plan' : ''}">
          <div class="popular-ribbon">⭐ EN ÇOK TERCİH EDİLEN</div>
          <div class="pricing-card-head">
            <span class="plan-tag" style="background:var(--or-l);color:var(--or-d)">PREMIUM PLUS</span>
            <h3 style="font-size:1.3rem;font-weight:800;margin:8px 0">KiraBöl Plus+</h3>
            <div class="pricing-price">${CFG.plusPrice} ₺ <small>/ 30 gün</small></div>
            <p style="color:var(--muted);font-size:.82rem;margin-top:6px">Maksimum görünürlük ve hızlı eşleşme.</p>
          </div>
          <div class="pricing-perks">
            <div class="perk-row"><span class="perk-ico ok">✓</span> <b>Tüm Aramalarda En Tepede Sıralama</b></div>
            <div class="perk-row"><span class="perk-ico ok">✓</span> <b>Altın Işıltılı Özel İlan Çerçevesi</b></div>
            <div class="perk-row"><span class="perk-ico ok">✓</span> <b>48 Saat Erken Erişim VIP İlanlar</b></div>
            <div class="perk-row"><span class="perk-ico ok">✓</span> <b>1 Adet İlanı Başa Al Dopingi (Hediye)</b></div>
            <div class="perk-row"><span class="perk-ico ok">✓</span> <b>Sınırsız Sesli Arama & Mesajlaşma</b></div>
            <div class="perk-row"><span class="perk-ico ok">✓</span> <b>Onaylı Plus+ Güvenilirlik Rozeti</b></div>
          </div>
          <div class="pricing-act">
            ${isPlanPlus ? `
              <button class="btn btn-ok btn-block" disabled>✓ Aktif Plus+ Planınız</button>
            ` : `
              <button class="btn btn-or btn-block btn-lg" onclick="openPlanPayModal('plus')">Plus+ Satın Al (${CFG.plusPrice} ₺)</button>
            `}
          </div>
        </div>

        <!-- PLAN 3: ÖĞRENCİ+ -->
        <div class="pricing-card student ${isPlanStudent ? 'active-plan' : ''}">
          <div class="student-ribbon">🎓 %60 ÖĞRENCİ DESTEĞİ</div>
          <div class="pricing-card-head">
            <span class="plan-tag" style="background:#EFF6FF;color:#2563EB">ÖĞRENCİLERE ÖZEL</span>
            <h3 style="font-size:1.3rem;font-weight:800;margin:8px 0">KiraBöl Öğrenci+ 🎓</h3>
            <div class="pricing-price">${CFG.studentPrice} ₺ <small>/ 30 gün</small> <span class="old-price">${CFG.plusPrice} ₺</span></div>
            <p style="color:var(--muted);font-size:.82rem;margin-top:6px">Plus+ ile aynı güçte, öğrenci bütçesine özel.</p>
          </div>
          <div class="pricing-perks">
            <div class="perk-row"><span class="perk-ico ok">✓</span> <b>KiraBöl Plus+ ile Birebir Aynı Tüm Özellikler</b></div>
            <div class="perk-row"><span class="perk-ico ok">✓</span> <b>Tüm Aramalarda En Tepede Sıralama</b></div>
            <div class="perk-row"><span class="perk-ico ok">✓</span> <b>Altın Işıltılı Özel İlan Çerçevesi</b></div>
            <div class="perk-row"><span class="perk-ico ok">✓</span> <b>48 Saat Erken Erişim VIP İlanlar</b></div>
            <div class="perk-row"><span class="perk-ico ok">✓</span> <b>Sınırsız Sesli Arama & Mesajlaşma</b></div>
            <div class="perk-row"><span class="perk-ico ok">✓</span> <b>Doğrulanmış Öğrenci Rozeti</b></div>
          </div>
          <div class="pricing-act">
            ${isPlanStudent ? `
              <button class="btn btn-ok btn-block" disabled>✓ Aktif Öğrenci+ Planınız</button>
            ` : (me.student_status === 'approved' ? `
              <button class="btn btn-or btn-block btn-lg" onclick="openPlanPayModal('student')">Öğrenci+ Satın Al (${CFG.studentPrice} ₺)</button>
            ` : (me.student_status === 'pending' ? `
              <button class="btn btn-soft btn-block" disabled>⏳ Belgeniz İnceleniyor</button>
            ` : `
              <button class="btn btn-or btn-block btn-lg" onclick="openStudentVerifyModal()">🎓 Öğrenci Belgesi Yükle & Al</button>
            `))}
          </div>
        </div>
      </div>
    `;
  }
}

window.delAvatar = async () => {
  if (!confirm('Profil fotoğrafınız kaldırılsın mı?')) return;
  await api('avatar_delete', {});
  toast('Fotoğraf kaldırıldı.');
  openProfile('edit');
};

/* ------------------------------------------------ ADMIN YÖNETİM & MODERASYON */
let adminData = { listings: [], users: [], reports: [], tab: 'pending' };

async function openAdminPanel() {
  try {
    const [resListings, resUsers, resReports] = await Promise.all([
      api('admin_listings'),
      api('admin_users'),
      api('admin_reports')
    ]);
    adminData.listings = resListings.listings;
    adminData.users = resUsers.users;
    adminData.reports = resReports.reports;
  } catch(e) { toast(e.message); return; }

  const pending = adminData.listings.filter(l => l.status === 'pending');
  const pendingReports = adminData.reports.filter(r => r.status === 'pending');

  openLayer('fs', `
  <div class="fs-top"><div>
    <h2 style="font-size:1.3rem;font-weight:800;color:var(--err)">🛡️ Yönetici Moderasyon Merkezi</h2>
    <div style="display:flex;gap:10px;align-items:center">
      <button class="btn btn-or btn-sm" onclick="openAdminBroadcastModal()">📢 Toplu Mesaj Gönder</button>
      <button class="btn btn-soft" id="admClose">${I.x}Kapat</button>
    </div>
  </div></div>
  <div class="fs-in">
    <div class="p-stat-grid">
      <div class="p-stat"><span>Onay Bekleyen İlanlar</span><b style="color:var(--err)">${pending.length}</b></div>
      <div class="p-stat"><span>Bekleyen Şikayetler</span><b style="color:var(--err)">${pendingReports.length}</b></div>
      <div class="p-stat"><span>Toplam Kullanıcı</span><b>${adminData.users.length}</b></div>
      <div class="p-stat"><span>Toplam İlan</span><b>${adminData.listings.length}</b></div>
    </div>

    <div class="p-tabs" style="margin-bottom:20px">
      <button class="p-tab active" id="admTabPending" onclick="switchAdmTab('pending')">⏳ Onay Bekleyen İlanlar (${pending.length})</button>
      <button class="p-tab" id="admTabStudents" onclick="switchAdmTab('students')">🎓 Öğrenci Onayları</button>
      <button class="p-tab" id="admTabReports" onclick="switchAdmTab('reports')">🚨 Şikayet Masası (${pendingReports.length})</button>
      <button class="p-tab" id="admTabUsers" onclick="switchAdmTab('users')">👥 Kullanıcı Yönetimi (${adminData.users.length})</button>
      <button class="p-tab" id="admTabAllListings" onclick="switchAdmTab('allListings')">📋 Tüm İlanlar (${adminData.listings.length})</button>
    </div>

    <div id="admContent"></div>
  </div>`);

  $('#admClose').onclick = closeFS;
  renderAdmContent('pending');
}

function switchAdmTab(tab) {
  document.querySelectorAll('.p-tabs .p-tab').forEach(b => b.classList.remove('active'));
  if (tab === 'pending') $('#admTabPending').classList.add('active');
  if (tab === 'students') $('#admTabStudents').classList.add('active');
  if (tab === 'reports') $('#admTabReports').classList.add('active');
  if (tab === 'users') $('#admTabUsers').classList.add('active');
  if (tab === 'allListings') $('#admTabAllListings').classList.add('active');
  renderAdmContent(tab);
}

window.openAdminBroadcastModal = () => {
  openModal(`
    <div class="mh"><h3>📢 Tüm Kullanıcılara Toplu Mesaj / Duyuru</h3><button class="x" onclick="closeModal()">${I.x}</button></div>
    <p style="color:var(--muted);font-size:.88rem;margin-bottom:14px">
      Göndereceğiniz bu duyuru, platformdaki tüm kayıtlı kullanıcıların <b>Bildirimler</b> kutusuna anında iletilecektir.
    </p>
    <div class="err" id="bcErr"></div>
    <form id="fAdminBroadcast">
      <div class="f"><label>Duyuru Başlığı</label><input id="bcTitle" placeholder="Örn: KiraBöl Sistem Güncellemesi & Yenilikler 🚀" required></div>
      <div class="f"><label>Duyuru Mesajı</label><textarea id="bcMsg" rows="4" placeholder="Kullanıcılarımıza iletmek istediğiniz detaylı duyuru metnini yazın..." required></textarea></div>
      <button class="btn btn-or btn-block btn-lg" type="submit" style="margin-top:14px">
        🚀 Tüm Kullanıcılara Bildirimi Gönder
      </button>
    </form>
  `);

  $('#fAdminBroadcast').onsubmit = async (e) => {
    e.preventDefault();
    const title = $('#bcTitle').value.trim();
    const message = $('#bcMsg').value.trim();
    try {
      const res = await api('admin_broadcast', { title, message });
      closeModal();
      toast(`Duyurunuz ${res.count} kullanıcıya başarıyla iletildi! 📢`);
    } catch(err) { showErr('#bcErr', err.message); }
  };
};

window.adminStudentDecide = async (id, action) => {
  let reason = '';
  if (action === 'reject') {
    reason = prompt('Reddetme sebebini girin (Kullanıcıya bildirim olarak gidecektir):', 'Belge okunamadı veya geçerlilik süresi dolmuş.');
    if (reason === null) return;
  }
  try {
    await api('admin_student_decide', { id, action, reason });
    toast(action === 'approve' ? 'Öğrenci onaylandı! 🎓' : 'Öğrenci başvurusu reddedildi.');
    switchAdmTab('students');
  } catch(e) { toast(e.message); }
};

function renderAdmContent(tab) {
  const c = $('#admContent');
  const pending = adminData.listings.filter(l => l.status === 'pending');

  if (tab === 'pending') {
    c.innerHTML = `
      <h3 style="font-size:1.2rem;font-weight:800;margin-bottom:14px">Onay Bekleyen İlan Sırası</h3>
      ${!pending.length ? '<p style="color:var(--muted)">İncelenmeyi bekleyen onay sırası boş.</p>' : `
        <table class="adm-table">
          <thead><tr><th>İlan No</th><th>Başlık</th><th>Ekleyen</th><th>Fiyat</th><th>İşlemler</th></tr></thead>
          <tbody>
            ${pending.map(l => `
              <tr>
                <td><b>#${l.listing_no}</b></td>
                <td><b>${esc(l.title)}</b><br><small style="color:var(--muted)">${esc(l.city)} / ${esc(l.district)}</small></td>
                <td>${esc(l.owner)}<br><small style="color:var(--muted)">${esc(l.owner_email || '')}</small></td>
                <td><b>${tl(l.price)}</b></td>
                <td>
                  <div class="adm-acts">
                    <button class="btn btn-soft btn-sm" onclick="openDetail(${l.id})">Gör</button>
                    <button class="btn btn-or btn-sm" onclick="adminApprove(${l.id})">Onayla</button>
                    <button class="btn btn-plain btn-sm" style="color:var(--err)" onclick="adminReject(${l.id})">Reddet</button>
                  </div>
                </td>
              </tr>
            `).join('')}
          </tbody>
        </table>
      `}
    `;
  } else if (tab === 'students') {
    c.innerHTML = '<div style="padding:20px;text-align:center"><div class="spin" style="margin:0 auto"></div><p style="margin-top:10px;color:var(--muted)">Öğrenci belgeleri yükleniyor...</p></div>';
    api('admin_student_list').then(res => {
      const students = res.students || [];
      c.innerHTML = `
        <h3 style="font-size:1.2rem;font-weight:800;margin-bottom:14px">🎓 Öğrenci Belgesi Doğrulama Sırası</h3>
        ${!students.length ? '<p style="color:var(--muted)">İncelenmeyi bekleyen öğrenci belgesi bulunmuyor.</p>' : `
          <table class="adm-table">
            <thead><tr><th>Öğrenci</th><th>Üniversite & Bölüm</th><th>Belge</th><th>Durum</th><th>İşlemler</th></tr></thead>
            <tbody>
              ${students.map(s => {
                const sInfo = s.student_doc_info || {};
                const stBadge = s.student_status === 'approved' ? '<span class="match-badge match-high">Onaylı</span>' : (s.student_status === 'pending' ? '<span class="match-badge match-low">Onay Bekliyor</span>' : '<span class="match-badge" style="background:var(--err)">Reddedildi</span>');
                return `
                  <tr>
                    <td><b>${esc(s.name)}</b><br><small style="color:var(--muted)">${esc(s.email)} / ${esc(s.phone)}</small></td>
                    <td>
                      <b>${esc(sInfo.university || '-')}</b><br>
                      <small style="color:var(--muted)">${esc(sInfo.department || '-')}</small><br>
                      ${sInfo.barcode ? `<span style="font-size:.76rem;background:var(--line);padding:2px 6px;border-radius:6px;font-family:monospace">Barkod: ${esc(sInfo.barcode)}</span>` : ''}
                    </td>
                    <td>
                      <div style="display:flex;flex-direction:column;gap:4px">
                        ${s.student_doc_id ? `<a class="btn btn-soft btn-sm" href="?img=${s.student_doc_id}" target="_blank">📄 Belgeyi İncele</a>` : '<span style="color:var(--muted)">Belge Yok</span>'}
                        <a class="btn btn-plain btn-sm" href="https://www.turkiye.gov.tr/belge-dogrulama" target="_blank" style="color:var(--blue);font-size:.74rem">🔗 e-Devlet Doğrula</a>
                      </div>
                    </td>
                    <td>${stBadge}</td>
                    <td>
                      <div class="adm-acts">
                        ${s.student_status !== 'approved' ? `<button class="btn btn-ok btn-sm" onclick="adminStudentDecide(${s.id}, 'approve')">Onayla</button>` : ''}
                        ${s.student_status !== 'rejected' ? `<button class="btn btn-plain btn-sm" style="color:var(--err)" onclick="adminStudentDecide(${s.id}, 'reject')">Reddet</button>` : ''}
                      </div>
                    </td>
                  </tr>
                `;
              }).join('')}
            </tbody>
          </table>
        `}
      `;
    }).catch(e => { c.innerHTML = '<p style="color:var(--err)">Hata: ' + esc(e.message) + '</p>'; });
  } else if (tab === 'reports') {
    c.innerHTML = `
      <h3 style="font-size:1.2rem;font-weight:800;margin-bottom:14px">Gelen Şikayet & Bildirim Masası</h3>
      ${!adminData.reports.length ? '<p style="color:var(--muted)">Şu an için şikayet bildirimi bulunmuyor.</p>' : `
        <table class="adm-table">
          <thead><tr><th>Şikayet Eden</th><th>Şikayet Edilen İlan / Kişi</th><th>Sebep & Detay</th><th>Durum</th><th>İşlem</th></tr></thead>
          <tbody>
            ${adminData.reports.map(r => `
              <tr>
                <td><b>${esc(r.reporter_name)}</b><br><small style="color:var(--muted)">${esc(r.reporter_email)}</small></td>
                <td>
                  ${r.listing_id ? `<b>İlan #${r.listing_id}:</b> ${esc(r.listing_title)}<br>` : ''}
                  ${r.reported_name ? `<small>Kullanıcı: ${esc(r.reported_name)}</small>` : ''}
                </td>
                <td><b style="color:var(--err)">${esc(r.reason)}</b><br><small>${esc(r.details || 'Detay yazılmadı.')}</small></td>
                <td><span class="badge-status ${r.status==='pending'?'pending':'approved'}">${r.status==='pending'?'İnceleniyor':'Çözüldü'}</span></td>
                <td>
                  <div class="adm-acts">
                    ${r.listing_id ? `<button class="btn btn-soft btn-sm" onclick="openDetail(${r.listing_id})">İlanı Aç</button>` : ''}
                    ${r.status==='pending' ? `<button class="btn btn-line btn-sm" style="color:var(--ok);border-color:var(--ok)" onclick="adminResolveReport(${r.id})">✅ Kapat</button>` : ''}
                  </div>
                </td>
              </tr>
            `).join('')}
          </tbody>
        </table>
      `}
    `;
  } else if (tab === 'users') {
    c.innerHTML = `
      <h3 style="font-size:1.2rem;font-weight:800;margin-bottom:14px">Kayıtlı Tüm Kullanıcılar & Moderasyon</h3>
      <div style="overflow-x:auto">
        <table class="adm-table">
          <thead>
            <tr>
              <th>ID</th>
              <th>Kullanıcı Adı</th>
              <th>E-posta & Telefon</th>
              <th>Mavi Tik</th>
              <th>Rozet</th>
              <th>Uyarılar</th>
              <th>Durum</th>
              <th>İşlemler</th>
            </tr>
          </thead>
          <tbody>
            ${adminData.users.map(u => `
              <tr>
                <td>#${u.id}</td>
                <td>
                  <b>${esc(u.name)}</b>
                  ${u.is_verified ? '<span class="blue-tick">✓</span>' : ''}
                  ${u.is_admin ? '<span style="background:var(--err);color:#fff;font-size:.65rem;padding:2px 6px;border-radius:4px;margin-left:4px">YÖNETİCİ</span>' : ''}
                </td>
                <td>${esc(u.email)}<br><small style="color:var(--muted)">${esc(u.phone || 'Telefon yok')}</small></td>
                <td>${u.is_verified ? '<b style="color:var(--blue)">✓ Var</b>' : '<span style="color:var(--muted)">Yok</span>'}</td>
                <td>${u.badge ? '<span style="color:#D97706;font-weight:800">⭐ Rozetli</span>' : '<span style="color:var(--muted)">Yok</span>'}</td>
                <td>${u.warnings_count > 0 ? `<b style="color:var(--err)">⚠️ ${u.warnings_count} Uyarı</b>` : '<span style="color:var(--muted)">0</span>'}</td>
                <td>
                  ${u.is_banned 
                    ? `<span class="badge-status banned">🚫 BANLANDI</span><br><small style="color:var(--err);font-size:.72rem">${esc(u.ban_reason)}</small>`
                    : `<span class="badge-status approved">Aktif</span>`
                  }
                </td>
                <td>
                  <div class="adm-acts">
                    <button class="btn btn-soft btn-sm" onclick="adminToggleVerify(${u.id}, ${!u.is_verified})">${u.is_verified ? 'Tik Kaldır' : '✓ Mavi Tik'}</button>
                    <button class="btn btn-plain btn-sm" onclick="adminToggleBadge(${u.id}, ${!u.badge})">${u.badge ? 'Rozeti Al' : '⭐ Rozet'}</button>
                    <button class="btn btn-soft btn-sm" onclick="adminWarnUser(${u.id}, '${esc(u.name)}')">⚠️ Uyar</button>
                    ${!u.is_admin ? `
                      ${u.is_banned 
                        ? `<button class="btn btn-line btn-sm" style="color:var(--ok);border-color:var(--ok)" onclick="adminBanUser(${u.id}, false)">🟢 Aç</button>`
                        : `<button class="btn btn-plain btn-sm" style="color:var(--err)" onclick="adminBanUser(${u.id}, true)">🚫 Ban</button>`
                      }
                    ` : ''}
                  </div>
                </td>
              </tr>
            `).join('')}
          </tbody>
        </table>
      </div>
    `;
  } else if (tab === 'allListings') {
    c.innerHTML = `
      <h3 style="font-size:1.2rem;font-weight:800;margin-bottom:14px">Sistemdeki Tüm İlanlar</h3>
      <table class="adm-table">
        <thead><tr><th>İlan No</th><th>Kategori</th><th>Başlık</th><th>Ekleyen</th><th>Durum</th><th>İşlemler</th></tr></thead>
        <tbody>
          ${adminData.listings.map(l => `
            <tr>
              <td><b>#${l.listing_no}</b></td>
              <td><span class="ltag ${l.find_type==='looking_home'?'blue':''}">${l.find_type==='looking_home'?'👤 Ev Arıyor':'🏠 Evi Var'}</span></td>
              <td><b>${esc(l.title)}</b><br><small style="color:var(--muted)">${tl(l.price)} · ${esc(l.city)}</small></td>
              <td>${esc(l.owner)}</td>
              <td><span class="badge-status ${l.status}">${l.status === 'pending' ? 'Bekliyor' : l.status === 'approved' ? 'Yayında' : 'Reddedildi'}</span></td>
              <td>
                <div class="adm-acts">
                  <button class="btn btn-soft btn-sm" onclick="openDetail(${l.id})">İncele</button>
                  <button class="btn btn-plain btn-sm" style="color:var(--err)" onclick="adminDelete(${l.id})">Sil</button>
                </div>
              </td>
            </tr>
          `).join('')}
        </tbody>
      </table>
    `;
  }
}

window.adminToggleVerify = async (id, verify) => {
  try {
    await api('admin_user_verify', { id, verify });
    toast(verify ? 'Kullanıcıya Mavi Tik verildi! ✓' : 'Mavi Tik kaldırıldı.');
    openAdminPanel(); loadListings();
  } catch(e) { toast(e.message); }
};

window.adminResolveReport = async (id) => {
  try {
    await api('admin_report_resolve', { id });
    toast('Şikayet çözümlendi ve kapatıldı! ✅');
    openAdminPanel();
  } catch(e) { toast(e.message); }
};

window.adminWarnUser = async (id, name) => {
  const msg = prompt(`"${name}" adlı kullanıcıya iletilecek uyarı mesajını yazın:`);
  if (!msg || !msg.trim()) return;
  try {
    await api('admin_user_warn', { id, message: msg.trim() });
    toast('Uyarı kullanıcıya iletildi! ⚠️');
    openAdminPanel();
  } catch(e) { toast(e.message); }
};

window.adminBanUser = async (id, doBan) => {
  let reason = '';
  if (doBan) {
    reason = prompt('Kullanıcıyı banlama sebebini yazın:');
    if (reason === null) return;
  } else {
    if (!confirm('Bu kullanıcının banı kaldırılsın mı?')) return;
  }
  try {
    await api('admin_user_ban', { id, ban: doBan, reason });
    toast(doBan ? 'Kullanıcı banlandı ve ilanları gizlendi! 🚫' : 'Kullanıcının banı kaldırıldı! 🟢');
    openAdminPanel(); loadListings();
  } catch(e) { toast(e.message); }
};

window.adminToggleBadge = async (id, give) => {
  try {
    await api('admin_user_badge', { id, give });
    toast(give ? 'Kullanıcıya 30 günlük öne çıkarma rozeti verildi! ⭐' : 'Kullanıcının rozeti kaldırıldı.');
    openAdminPanel(); loadListings();
  } catch(e) { toast(e.message); }
};

window.adminApprove = async (id) => {
  try {
    await api('admin_approve', {id});
    toast('İlan onaylandı! ✅');
    if ($('#fs').classList.contains('show')) openAdminPanel();
    loadListings();
  } catch(e) { toast(e.message); }
};

window.adminReject = async (id) => {
  if (!confirm('İlan reddedilsin mi?')) return;
  try {
    await api('admin_reject', {id});
    toast('İlan reddedildi.');
    if ($('#fs').classList.contains('show')) openAdminPanel();
    loadListings();
  } catch(e) { toast(e.message); }
};

window.adminDelete = async (id) => {
  if (!confirm('Bu ilan kalıcı olarak silinsin mi?')) return;
  try {
    await api('listing_delete', {id});
    toast('İlan silindi.');
    openAdminPanel(); loadListings();
  } catch(e) { toast(e.message); }
};

/* ------------------------------------------------ GİRİŞ & İLAN VERME MODALLARI */
function openAuth(tab = 'login', note = '') {
  openModal(`
    <div class="mh"><h3 id="authTitle">${tab === 'login' ? 'Giriş Yap' : 'Kayıt Ol'}</h3><button class="x" onclick="closeModal()">${I.x}</button></div>
    ${note ? `<p style="color:var(--muted);margin-bottom:12px">${esc(note)}</p>` : ''}
    <div class="err" id="aErr"></div>
    <div id="regFields" style="display:${tab === 'register' ? 'block' : 'none'}">
      <div class="f"><label>Ad Soyad</label><input id="aName" placeholder="Adınız Soyadınız"></div>
      <div class="f"><label>Telefon</label><input id="aPhone" placeholder="05XX XXX XX XX"></div>
    </div>
    <div class="f"><label>E-posta</label><input id="aMail" type="email" placeholder="ornek@kirabol.com"></div>
    <div class="f"><label>Şifre</label><input id="aPass" type="password" placeholder="******"></div>
    <div style="display:flex;gap:10px;margin-top:12px">
      <button class="btn btn-or btn-block" id="bDoAuth">${tab === 'login' ? 'Giriş Yap' : 'Kayıt Ol'}</button>
    </div>
    <div style="text-align:center;margin-top:14px;font-size:.9rem">
      <a href="#" id="authToggle" style="color:var(--or-d);font-weight:700">
        ${tab === 'login' ? 'Hesabın yok mu? Kayıt Ol' : 'Zaten hesabın var mı? Giriş Yap'}
      </a>
    </div>
  `);

  let currentTab = tab;
  $('#authToggle').onclick = (e) => {
    e.preventDefault();
    currentTab = currentTab === 'login' ? 'register' : 'login';
    $('#authTitle').textContent = currentTab === 'login' ? 'Giriş Yap' : 'Kayıt Ol';
    $('#bDoAuth').textContent = currentTab === 'login' ? 'Giriş Yap' : 'Kayıt Ol';
    $('#regFields').style.display = currentTab === 'register' ? 'block' : 'none';
    $('#authToggle').textContent = currentTab === 'login' ? 'Hesabın yok mu? Kayıt Ol' : 'Zaten hesabın var mı? Giriş Yap';
  };

  $('#bDoAuth').onclick = async () => {
    try {
      if (currentTab === 'login') {
        me = (await api('login', {email: $('#aMail').value, password: $('#aPass').value})).user;
        toast('Giriş başarılı!');
      } else {
        me = (await api('register', {
          name: $('#aName').value,
          phone: $('#aPhone').value,
          email: $('#aMail').value,
          password: $('#aPass').value
        })).user;
        toast('Kayıt oluşturuldu ve giriş yapıldı!');
      }
      closeModal();
      renderNav();
      loadListings();
    } catch(e) {
      showErr('#aErr', e.message);
    }
  };
}

function openListingForm() {
  openModal(`
    <div class="mh"><h3>İlan Yayınla</h3><button class="x" onclick="closeModal()">${I.x}</button></div>
    <div class="err" id="nErr"></div>
    
    <div class="f">
      <label>İlan Türü / Ev Durumunuz</label>
      <select id="nFindType">
        <option value="has_home">🏠 Evim Var (Evime Ev Arkadaşı / Kiracı Arıyorum)</option>
        <option value="looking_home">👤 Evim Yok (Birlikte Ev Tutacak Arkadaş / Ev Arıyorum)</option>
        <option value="temporary">🧳 Kısa Süreli / Geçici Konaklama (Staj & Sınav Odası)</option>
      </select>
    </div>

    <div class="f"><label id="lblTitle">İlan Başlığı</label><input id="nTitle" placeholder="Örn: Kadıköy 2+1 evime ev arkadaşı arıyorum"></div>
    
    <div class="row2">
      <div class="f"><label>Tür</label><select id="nType">${TYPES.map(t=>`<option>${t}</option>`).join('')}</select></div>
      <div class="f"><label>Oda Sayısı</label><select id="nRooms">${ROOMS.map(r=>`<option>${r}</option>`).join('')}</select></div>
    </div>

    <div class="row2">
      <div class="f"><label>Şehir (81 İl)</label><select id="nCity">${CITIES_81.map(c=>`<option ${c==='İstanbul'?'selected':''}>${c}</option>`).join('')}</select></div>
      <div class="f"><label id="lblPrice">Fiyat (₺ / ay)</label><input id="nPrice" type="number" placeholder="15000"></div>
    </div>

    <div class="row2">
      <div class="f"><label>İlçe / Semt</label><input id="nDist" placeholder="Örn: Kadıköy"></div>
      <div class="f"><label>Yaşınız</label><input id="nUserAge" type="number" placeholder="Örn: 23"></div>
    </div>

    <div class="row2">
      <div class="f"><label>Mesleğiniz / Durumunuz</label>
        <select id="nOcc">
          <option value="Öğrenci">🎓 Öğrenci</option>
          <option value="Çalışan">💼 Çalışan</option>
          <option value="Memur">🏛️ Memur</option>
          <option value="Serbest Meslek">💻 Serbest Meslek</option>
        </select>
      </div>
      <div class="f"><label>Alkol Alışkanlığı</label>
        <select id="nAlcohol">
          <option value="Hayır">Hayır</option>
          <option value="Evet">Evet</option>
          <option value="Sosyal">Sosyal</option>
        </select>
      </div>
    </div>

    <div class="f">
      <label>🚇 En Yakın Metro / Metrobüs Durağı (Opsiyonel)</label>
      <input id="nMetro" placeholder="Örn: M2 Yenikapı Metrosuna 5 dk yürüme">
    </div>

    <div class="f">
      <label>📍 Haritadan Evin / Arayışın Konumunu Belirleyin <small style="color:var(--muted);font-weight:normal">(Pini taşımak için haritaya tıklayın)</small></label>
      <div id="pickerMap" style="height:190px;border-radius:12px;border:1.5px solid var(--line);overflow:hidden"></div>
      <input type="hidden" id="nLat" value="41.0082">
      <input type="hidden" id="nLng" value="28.9784">
    </div>

    <h4 style="margin:10px 0 6px;font-size:.9rem;color:var(--or-d)">🌱 Alışkanlıklar & Tercihler</h4>
    <div class="row3">
      <div class="f"><label>Sigara</label>
        <select id="nSmoking">
          <option value="Hayır">🚭 Hayır</option>
          <option value="Balkonda">Balkonda Serbest</option>
          <option value="Evet">Evet</option>
        </select>
      </div>
      <div class="f"><label>Evcil Hayvan</label>
        <select id="nPet">
          <option value="Kabul">🐾 Kabul Edilir</option>
          <option value="Yok">Kabul Edilmez</option>
        </select>
      </div>
      <div class="f"><label>Cinsiyet Tercihi</label>
        <select id="nGender">
          <option value="Erkek">Erkek</option>
          <option value="Kadın">Kadın</option>
          <option value="Fark Etmez">Fark Etmez</option>
        </select>
      </div>
    </div>

    <div id="amenitiesZone" class="f">
      <label>🛋️ Ev ve Oda Olanakları</label>
      <div class="amenity-grid">
        ${AMENITIES_LIST.map(a => `
          <label class="amenity-label"><input type="checkbox" name="amenity" value="${a}"> ${a}</label>
        `).join('')}
      </div>
    </div>

    <div class="f"><label>Fotoğraflar</label><input type="file" id="nPhotos" multiple accept="image/*"></div>
    <div class="f"><label>Açıklama</label><textarea id="nDesc" placeholder="Kendiniz, aradığınız ev veya ev arkadaşı kriterleri hakkında detaylar..."></textarea></div>
    <button class="btn btn-or btn-lg btn-block" id="nSave">İlanı Yayınla</button>
  `, true);

  $('#nFindType').onchange = () => {
    const isLooking = $('#nFindType').value === 'looking_home';
    $('#lblPrice').textContent = isLooking ? 'Aylık Bütçeniz (₺ / ay)' : 'Kira / Oda Bedeli (₺ / ay)';
    $('#amenitiesZone').style.display = isLooking ? 'none' : 'block';
  };

  setTimeout(() => {
    const defCity = $('#nCity').value;
    const defaultCoords = CITIES_COORDS[defCity] || [41.0082, 28.9784];
    $('#nLat').value = defaultCoords[0];
    $('#nLng').value = defaultCoords[1];

    const pMap = L.map('pickerMap').setView(defaultCoords, 12);
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png').addTo(pMap);
    let pMarker = L.marker(defaultCoords, {draggable: true}).addTo(pMap);

    const updateCoords = (lat, lng) => {
      $('#nLat').value = lat.toFixed(6);
      $('#nLng').value = lng.toFixed(6);
    };

    pMap.on('click', e => {
      pMarker.setLatLng(e.latlng);
      updateCoords(e.latlng.lat, e.latlng.lng);
    });
    pMarker.on('dragend', () => {
      const pos = pMarker.getLatLng();
      updateCoords(pos.lat, pos.lng);
    });

    $('#nCity').onchange = () => {
      const c = $('#nCity').value;
      const coords = CITIES_COORDS[c] || [39.0, 35.0];
      pMap.setView(coords, CITIES_COORDS[c] ? 12 : 7);
      pMarker.setLatLng(coords);
      updateCoords(coords[0], coords[1]);
    };
  }, 160);

  $('#nSave').onclick = async () => {
    const fd = new FormData();
    fd.append('title', $('#nTitle').value);
    fd.append('type', $('#nType').value);
    fd.append('rooms', $('#nRooms').value);
    fd.append('city', $('#nCity').value);
    fd.append('district', $('#nDist').value);
    fd.append('price', $('#nPrice').value);
    fd.append('description', $('#nDesc').value);
    fd.append('find_type', $('#nFindType').value);
    fd.append('user_age', $('#nUserAge').value);
    fd.append('occupation', $('#nOcc').value);
    fd.append('alcohol', $('#nAlcohol').value);
    fd.append('metro_station', $('#nMetro').value);
    fd.append('lat', $('#nLat').value);
    fd.append('lng', $('#nLng').value);
    fd.append('smoking', $('#nSmoking').value);
    fd.append('pet', $('#nPet').value);
    fd.append('gender_pref', $('#nGender').value);

    document.querySelectorAll('input[name="amenity"]:checked').forEach(ch => {
      fd.append('amenities[]', ch.value);
    });

    const files = $('#nPhotos').files;
    for (let i = 0; i < files.length; i++) fd.append('photos[]', files[i]);

    try {
      const res = await api('listing_save', fd);
      closeModal();
      if (res.status === 'pending') {
        alert('İlanınız başarıyla alındı! Yönetici onayının ardından sitede yayınlanacaktır.');
      } else {
        toast('İlanınız yayına alındı.');
      }
      loadListings();
    } catch(e) { showErr('#nErr', e.message); }
  };
}

/* ------------------------------------------------ BAŞLAT */
(async function init() {
  try { me = (await api('me')).user; if (me) initCallPoller(); } catch (e) { me = null; }
  renderNav();
  loadListings();
})();


/* ================= MODERN TAM EKRAN SOHBET & WEBRTC SESLİ ARAMA ================= */
let activeChatOtherId = null, activeChatListingId = null, activeChatName = '';
let peerConn = null, localStream = null, isCalling = false;
let pendingCallOffer = null, activeCallTargetUid = null;
let lastSignalId = 0, callPollTimer = null;
let fsChatPollTimer = null, lastMsgId = 0;
let allThreadsCache = [];

// Üst menüdeki mesajlar butonuna basıldığında TAM EKRAN AÇILIR
window.openConversations = async function() {
  $('#fsChatWrap').classList.add('open');
  loadFullScreenThreads();
  initCallPoller();
};

window.closeFullScreenChat = function() {
  $('#fsChatWrap').classList.remove('open');
  if (fsChatPollTimer) clearInterval(fsChatPollTimer);
  activeChatOtherId = null;
};

// Konuşma listesini yükle ve filtrele
async function loadFullScreenThreads() {
  try {
    const res = await api('conversations');
    allThreadsCache = res.threads || [];
    renderFsThreads(allThreadsCache);
  } catch(e) {}
}

function renderFsThreads(threads) {
  const container = $('#fsChatThreads');
  if (!threads.length) {
    container.innerHTML = '<div class="empty">Henüz bir mesajınız yok.</div>';
    return;
  }
  container.innerHTML = threads.map(t => {
    const isActive = activeChatOtherId === t.other;
    const timeStr = t.created_at ? new Date(t.created_at).toLocaleTimeString('tr-TR', { hour: '2-digit', minute: '2-digit' }) : '';
    const initial = (t.other_name || '?').charAt(0).toUpperCase();
    const avHtml = t.other_avatar ? `<img src="${imgSrc(t.other_avatar)}" alt="">` : `<div class="av-letter">${initial}</div>`;
    return `
      <div class="fs-thread-item ${isActive ? 'active' : ''}" onclick="openChatInMain(${t.listing_id || 0}, ${t.other}, '${esc(t.other_name)}')">
        <div class="fs-thread-av">
          ${avHtml}
          ${t.other_badge ? '<span class="rozet" style="width:16px;height:16px;top:auto;bottom:-2px;left:auto;right:-2px;font-size:.55rem">⭐</span>' : ''}
        </div>
        <div class="fs-thread-info">
          <div class="fs-thread-top">
            <span class="fs-thread-name">${esc(t.other_name)}</span>
            <span class="fs-thread-time">${timeStr}</span>
          </div>
          <div class="fs-thread-sub">
            <span class="fs-thread-body">${esc(t.body || '')}</span>
            ${t.unread ? `<span class="fs-thread-unread">${t.unread}</span>` : ''}
          </div>
        </div>
      </div>
    `;
  }).join('');
}

// Konuşma arama filtresi
window.filterFsThreads = function(val) {
  const q = val.trim().toLowerCase();
  if (!q) { renderFsThreads(allThreadsCache); return; }
  const filtered = allThreadsCache.filter(t => (t.other_name || '').toLowerCase().includes(q) || (t.body || '').toLowerCase().includes(q));
  renderFsThreads(filtered);
};

window.openChatInMain = async function(listingId, otherId, otherName) {
  activeChatOtherId = otherId;
  activeChatListingId = listingId;
  activeChatName = otherName;

  $('#fsChatMain').classList.add('active');
  $('#bBackToThreads').style.display = 'inline-flex';
  $('#fsChatHeaderActions').style.display = 'block';
  $('#fsChatForm').style.display = 'flex';
  $('#fsChatHeaderInfo').innerHTML = `
    <div style="display:flex;align-items:center;gap:10px">
      <div style="font-size:1.05rem;font-weight:800;color:var(--ink)">${esc(otherName)}</div>
      <small style="color:#10B981;font-weight:700">● Çevrimiçi</small>
    </div>
  `;

  $('#bStartVoiceCall').onclick = () => startVoiceCall(otherId, otherName);
  loadChatMessages(listingId, otherId);
  renderFsThreads(allThreadsCache);
};

function renderChatBubble(m) {
  const isMe = m.sender_id === me.id;
  const timeStr = m.created_at ? new Date(m.created_at).toLocaleTimeString('tr-TR', { hour: '2-digit', minute: '2-digit' }) : '';
  return `
    <div class="msg-row ${isMe ? 'me' : 'other'}">
      <div class="msg ${isMe ? 'me' : 'other'}">
        <div class="msg-text">${esc(m.body)}</div>
        <div class="msg-time">${timeStr}</div>
      </div>
    </div>
  `;
}

async function loadChatMessages(listingId, otherId) {
  if (fsChatPollTimer) clearInterval(fsChatPollTimer);
  try {
    const res = await api('thread', null, `&listing=${listingId || 0}&with=${otherId}`);
    const box = $('#fsChatMessages');
    const msgs = (res.messages || []).filter(m => !m.body.startsWith('[CALL_SIGNAL:'));
    lastMsgId = msgs.length ? Math.max(...msgs.map(m => m.id)) : 0;
    box.innerHTML = msgs.map(m => renderChatBubble(m)).join('') || '<div class="empty">Henüz bir mesaj yok. İlk mesajı yazın! 👋</div>';
    box.scrollTop = box.scrollHeight;
  } catch(e) {}

  // Canlı Polling: 2.5 saniyede bir yeni mesajları otomatik çek
  fsChatPollTimer = setInterval(async () => {
    if (!activeChatOtherId || !$('#fsChatWrap').classList.contains('open')) return;
    try {
      const pollRes = await api('thread', null, `&listing=${listingId || 0}&with=${otherId}&after=${lastMsgId}`);
      const newMsgs = (pollRes.messages || []).filter(m => !m.body.startsWith('[CALL_SIGNAL:'));
      if (newMsgs && newMsgs.length) {
        const box = $('#fsChatMessages');
        const emptyEl = box.querySelector('.empty');
        if (emptyEl) emptyEl.remove();
        newMsgs.forEach(m => {
          box.innerHTML += renderChatBubble(m);
          lastMsgId = Math.max(lastMsgId, m.id);
        });
        box.scrollTop = box.scrollHeight;
      }
    } catch(err) {}
  }, 2500);
}

// Mesaj Gönderme Fonksiyonu (Hem Enter hem de Buton için merkezi fonksiyon)
window.sendFsMessage = async function() {
  const input = $('#fsChatInput');
  if (!input) return;
  const val = input.value.trim();
  if (!val || !activeChatOtherId) return;

  try {
    const sent = await api('message_send', { to: activeChatOtherId, listing_id: activeChatListingId, body: val });
    const box = $('#fsChatMessages');
    if (box) {
      const emptyEl = box.querySelector('.empty');
      if (emptyEl) emptyEl.remove();

      box.innerHTML += renderChatBubble({
        sender_id: me.id,
        body: val,
        created_at: sent.message.created_at || new Date().toISOString()
      });
      lastMsgId = Math.max(lastMsgId, sent.message.id || 0);
      box.scrollTop = box.scrollHeight;
    }
    input.value = '';
    input.style.height = 'auto';
    loadFullScreenThreads();
  } catch(err) {
    if (err.message === 'EARLY_BIRD_LOCKED') {
      openEarlyBirdModal();
    } else {
      toast(err.message);
    }
  }
};


/* --- WEBRTC NUMARASIZ CANLI SESLİ GÖRÜŞME SİSTEMİ --- */
const rtcConfig = { iceServers: [{ urls: 'stun:stun.l.google.com:19302' }] };

// Global Arama Poller'ı (Gelen aramaları 3 saniyede bir dinler)
function initCallPoller() {
  if (callPollTimer) clearInterval(callPollTimer);
  callPollTimer = setInterval(async () => {
    if (!me) return;
    try {
      const res = await api('call_poll', null, `&after=${lastSignalId}`);
      const signals = res.signals || [];
      for (const sig of signals) {
        lastSignalId = Math.max(lastSignalId, sig.id);
        handleIncomingCallSignal(sig);
      }
    } catch(e) {}
  }, 3000);
}

// Gelen Arama Sinyallerini İşle
async function handleIncomingCallSignal(sig) {
  if (sig.type === 'offer') {
    if (isCalling) return; // Zaten meşgul
    pendingCallOffer = sig.data;
    activeCallTargetUid = sig.sender_id;
    isCalling = true;

    $('#callUserName').textContent = sig.sender_name || 'Kullanıcı';
    $('#callStatusText').textContent = 'Gelen Sesli Arama... 📞';
    $('#callAv').src = sig.sender_avatar ? imgSrc(sig.sender_avatar) : 'https://cdn-icons-png.flaticon.com/512/847/847969.png';
    $('#bAcceptCall').style.display = 'grid';
    $('#callModal').classList.add('active');
  } else if (sig.type === 'answer') {
    if (peerConn && isCalling) {
      await peerConn.setRemoteDescription(new RTCSessionDescription(sig.data));
      $('#callStatusText').textContent = 'Görüşme Devam Ediyor 🟢';
    }
  } else if (sig.type === 'candidate') {
    if (peerConn && sig.data) {
      try { await peerConn.addIceCandidate(new RTCIceCandidate(sig.data)); } catch(e) {}
    }
  } else if (sig.type === 'hangup') {
    endVoiceCall(false);
  }
}

// Aramayı Başlat (Arayan Taraf)
async function startVoiceCall(targetUid, targetName) {
  isCalling = true;
  activeCallTargetUid = targetUid;
  $('#callUserName').textContent = targetName;
  $('#callStatusText').textContent = 'Bağlanıyor / Aranıyor...';
  $('#bAcceptCall').style.display = 'none';
  $('#callModal').classList.add('active');

  try {
    localStream = await navigator.mediaDevices.getUserMedia({ audio: true, video: false });
    peerConn = new RTCPeerConnection(rtcConfig);
    localStream.getTracks().forEach(track => peerConn.addTrack(track, localStream));

    peerConn.ontrack = (event) => {
      $('#remoteAudio').srcObject = event.streams[0];
      $('#callStatusText').textContent = 'Görüşme Devam Ediyor 🟢';
    };

    peerConn.onicecandidate = (event) => {
      if (event.candidate) {
        api('call_signal', { to: targetUid, type: 'candidate', data: event.candidate });
      }
    };

    const offer = await peerConn.createOffer();
    await peerConn.setLocalDescription(offer);
    await api('call_signal', { to: targetUid, type: 'offer', data: offer });
  } catch (err) {
    alert('Mikrofon izni verilmedi veya arama başlatılamadı: ' + err.message);
    endVoiceCall();
  }
}

// Gelen Aramayı Kabul Et (Alıcı Taraf)
$('#bAcceptCall').onclick = async () => {
  $('#bAcceptCall').style.display = 'none';
  $('#callStatusText').textContent = 'Bağlantı kuruluyor...';

  try {
    localStream = await navigator.mediaDevices.getUserMedia({ audio: true, video: false });
    peerConn = new RTCPeerConnection(rtcConfig);
    localStream.getTracks().forEach(track => peerConn.addTrack(track, localStream));

    peerConn.ontrack = (event) => {
      $('#remoteAudio').srcObject = event.streams[0];
      $('#callStatusText').textContent = 'Görüşme Devam Ediyor 🟢';
    };

    peerConn.onicecandidate = (event) => {
      if (event.candidate && activeCallTargetUid) {
        api('call_signal', { to: activeCallTargetUid, type: 'candidate', data: event.candidate });
      }
    };

    await peerConn.setRemoteDescription(new RTCSessionDescription(pendingCallOffer));
    const answer = await peerConn.createAnswer();
    await peerConn.setLocalDescription(answer);
    await api('call_signal', { to: activeCallTargetUid, type: 'answer', data: answer });
    $('#callStatusText').textContent = 'Görüşme Devam Ediyor 🟢';
  } catch (err) {
    alert('Arama yanıtlanamadı veya mikrofon izni verilmedi: ' + err.message);
    endVoiceCall();
  }
};

$('#bHangupCall').onclick = () => endVoiceCall(true);

function endVoiceCall(notifyPeer = true) {
  const target = activeChatOtherId || activeCallTargetUid;
  if (notifyPeer && target) {
    api('call_signal', { to: target, type: 'hangup', data: {} });
  }
  if (localStream) localStream.getTracks().forEach(t => t.stop());
  if (peerConn) peerConn.close();
  peerConn = null; localStream = null; isCalling = false;
  pendingCallOffer = null; activeCallTargetUid = null;
  $('#callModal').classList.remove('active');
  $('#bAcceptCall').style.display = 'none';
}
</script>

<!-- TAM EKRAN SOHBET ALANI -->
<div class="fs-chat-wrap" id="fsChatWrap">
  <div class="fs-chat-sidebar">
    <div class="fs-chat-sidebar-head">
      <h3 style="font-size:1.2rem;font-weight:800;color:var(--ink)">💬 Sohbetler</h3>
      <button class="btn btn-plain btn-sm" onclick="closeFullScreenChat()">✕ Kapat</button>
    </div>
    <div class="fs-chat-search">
      <input placeholder="Sohbetlerde veya kişilerde ara..." oninput="filterFsThreads(this.value)">
    </div>
    <div class="fs-chat-threads" id="fsChatThreads"></div>
  </div>
  <div class="fs-chat-main" id="fsChatMain">
    <div class="fs-chat-header" id="fsChatHeader">
      <div style="display:flex;align-items:center;gap:12px">
        <button class="btn btn-plain btn-sm" id="bBackToThreads" style="display:none" onclick="$('#fsChatMain').classList.remove('active')">← Geri</button>
        <div id="fsChatHeaderInfo"><b>Bir sohbet seçin</b></div>
      </div>
      <div id="fsChatHeaderActions" style="display:none">
        <button class="btn btn-or btn-sm" id="bStartVoiceCall" style="box-shadow:0 4px 14px rgba(255,102,0,.25)">📞 Sesli Ara</button>
      </div>
    </div>
    <div class="fs-chat-messages" id="fsChatMessages">
      <div class="empty">Konuşmak için soldan bir kişi seçin.</div>
    </div>
    <form class="fs-chat-input-bar" id="fsChatForm" style="display:none" onsubmit="event.preventDefault();">
      <div class="fs-chat-input-box">
        <textarea id="fsChatInput" class="fs-chat-textarea" placeholder="Mesaj gir" rows="1" onkeydown="if(event.key==='Enter'&&!event.shiftKey){event.preventDefault();sendFsMessage();}" oninput="this.style.height='auto';this.style.height=Math.min(this.scrollHeight,120)+'px'"></textarea>
      </div>
      <button class="btn-chat-send" type="button" onclick="sendFsMessage()" title="Gönder">
        <svg viewBox="0 0 24 24" width="20" height="20" fill="none" stroke="currentColor" stroke-width="2.3" stroke-linecap="round" stroke-linejoin="round">
          <line x1="22" y1="2" x2="11" y2="13"></line>
          <polygon points="22 2 15 22 11 13 2 9 22 2"></polygon>
        </svg>
      </button>
    </form>
  </div>
</div>

<!-- SESLİ GÖRÜŞME EKRANI -->
<div class="call-modal" id="callModal">
  <img class="call-avatar" id="callAv" src="https://cdn-icons-png.flaticon.com/512/847/847969.png">
  <h3 id="callUserName" style="font-size:1.25rem;font-weight:800">Kullanıcı</h3>
  <span id="callStatusText" style="font-size:.85rem;opacity:.8;margin-top:4px">Sesli arama başlatılıyor...</span>
  <div class="call-acts">
    <button class="btn-call-act btn-call-accept" id="bAcceptCall" style="display:none">📞</button>
    <button class="btn-call-act btn-call-hangup" id="bHangupCall">✕</button>
  </div>
  <audio id="remoteAudio" autoplay></audio>
</div>
</body>
</html>
