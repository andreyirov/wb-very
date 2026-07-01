# wb-very

Конфигурация умного дома: **Wiren Board** (освещение по MQTT) + **Home Assistant** (панели, автоматизации) + **ESPHome** (бризеры).

## Структура репозитория

| Путь | Назначение |
|------|-----------|
| `etc/` | Конфиги Wiren Board (`wb-mqtt-serial.conf` и др.) |
| `very-home_assistant/` | Home Assistant: `configuration.yaml`, `packages/`, `dashboards/`, `.storage/` |
| `very-esphome/` | ESPHome-конфиги бризеров (`*.yml`) |

MQTT-брокер в HA: **192.168.180.10** (wb8).
Панели Lovelace: **Освещение** (весь свет по помещениям) и **Климат** (бризеры).

Помещения (areas): Кухня-гостиная, Спальня-кабинет, Детская, Ванная, Прачечная, Коридор.

CCT-ленты: диапазон цветовой температуры в HA — **2700–4000 K** (регистр WB 0–100 → K).

## Освещение: Wiren Board ↔ Home Assistant

Сущности `light.*`, определены в `very-home_assistant/homeassistant/packages/wirenboard.yaml`.

### Кухня-гостиная

| Зона | WB | HA entity |
|------|-----|-----------|
| Подсветка витрины (CCT) | wb-led_16 CCT1 (ch 1+2) | `light.kukhnia_vitrina` |
| Зона готовки (W) | wb-led_16 ch 3 | `light.kukhnia_zona_gotovki` |
| Потолочные светильники (кухня) | wb-mdm3_175 K2 | `light.gostinaia_potolok` |
| Потолочные светильники (гостиная) | wb-mdm3_175 K3 | `light.kukhnia_potolok` |
| Люстра над столом | wb-mdm3_124 K1 | `light.gostinaia_liustra_stol` |
| Окно 2 — шторы | wb-led_25 CCT1 | `light.gostinaia_shtory_okno2` |
| Окно 3 — шторы | wb-led_25 CCT2 | `light.gostinaia_shtory_okno3` |

wb-led_16 `dimmer_mode = 2` (CCT + W + W): ch 1+2 → CCT1, ch 3 → W, ch 4 → W.
Потолки кухни/гостиной: поменяны местами только подписи, привязка entity_id к каналам сохранена
(`light.kukhnia_potolok` = K3 = гостиная, `light.gostinaia_potolok` = K2 = кухня).

### Спальня-кабинет

| Зона | WB | HA entity |
|------|-----|-----------|
| Спальня — шторы | wb-led_48 CCT2 | `light.spalnia_shtory` |
| Спальня — люстра | wb-mdm3_109 K2 | `light.spalnia_liustra` |
| Прикроватный у стены | wb-mdm3_209 K1 | `light.prikrovatnyi_stena` |
| Прикроватный у окна | wb-mdm3_209 K2 | `light.prikrovatnyi_okno` |
| Кабинет — верхний свет | wb-mdm3_209 K3 | `light.kabinet_potolok` |
| Кабинет — подсветка стола | wb-led_47 ch 3 | `light.kabinet_stol` |

### Детская

| Зона | WB | HA entity |
|------|-----|-----------|
| Шторы | wb-led_48 CCT1 | `light.detskaia_shtory` |
| Люстра | wb-mdm3_201 K2 | `light.detskaia_liustra` |
| Рабочая зона | wb-mdm3_109 K1 | `light.detskaia_rabochaia_zona` |
| Подсветка стены | wb-led_47 ch 2 | `light.detskaia_stena` |

### Ванная

| Зона | WB | HA entity |
|------|-----|-----------|
| Потолок | wb-mdm3_175 K1 | `light.vanna_potolok` |
| Подсветка | wb-led_16 ch 4 | `light.vannaya_podsvetka` |

### Прачечная

| Зона | WB | HA entity |
|------|-----|-----------|
| Потолок | wb-mdm3_118 K3 | `light.postirochnaia_potolok` |

### Коридор

| Зона | WB | HA entity |
|------|-----|-----------|
| Подсветка | wb-led_36 CCT1 | `light.koridor_podsvetka` |
| Потолочные | wb-mdm3_110 K3 | `light.koridor_potolok` |

## Бризеры (ESPHome / Tuya) — панель «Климат»

Конфиги ESP: `very-esphome/*.yml`. Авторизация API — ключ шифрования (noise_psk),
пароль удалён (ESPHome 2026.1.0). Устройства добавляются в HA через UI:
Настройки → Устройства → Добавить интеграцию → ESPHome (host + ключ шифрования).

| Помещение | ESP device | Статус в HA | Префикс entity_id |
|-----------|-----------|-------------|-------------------|
| Кухня-гостиная | `livingroom_brizer` (192.168.180.12) | добавлен, auth=ключ | `*.livingroom_brizer_*` |
| Спальня-кабинет | `Bedroom_brizer` | добавить через UI | `*.bedroom_brizer_*` |
| Детская | `Childrenroom_brizer` | добавить через UI | `*.childrenroom_brizer_*` |
