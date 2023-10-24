# 
# GeoSite RU
Предоставление доступа к заблокированным извне Русским сайтам.
## Установка для сервера

Создаем папку 
```
mkdir -p /var/lib/marzban/assets/
```

Скачиваем 
```
wget -O /var/lib/marzban/assets/ru.dat https://github.com/Iambabyninja/geosite_ru/releases/latest/download/ru.dat
```

Устанавливаем значение в .env файле
```
XRAY_ASSETS_PATH = "/var/lib/marzban/assets/"
```

Редактируем xray_config.json

```
    "routing": {
        "domainStrategy": "IPIfNonMatch",
        "rules": [
            {
                "type": "field",
                "outboundTag": "blackhole",
                "ip": [
                    "geoip:private",
                    "geoip:ru"
                ]
            },
            {
                "type": "field",
                "port": 53,
                "network": "tcp,udp",
                "outboundTag": "DNS-Internal"
            },
            {
                "type": "field",
                "outboundTag": "blackhole",
                "protocol": [
                    "bittorrent"
                ]
            },
            {
                "outboundTag": "blackhole",
                "domain": [
                    "regexp:.*\\.ru$",
                    "ext:ru.dat:all"
                ],
                "type": "field"
            }
        ]
    }
```

## Установка для узла

Создаем папку 
```
mkdir -p /var/lib/marzban/assets/
```

Скачиваем 
```
wget -O /var/lib/marzban/assets/geosite.dat https://github.com/v2fly/domain-list-community/releases/latest/download/dlc.dat
```
```
wget -O /var/lib/marzban/assets/geoip.dat https://github.com/v2fly/geoip/releases/latest/download/geoip.dat
```
```
wget -O /var/lib/marzban/assets/ru.dat https://github.com/Iambabyninja/geosite_ru/releases/latest/download/ru.dat
```

Переходим в папку контейнера
```
cd /opt/Marzban-node
```

Редактируем файл docker-compose.yml
```
nano docker-compose.yml
```

Файл docker-compose.yml должен выглядеть так

```
services:
  marzban-node:
    # build: .
    image: gozargah/marzban-node:latest
    restart: always
    network_mode: host

    volumes:
      - /var/lib/marzban-node:/var/lib/marzban-node
      - /var/lib/marzban/assets:/usr/local/share/xray
```

Пересоздаем контейнер

```
docker compose down && docker compose up -d
```
