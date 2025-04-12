# 💾 Автоматическое резервное копирование Marzban + Telegram-бот

## 🛡 Назначение
Скрипт + бот создают автоматические и ручные резервные копии клиентов **Marzban** с отправкой уведомлений в Telegram.

## ⚙️ Что делает

- 🕛 Автоматически создаёт бэкап каждый день в **00:00 (МСК)**
- 💬 При запросе в боте вручную делает бэкап и присылает результат
- 💣 При ошибке отправляет **лог-файл** в Telegram
- 📦 При успехе отправляет **ZIP-архив**
- ♻️ Хранит **только 3 последних бэкапа**
- 👤 Работает в Docker-окружении **без изменения оригинального образа**
- 🔐 Архивы **не шифруются** — можно сразу открыть
- 📁 Архивы хранятся в `/opt/marzban_backups`

## 📂 Структура проекта

/opt/marzban_backup_system/ └── backup.sh # Основной скрипт
/opt/marzban_backups/ # Папка для хранения бэкапов
/opt/marzban_telegram_bot/ ├── bot.py # Telegram-бот ├── config.ini # Настройки токена и ID ├── requirements.txt # Зависимости └── venv/ # Виртуальное окружение


## 🛠 Установка

### 1. Копирование проекта:
mkdir -p /opt/marzban_backup_system /opt/marzban_backups /opt/marzban_telegram_bot

2. Закиньте файлы из архива в соответствующие папки
3. Установка зависимостей:

cd /opt/marzban_telegram_bot
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

4. Укажите токен и ID Telegram в config.ini

🔧 Systemd-сервис
Создайте файл:
sudo nano /etc/systemd/system/marzban-bot.service

Пример содержимого:
[Unit]
Description=Marzban Telegram Bot
After=network.target

[Service]
WorkingDirectory=/opt/marzban_telegram_bot
ExecStart=/opt/marzban_telegram_bot/venv/bin/python3 /opt/marzban_telegram_bot/bot.py
Restart=always

[Install]
WantedBy=multi-user.target

Активируй бот:
sudo systemctl daemon-reload
sudo systemctl enable marzban-bot
sudo systemctl start marzban-bot

