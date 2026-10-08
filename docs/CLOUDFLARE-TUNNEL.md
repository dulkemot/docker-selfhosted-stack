# Cloudflare Tunnel (alternatif tanpa buka port)

Cocok untuk server di balik CGNAT / IndiHome: tidak perlu port forwarding maupun IP publik.

## Langkah

1. Domain sudah di Cloudflare. Install `cloudflared` di server (atau tambah service di compose).
2. Login dan buat tunnel:
```bash
cloudflared tunnel login
cloudflared tunnel create server-lampung
cloudflared tunnel route dns server-lampung uptime.domainmu.id
```
3. Jalankan tunnel mengarah ke Caddy:
```bash
cloudflared tunnel --url http://localhost:80 run server-lampung
```
Atau sebagai service compose:
```yaml
  tunnel:
    image: cloudflare/cloudflared:latest
    restart: unless-stopped
    command: tunnel --no-autoupdate run --token ${CLOUDFLARE_TOKEN}
```
4. Di Cloudflare Zero Trust → Networks → Tunnels, atur Public Hostname ke service `http://caddy:80`.

## Catatan

- TLS terminasi di Cloudflare; di Caddy cukup HTTP biasa.
- Aktifkan **Authenticated Origin Pulls** bila perlu keamanan ekstra.
