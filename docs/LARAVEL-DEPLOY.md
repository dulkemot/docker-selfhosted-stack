# Pola Deploy Laravel di Server yang Sama

Stack ini + 1 container app. Alur yang dipakai di proyek hospitality/klien.

## 1. Build image sekali, jalan di mana saja

```dockerfile
FROM php:8.3-fpm-alpine
RUN docker-php-ext-install pdo_mysql opcache
COPY . /var/www
WORKDIR /var/www
```

## 2. Service tambahan di compose

```yaml
  app:
    image: registry/app-klien:latest
    restart: unless-stopped
    env_file: .env.app
```

## 3. Rilis aman (checklist)

1. `php artisan migrate --force` (backup DB dulu!)
2. `php artisan config:cache && route:cache && view:cache`
3. `php artisan queue:restart` (bila pakai queue worker)
4. Cek Uptime Kuma — harus tetap hijau selama rilis
5. Rollback = deploy ulang tag image sebelumnya

## 4. Queue & scheduler

- Queue: 1 container `php artisan queue:work --tries=3`
- Scheduler: cron host `* * * * * docker exec app php artisan schedule:run`
