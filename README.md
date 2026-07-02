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

 1. Документация пользователя (краткая) — 3.5 балла
Этот документ пишется на 1–2 страницы. Шаблон:

text
Наименование программы: [Название ПО], версия [X.X]

Краткое описание:
[Название] — это [тип ПО], предназначенный для [основная функция].
Разработан [компания/сообщество]. Распространяется бесплатно / по лицензии [тип].

Запуск программы:
1. Меню приложений → [категория] → [название]
2. Терминал: [команда]
3. Ярлык на рабочем столе

Запланированный выход:
1. Меню → Файл → Выход
2. Ctrl+Q / Alt+F4
3. Закрыть окно (✕)
📘 2. Полное руководство по ГОСТ 19.505-79 — 5 баллов
Титульный лист
text
[Полное название учебного заведения]
[Название кафедры/ПЦК]

РУКОВОДСТВО ОПЕРАТОРА

[Полное название программы] версия [X.X]

Составил: [Твои ФИО]
[Дата]
Аннотация
«Настоящий документ является руководством оператора для программы «[Название ПО]» версии [X.X]. Руководство предназначено для пользователей, эксплуатирующих программу, и содержит сведения о её назначении, условиях выполнения, необходимых действиях оператора, а также описание сообщений системы. Документ разработан в соответствии с ГОСТ 19.505-79 ЕСПД.» 

Раздел 1. Назначение программы
text
1.1 Назначение: Программа «[Название]» предназначена для [основная цель].

1.2 Основные функции:
- [Функция 1]
- [Функция 2]
- [Функция 3]
Раздел 2. Условия выполнения программы
text
2.1 Минимальный состав технических средств:
- Процессор: 2 ГГц
- ОЗУ: 4 ГБ (рекомендуется 8 ГБ)
- Свободное место на диске: 10 ГБ

2.2 Минимальный состав программных средств:
- ОС: Linux Ubuntu 20.04 / 22.04 LTS
- Доп. ПО: [список]

2.3 Требования к персоналу (оператору):
Пользователь должен обладать базовыми навыками работы с ОС Linux и командной строкой [citation:1].
Раздел 3. Выполнение программы
text
3.1 Запуск программы:
Терминал: [команда запуска]

3.2 Описание операций (таблица):
| Операция | Действия | Результат |
|----------|----------|-----------|
| Создание | [действие] | [результат] |
| Настройка | [действие] | [результат] |
| Запуск | [действие] | [результат] |

3.3 Завершение работы:
- Интерфейс: кнопка «Выход»
- Терминал: Ctrl+C
Раздел 4. Сообщения оператору
text
| Текст сообщения | Причина | Действия |
|-----------------|---------|----------|
| Permission denied | Недостаточно прав | Выполнить с sudo |
| Port already in use | Порт занят | Сменить порт |
🐳 Готовые описания для ВСЕХ возможных приложений
CI/CD Платформы
1. Jenkins 

Элемент	Содержание
Назначение	Автоматизация сборки, тестирования и развертывания приложений 
Запуск	sudo systemctl start jenkins
Доступ	http://[IP]:8080 
Начальный пароль	sudo cat /var/lib/jenkins/secrets/initialAdminPassword 
Основные операции	Создание Item → Настройка Git репозитория → Запуск сборки → Просмотр логов 
2. GitLab Runner 

Элемент	Содержание
Назначение	CI/CD исполнитель для GitLab 
Установка	sudo apt install gitlab-runner
Регистрация	sudo gitlab-runner register
3. Arbess 

Элемент	Содержание
Назначение	CI/CD инструмент (аналог Jenkins) 
Установка	dpkg -i tiklab-arbess-x.x.x.deb 
Запуск	cd /opt/tiklab-arbess/bin && ./arbess start 
Доступ	http://[IP]:9200, логин admin/123456 
Контейнеризация и оркестрация
4. Docker 

Элемент	Содержание
Назначение	Контейнеризация приложений 
Установка	sudo apt install -y docker.io 
Запуск контейнера	docker run -d --name [имя] [образ]
Просмотр	docker ps, docker images
Остановка	docker stop [имя]
Docker Compose	Управление многоконтейнерными приложениями 
5. Kubernetes (kubectl) 

Элемент	Содержание
Назначение	Управление кластерами контейнеров 
Проверка	kubectl cluster-info
Основные операции	kubectl get pods, kubectl apply -f [file].yaml
6. minikube 

Элемент	Содержание
Назначение	Локальный Kubernetes кластер 
Запуск	minikube start
7. Helm 

Элемент	Содержание
Назначение	Пакетный менеджер для Kubernetes 
Проверка	helm version 
Мониторинг
8. Prometheus 

Элемент	Содержание
Назначение	Сбор метрик и мониторинг 
Запуск	prometheus --config.file=/etc/prometheus/prometheus.yml 
Доступ	http://localhost:9090 
9. Grafana 

Элемент	Содержание
Назначение	Визуализация данных мониторинга 
Запуск	sudo systemctl start grafana-server 
Доступ	http://[IP]:3000, логин admin/admin 
Инфраструктура как код (IaC)
10. Terraform 

Элемент	Содержание
Назначение	Управление инфраструктурой через код 
Проверка	terraform --version 
Основные команды	terraform init, terraform plan, terraform apply
11. Ansible 

Элемент	Содержание
Назначение	Автоматизация конфигурации 
Проверка	ansible --version 
Запуск плейбука	ansible-playbook -i inventory playbook.yml 
Управление секретами и безопасность
12. HashiCorp Vault 

Элемент	Содержание
Назначение	Управление секретами 
Запуск	vault server -dev
13. UFW (файрвол)

Элемент	Содержание
Назначение	Защита периметра
Команды	sudo ufw allow ssh, sudo ufw enable, sudo ufw status
Инструменты разработки
14. VS Code / VS Codium 

Элемент	Содержание
Назначение	Редактор кода с поддержкой всех языков 
Установка	sudo snap install code --classic
Запуск	code .
15. Git 

Элемент	Содержание
Назначение	Система контроля версий 
Настройка	git config --global user.name "Name" 
Основные команды	git init, git add, git commit, git push
16. Java (OpenJDK) 

Элемент	Содержание
Назначение	Среда выполнения для Java приложений 
Установка	sudo apt install openjdk-17-jdk 
Проверка	java -version 
Облачные CLI
17. AWS CLI 

Элемент	Содержание
Назначение	Управление AWS ресурсами 
Настройка	aws configure 
Проверка	aws --version 
18. Azure CLI, Google Cloud SDK 
Отключение	sudo ufw disable
