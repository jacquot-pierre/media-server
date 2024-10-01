# media-server
My media-server powered by ombi, sonarr, radardd and transmission

## Qbitorrent

```yaml
  qbittorrent:
    image: lscr.io/linuxserver/qbittorrent:4.6.2
    container_name: qbittorrent
    network_mode: "service:gluetun"
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Etc/UTC
      - WEBUI_PORT=9092
    deploy:
      resources:
        limits:
          cpus: "0.5"
          memory: 256M
        reservations:
          memory: 128M
    volumes:
      - /qbittorrent/config:/config
      - /data/qbittorrent/downloads:/downloads/complete
      - /data/qbittorrent/downloads/incomplete:/downloads/incomplete
    restart: unless-stopped
    labels:
      - traefik.enable=true
      - traefik.http.routers.qbittorrent.rule=Host(`qbittorrent.seigneurguerande.fr`)
      - traefik.http.services.qbittorrent.loadbalancer.server.port=9092
      - traefik.http.routers.qbittorrent.entrypoints=websecure
      - traefik.http.routers.qbittorrent.tls.certresolver=myresolver
      - "traefik.http.routers.qbittorrent.middlewares=local-ipwhitelist"
      - homepage.group=Tools
      - homepage.name=qbittorrent
      - homepage.icon=qbittorrent.png
      - homepage.href=https://qbittorrent.seigneurguerande.fr
      - homepage.description=Qbittorrent server
      - homepage.widget.type=qbittorrent
      - homepage.widget.url=https://qbittorrent.seigneurguerande.fr
      - homepage.widget.username=
      - homepage.widget.password=
      - homepage.widget.fields=["leech", "download", "seed", "upload"]
```