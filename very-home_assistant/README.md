# very-home_assistant

Home Assistant для квартиры Very · интеграция с Wiren Board.

## Сеть

| Устройство | IP |
|------------|-----|
| Wiren Board 8 (MQTT) | `192.168.180.10` |
| Raspberry Pi 5 (HA) | `192.168.180.11` |
| ESP Brizer гостиная | `192.168.180.12` |

Для отладки с VPN WB доступен на `10.8.0.19` — в конфигурации HA всегда используется LAN-адрес брокера.

## Свет (Wiren Board)

Соответствие каналов WB ↔ комнаты: [`svet.md`](../svet.md) в корне репозитория.

### Файлы

| Файл | Назначение |
|------|------------|
| `homeassistant/packages/wirenboard.yaml` | MQTT-брокер и 22 сущности `light.*` |
| `homeassistant/dashboards/svet.yaml` | Панель Lovelace «Свет» |
| `homeassistant/themes/wb_svet.yaml` | Тёмная тема для панели |
| `homeassistant/scripts.yaml` | Скрипт «Выключить всё» |

### После обновления конфигурации

1. Перезапустить Home Assistant (Настройки → Система → Перезапуск).
2. В боковом меню появится панель **Свет** (тема `wb_svet`).
3. При первом запуске MQTT-сущностей назначьте комнаты (Настройки → Пространства), если `suggested_area` не подхватилась автоматически.

### Запуск в контейнере

```bash
cd very-home_assistant
docker compose up -d
```
