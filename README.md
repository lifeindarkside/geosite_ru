# 
# GeoSite RU
Предоставление доступа к заблокированным извне Русским сайтам.
## Установка 

Создаем папку 
```
mkdir -p /var/lib/marzban/assets/
```

Скачиваем 
```
wget -O /var/lib/marzban/assets/ru.dat FILELINK
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
