 1. Обновление системы (первым делом!)
```bash
sudo apt update && sudo apt upgrade -y
2. Guest Additions (VirtualBox)
bash
sudo apt install -y virtualbox-guest-utils virtualbox-guest-x11 virtualbox-guest-dkms
sudo reboot
3. SSH-сервер
bash
sudo apt install -y openssh-server
sudo systemctl enable ssh && sudo systemctl start ssh
sudo systemctl status ssh  # должно быть active (running)
hostname -I  # узнать IP
4. Проверка интернета
bash
ip addr show
ping -c 4 google.com
curl -I https://google.com
5. Базовое ПО
bash
sudo apt install -y libreoffice p7zip-full p7zip-rar htop chromium-browser
6. Виртуальный принтер (CUPS + PDF)
bash
sudo apt install -y cups cups-pdf
sudo systemctl enable cups && sudo systemctl start cups
# Настройки → Принтеры → появится "PDF"
7. Установка ПО через snap (быстро)
bash
sudo snap install code --classic          # VS Code
sudo snap install pycharm-community --classic
sudo snap install intellij-idea-community --classic
sudo snap install eclipse --classic
8. Среда выполнения (Python, Java, Node.js)
bash
sudo apt install -y python3 python3-pip python3-venv
sudo apt install -y default-jdk
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash - && sudo apt install -y nodejs
БЛОК 2 — Защита и бэкап (20–35 мин)
1. Резервное копирование (Timeshift)
bash
sudo apt install timeshift -y
sudo timeshift --create --comment "Точка восстановления Linux"
sudo timeshift --list
2. Установочный образ (Export в VirtualBox)
text
VirtualBox → Файл → Экспортировать конфигурацию → выбрать ВМ → Формат OVA
3. Точки восстановления (снимки ВМ)
text
VirtualBox → Снимки → Создать снимок → "До установки ПО" и "После установки ПО"
4. Группы пользователей и права
bash
sudo groupadd developers
sudo groupadd analysts

sudo useradd -m -G developers devuser
sudo passwd devuser

sudo useradd -m -G analysts analystuser
sudo passwd analystuser

sudo mkdir /opt/dev_workspace /opt/analytics_data
sudo chown :developers /opt/dev_workspace && sudo chmod 770 /opt/dev_workspace
sudo chown :analysts /opt/analytics_data && sudo chmod 770 /opt/analytics_data

cat /etc/group | grep -E 'developers|analysts'
5. Журнал мониторинга
bash
sudo systemctl status rsyslog
sudo apt install -y logwatch
sudo logwatch --output stdout --format text --range today
tail -f /var/log/syslog
6. Файрвол (UFW)
bash
sudo apt install -y ufw
sudo ufw allow ssh && sudo ufw allow 80 && sudo ufw enable
sudo ufw status
БЛОК 3 — Профильное ПО (35–60 мин)
1. Docker
bash
sudo apt install -y docker.io
sudo systemctl enable docker && sudo systemctl start docker
sudo docker --version
2. Git
bash
sudo apt install -y git
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
3. MySQL / PostgreSQL
bash
sudo apt install -y mysql-server
sudo mysql_secure_installation
sudo systemctl enable mysql

sudo apt install -y postgresql postgresql-contrib
sudo systemctl enable postgresql
4. Антивирус ClamAV
bash
sudo apt install -y clamav clamtk
sudo freshclam
clamscan --version
5. Настройка совместимости (5 пунктов по 0.5 балла)
bash
# 1. Высококонтрастный режим (Настройки → Доступность → Высококонтрастный)

# 2. Низкое разрешение — в настройках ВМ изменить на 800×600

# 3. Отключить композицию
gsettings set org.gnome.mutter compositing-manager false

# 4. Отключить масштабирование
gsettings set org.gnome.desktop.interface scaling-factor 1
xrandr --dpi 96

# 5. Проверить отображение меню и кнопок
6. Настройка обмена данными (+2.5 балла)
bash
# IDE + Git
mkdir ~/project && cd ~/project
git init

# IDE + БД — в VS Code установить расширение "Database Client"

# Проверка Python
python3 -c "print('Hello World')"
БЛОК 4 — Документация (60–75 мин)
📄 Документация пользователя (3.5 балла) — кратко, 1-2 стр.
text
Наименование: [Название ПО], версия X.X

Краткое описание:
[Название] — это [тип ПО], предназначенный для [основная функция].
Разработан [компания]. Распространяется бесплатно / по лицензии.

Запуск программы:
1. Меню приложений → [категория] → [название]
2. Терминал: [команда]
3. Ярлык на рабочем столе

Запланированный выход:
1. Меню → Файл → Выход
2. Ctrl+Q или Alt+F4
3. Закрыть окно (✕)
📘 Руководство пользователя по ГОСТ (5 баллов) — структура
text
ТИТУЛЬНЫЙ ЛИСТ:
[Организация]
РУКОВОДСТВО ПОЛЬЗОВАТЕЛЯ
[Название ПО] версия X.X
Составил: [ФИО]
Дата: [дата]

АННОТАЦИЯ:
Настоящий документ является руководством пользователя для [название ПО]
версии [X]. Разработан в соответствии с ГОСТ Р 59795-2021.

1. ОБЩИЕ СВЕДЕНИЯ
Полное название: [название]
Версия: X.X
Назначение: [1-2 предложения]
Разработчик: [компания]
Лицензия: [тип]
Сайт: [url]

2. УСЛОВИЯ ПРИМЕНЕНИЯ
| Компонент | Минимальные требования |
|-----------|------------------------|
| ОС        | Linux Ubuntu 20.04+    |
| Процессор | 2 ГГц                  |
| ОЗУ       | 4 ГБ                   |
| Диск      | 10 ГБ свободного места |
| Доп. ПО   | [список]               |

3. ВЫПОЛНЕНИЕ ПРОГРАММЫ
3.1 Запуск программы
[описание как запустить]

3.2 Описание операций
| Операция | Действия | Результат |
|----------|----------|-----------|
| Создание | Меню → Создать | Открыт новый проект |
| Открытие | Меню → Открыть | Файл загружен |
| Сохранение | Ctrl+S | Файл сохранён |

3.3 Завершение работы
Меню → Выход

4. СООБЩЕНИЯ ОПЕРАТОРУ
| Сообщение | Причина | Действие |
|-----------|---------|----------|
| Ошибка X  | Причина Y| Сделать Z |
БЛОК 5 — Демонстрация (75–80 мин)
Порядок показа преподавателю:
Рабочий стол → установленное ПО

ip addr show + ping google.com — интернет есть

sudo systemctl status ssh — SSH работает

VNC Viewer — удалённый доступ

Снимки ВМ в VirtualBox

sudo timeshift --list — бэкап

cat /etc/group | grep -E 'developers|analysts' — группы

Запуск профильного ПО → тестовый проект

Открыть руководство пользователя в LibreOffice

Устно: «Выбор ПО обоснован: [название] — это [open-source/стандарт], совместим с Linux, подходит для [задача]»

Шпаргалка по обоснованию ПО (устно)
ПО	Шаблон обоснования
VS Code	Бесплатный, мощный редактор с поддержкой всех языков (Bash, Python, Go, Rust, YAML, JSON)

Docker	Стандарт контейнеризации, совместим с Linux, упрощает развёртывание

Jenkins/GitLab CI	Популярная CI/CD платформа, автоматизирует сборку и тестирование

Prometheus + Grafana	Система мониторинга с открытым кодом, активно используется в DevOps
ClamAV	Бесплатный антивирус для Linux, обеспечивает защиту периметра

MySQL/PostgreSQL	Стандартные СУБД, совместимы с Linux, подходят для хранения данных

LibreOffice	Бесплатный офисный пакет, совместим с Linux
