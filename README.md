# MMRC MPV Player

Нативный медиаплеер для Linux на базе MPV. Подключается к MMRC серверу через Socket.IO и воспроизводит медиаконтент с аппаратным ускорением.

## Возможности

- Видео (MP4, WebM, MKV, MOV, AVI)
- Аудио (MP3, AAC, WAV, FLAC, OGG, M4A)
- Изображения (PNG, JPG, JPEG, GIF, WebP)
- Стриминг (HLS, DASH)
- Папки (ZIP-архивы изображений)
- Аппаратное ускорение (VAAPI, VDPAU, MMAL)
- Автопереподключение
- Определение типа устройства (Raspberry Pi, x86)
- Оптимальные параметры для каждого устройства

## Требования

- Python 3.10+
- MPV 0.32+
- Подключение к интернету
- Доступ к MMRC серверу

## Установка

### Автоматическая (Raspberry Pi / Ubuntu)

```bash
curl -fsSL https://raw.githubusercontent.com/ya-k0v/MMRC-mpv-player/main/install.sh | bash
```

### Ручная

```bash
# Установить зависимости
pip install -r requirements.txt

# Установить MPV
sudo apt install mpv

# Запустить
python mpv_client.py --server http://192.168.1.100:3000 --device-id LIN001
```

## Конфигурация

### Аргументы командной строки

```bash
python mpv_client.py \
  --server http://192.168.1.100:3000 \
  --device-id LIN001 \
  --name "Living Room TV"
```

### Параметры MPV

Файл конфигурации MPV: `~/.config/mpv/mpv.conf`

Пример для Raspberry Pi:
```
hwdec=mmal
vo=gpu
gpu_context=drm
```

## Systemd сервис

```bash
# Установить сервис
sudo cp videocontrol-mpv@.service /etc/systemd/system/
sudo systemctl enable videocontrol-mpv@<device-id>
sudo systemctl start videocontrol-mpv@<device-id>
```

## Определение устройства

Плеер автоматически определяет тип устройства и применяет оптимальные параметры:

| Устройство | HWDec | VO | Особенности |
|------------|-------|----|----|
| Raspberry Pi | mmal | drm | Оптимизировано для ARM |
| x86 Linux | vaapi | gpu | VAAPI аппаратное ускорение |
| Wayland | auto | gpu | Wayland поддержка |

## Структура проекта

```
├── mpv_client.py              # Основной клиент
├── requirements.txt           # Python зависимости
├── install.sh                 # Скрипт установки
├── quick-install.sh           # Быстрая установка
├── mpv.conf.raspberry-pi      # Конфигурация MPV для RPi
└── videocontrol-mpv@.service  # Systemd сервис
```

## Зависимости

- python-socketio — подключение к серверу
- python-engineio — транспортный слой
- requests — HTTP запросы
- mpv — воспроизведение медиа
