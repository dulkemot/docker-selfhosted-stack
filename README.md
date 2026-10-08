# Docker Self-Hosted Stack 🇮🇩

Satu `docker-compose.yml` untuk server kecil (VPS / Mini PC / Proxmox LXC): reverse proxy HTTPS otomatis (Caddy), monitoring (Prometheus + Grafana + Node Exporter), dan uptime monitor (Uptime Kuma).

![Docker](https://img.shields.io/badge/docker-compose-blue?style=for-the-badge&logo=docker)
![License](https://img.shields.io/badge/license-MIT-blue?style=for-the-badge)

## Layanan

| URL | Fungsi |
|---|---|
| `https://domainmu.id` | Reverse proxy Caddy (HTTPS otomatis Let's Encrypt) |
| `:3001` | Uptime Kuma — monitor HTTP/ping + alert Telegram |
| `:3000` | Grafana — dashboard (admin / isi `GRAFANA_PASSWORD`) |
| `:9090` | Prometheus — simpan metrics 15 hari |

## Cara pakai

```bash
cp .env.example .env   # isi domain + password
docker compose up -d
docker compose ps
```

Routing domain diatur di `Caddyfile`:
```
uptime.domainmu.id {
  reverse_proxy uptime-kuma:3001
}
```

## Struktur

```
docker-compose.yml      # semua service
Caddyfile               # routing domain
.env.example            # variabel wajib
prometheus/prometheus.yml  # target scrape
docs/CLOUDFLARE-TUNNEL.md  # alternatif tanpa buka port 80/443
docs/LARAVEL-DEPLOY.md     # pola deploy Laravel di server yang sama
```

## Catatan

- Butuh port 80/443 terbuka untuk HTTPS otomatis. Di balik CGNAT? Pakai Cloudflare Tunnel (lihat docs).
- Backup volume: `docker run --rm -v <vol>:/data -v $(pwd):/b alpine tar czf /b/vol-backup.tar.gz /data`

MIT — dulkemot 2026
