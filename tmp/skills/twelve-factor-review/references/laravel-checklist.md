# Laravel Twelve-Factor Checklist

## Factor I: Codebase

**Principle**: One codebase tracked in revision control, many deploys

### Check Items
- [ ] Single Git repository for the application
- [ ] No code shared via copy-paste between apps (use Composer packages instead)
- [ ] `.gitignore` properly excludes: `vendor/`, `.env`, `storage/`, `node_modules/`

### Anti-patterns
```php
// ❌ Hardcoded environment-specific paths
$path = '/var/www/production/storage';

// ✅ Use Laravel helpers
$path = storage_path();
```

---

## Factor II: Dependencies

**Principle**: Explicitly declare and isolate dependencies

### Check Items
- [ ] All PHP dependencies in `composer.json`
- [ ] `composer.lock` committed to repo
- [ ] No reliance on system-wide packages (global Composer installs)
- [ ] PHP extensions declared in `composer.json` require section
- [ ] Frontend deps in `package.json` with lockfile

### Anti-patterns
```json
// ❌ Missing extension requirements
{
    "require": {
        "php": "^8.1"
    }
}

// ✅ Declare required extensions
{
    "require": {
        "php": "^8.1",
        "ext-pdo": "*",
        "ext-redis": "*",
        "ext-json": "*"
    }
}
```

---

## Factor III: Config

**Principle**: Store config in the environment

### Check Items
- [ ] All environment-specific values use `env()` function
- [ ] No hardcoded credentials, API keys, or URLs in code
- [ ] `.env.example` documents all required variables
- [ ] Config files use `env()` with sensible defaults
- [ ] No `env()` calls outside of `config/*.php` files

### Anti-patterns
```php
// ❌ Hardcoded credentials
$apiKey = 'sk-abc123';

// ❌ env() in application code
$debug = env('APP_DEBUG');

// ✅ Use config
$apiKey = config('services.api.key');
$debug = config('app.debug');
```

```php
// config/services.php
// ✅ env() only in config files with defaults
'api' => [
    'key' => env('API_KEY'),
    'url' => env('API_URL', 'https://api.example.com'),
],
```

---

## Factor IV: Backing Services

**Principle**: Treat backing services as attached resources

### Check Items
- [ ] Database connections configured via env vars (`DB_HOST`, `DB_DATABASE`, etc.)
- [ ] Cache driver configurable (`CACHE_DRIVER`)
- [ ] Queue connection configurable (`QUEUE_CONNECTION`)
- [ ] Mail service configurable (`MAIL_MAILER`)
- [ ] No hardcoded service endpoints
- [ ] Services swappable without code changes

### Anti-patterns
```php
// ❌ Hardcoded Redis host
$redis = new Redis();
$redis->connect('192.168.1.100', 6379);

// ✅ Use Laravel's Redis facade with config
use Illuminate\Support\Facades\Redis;
Redis::set('key', 'value');
```

---

## Factor V: Build, Release, Run

**Principle**: Strictly separate build and run stages

### Check Items
- [ ] Build artifacts not in source control (`/public/build/`, compiled assets)
- [ ] `composer install --no-dev` for production
- [ ] `php artisan config:cache` in deployment
- [ ] `php artisan route:cache` in deployment
- [ ] No runtime compilation of views in production

### Deployment Script Example
```bash
# ✅ Proper deployment separation
composer install --no-dev --optimize-autoloader
npm ci && npm run build
php artisan config:cache
php artisan route:cache
php artisan view:cache
php artisan migrate --force
```

---

## Factor VI: Processes

**Principle**: Execute the app as one or more stateless processes

### Check Items
- [ ] Session driver is NOT `file` in production (`SESSION_DRIVER=redis|database|memcached`)
- [ ] Cache driver is NOT `file` for multi-server (`CACHE_DRIVER=redis|memcached`)
- [ ] No local filesystem for user uploads (use S3, etc.)
- [ ] No sticky sessions required
- [ ] No in-memory state between requests

### Anti-patterns
```php
// ❌ Storing state in static properties
class Counter {
    public static int $count = 0;

    public function increment() {
        return ++self::$count; // Lost between requests!
    }
}

// ❌ File-based sessions in multi-server
// .env: SESSION_DRIVER=file

// ✅ Redis sessions
// .env: SESSION_DRIVER=redis
```

---

## Factor VII: Port Binding

**Principle**: Export services via port binding

### Check Items
- [ ] App is self-contained (doesn't require Apache/Nginx to run)
- [ ] Can run via `php artisan serve`
- [ ] Health check endpoint available

### Notes
Laravel inherently supports this via `artisan serve`. For production, use PHP-FPM with Nginx/Apache as a reverse proxy.

---

## Factor VIII: Concurrency

**Principle**: Scale out via the process model

### Check Items
- [ ] Long-running tasks use Queue jobs, not synchronous processing
- [ ] Queue workers can be scaled horizontally
- [ ] Scheduled tasks use Laravel Scheduler (`php artisan schedule:run`)
- [ ] No reliance on single-process state

### Anti-patterns
```php
// ❌ Synchronous heavy processing in controller
public function processReport(Request $request)
{
    $this->generateLargeReport(); // Blocks for minutes
    return response()->json(['status' => 'done']);
}

// ✅ Queue the job
public function processReport(Request $request)
{
    GenerateReportJob::dispatch($request->user());
    return response()->json(['status' => 'queued']);
}
```

---

## Factor IX: Disposability

**Principle**: Maximize robustness with fast startup and graceful shutdown

### Check Items
- [ ] Fast application boot time
- [ ] Queue workers handle `SIGTERM` gracefully (`--timeout` option)
- [ ] Database transactions are short-lived
- [ ] No long-running locks on startup

### Queue Worker Config
```bash
# ✅ Graceful shutdown with timeout
php artisan queue:work --timeout=60 --tries=3
```

### Anti-patterns
```php
// ❌ Long boot-time operations in ServiceProvider
public function boot()
{
    $this->loadAllDataFromApi(); // Slow startup!
}
```

---

## Factor X: Dev/Prod Parity

**Principle**: Keep development, staging, and production as similar as possible

### Check Items
- [ ] Same database engine in dev and prod (not SQLite in dev, MySQL in prod)
- [ ] Same queue driver in dev and prod (not `sync` in dev, `redis` in prod)
- [ ] Same cache driver in dev and prod
- [ ] Docker/containerized development environment matches production
- [ ] `.env.example` reflects production config structure

### Anti-patterns
```env
# ❌ Different drivers per environment
# Dev: QUEUE_CONNECTION=sync
# Prod: QUEUE_CONNECTION=redis

# ✅ Use Redis in both, or use a local Redis in dev
# Dev: QUEUE_CONNECTION=redis (with local Redis)
# Prod: QUEUE_CONNECTION=redis
```

---

## Factor XI: Logs

**Principle**: Treat logs as event streams

### Check Items
- [ ] Logs written to `stdout`/`stderr` in production (`LOG_CHANNEL=stderr`)
- [ ] No log files in production (use log aggregation service)
- [ ] Use `Log` facade, not `error_log()` or `echo`
- [ ] Structured logging (JSON format) for log aggregation

### Anti-patterns
```php
// ❌ Direct file writing
file_put_contents('/var/log/app.log', $message);

// ❌ echo/print for logging
echo "Processing user: " . $userId;

// ✅ Use Log facade
use Illuminate\Support\Facades\Log;
Log::info('Processing user', ['user_id' => $userId]);
```

### Production Config
```php
// config/logging.php
'channels' => [
    'stderr' => [
        'driver' => 'monolog',
        'handler' => StreamHandler::class,
        'with' => [
            'stream' => 'php://stderr',
        ],
        'formatter' => JsonFormatter::class, // Structured logs
    ],
],
```

---

## Factor XII: Admin Processes

**Principle**: Run admin/management tasks as one-off processes

### Check Items
- [ ] Migrations via `php artisan migrate`
- [ ] One-off tasks as Artisan commands
- [ ] No admin tasks via web routes
- [ ] REPL available (`php artisan tinker`)
- [ ] Admin commands can run in same environment as app

### Anti-patterns
```php
// ❌ Admin task via web route
Route::get('/admin/cleanup', function () {
    DB::table('old_data')->delete();
    return 'Done';
});

// ✅ Artisan command
class CleanupOldDataCommand extends Command
{
    protected $signature = 'app:cleanup-old-data';

    public function handle()
    {
        DB::table('old_data')->delete();
        $this->info('Cleanup complete');
    }
}
```
