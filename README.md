# 💾 Автоматическое резервное копирование Marzban + Telegram-бот

## 🛡 Назначение
Скрипт + бот создают автоматические и ручные резервные копии клиентов Marzban с отправкой уведомлений в Telegram.

## ⚙️ Что делает
- 🕛 Автоматически создаёт бэкап каждый день в 00:00 (по МСК)
- 💬 При запросе в боте вручную делает бэкап и присылает результат
- 💣 При ошибке отправляет лог-файл в Telegram
- 📦 При успехе отправляет zip-архив
- ♻️ Хранит только 3 последних бэкапа
- 👤 Работает в Docker-окружении без изменений образа Marzban
- 🔐 Не шифрует архив (чтобы можно было сразу открыть)
- 📁 Архивы хранятся в `/opt/marzban_backups`

## 📂 Структура проекта
```bash
/opt/marzban_backup_system/
└── backup.sh               # основной скрипт

/opt/marzban_backups/       # папка для хранения бэкапов

/opt/marzban_telegram_bot/
├── bot.py                  # Telegram-бот
├── config.ini              # настройки токена и пути
├── requirements.txt        # зависимости
└── venv/                   # виртуальное окружение
```

## 🛠 Настройка

### 1. Клонирование проекта
```bash
mkdir -p /opt/marzban_backup_system /opt/marzban_backups /opt/marzban_telegram_bot
```

### 2. Закинь туда файлы из архива.

### 3. Установка зависимостей
```bash
cd /opt/marzban_telegram_bot
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

### 4. Настройка конфигурации
Добавь Telegram-токен и ID в файл `config.ini`.

### 5. Настройка systemd-сервиса
```bash
sudo nano /etc/systemd/system/marzban-bot.service
```

Пример содержимого:
```ini
[Unit]
Description=Marzban Telegram Bot
After=network.target

[Service]
WorkingDirectory=/opt/marzban_telegram_bot
ExecStart=/opt/marzban_telegram_bot/venv/bin/python3 /opt/marzban_telegram_bot/bot.py
Restart=always

[Install]
WantedBy=multi-user.target
```

### 6. Запуск бота
```bash
sudo systemctl daemon-reload
sudo systemctl enable marzban-bot
sudo systemctl start marzban-bot
```
