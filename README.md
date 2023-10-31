
Для настройки и запуска бота хорошо бы иметь опыт в Python и Linux.
Рекомендации:
- Ubuntu 18.04+ или любой другой
- Linuxmysql 5.7
- python 3.6-3.8
Установка и настройка
устанавливаем необходимые пакеты:
apt update && apt install mysql-server python3-pip
Желательно поставить mysql-server5.7, но все будет работать и на восьмой версии. 
Далее ставим необходимые модули. Все модули описаны в файле requirements.txt:
- Flask==2.1.0
- PyMySQL==0.9.3
- PyYAML==3.12
- requests==2.18.4
- python-dotenv==0.21.0
pip3 install -r requirements.txt
Теперь необходимо сгенерировать сертификаты и залить схему с демо-данными в базу данных.
openssl req -x509 -newkey rsa:4096 -keyout cert.key -out cert.crt -days 365
Ключ и сертификат надо перенести в папку adult/certs
Теперь перейдем в mysql и создадим базу данных:
create database adult;
Далее необходимо залить схему из дампа, который лежит в папке с исходниками:
mysql -D adult < mysql.sql
Далее снова зайдем в mysql и создадим пользователя, дадим ему прав на базу:
CREATE USER 'adult'@'localhost' IDENTIFIED WITH 'mysql_native_password' BY 'adult';
grant all privileges on *.* to ‘adult'@'localhost';
Бота необходимо настроить через переменные окружения. Все переменные описаны в файле adult/.env 
Нет универсального способа подгружать переменные в окружение / систему, т.к все зависит от способа запуска бота.
MYSQL_ADDRESS - адресс, по которому доступна MySQL
MYSQL_USER - пользователь с правами до БД
MYSQL_PASSWORD - пароль от пользователя
MYSQL_DATABASE - название базы данных
MYSQL_ROOT_PASSWORD - пароль рута БД
BOT_ADDRESS - адрес бота
BOT_PORT - порт бота
BOT_TOKEN - токен бота
BOT_TOKEN_DEV - токен бота разработчика
BOT_DEV_CHAT_ID - чат_ИД разработчика
CRT_FILE_PATH - путь до файла с сертификатом
CRT_FILE_KEY_PATH - путь до файла с ключом от сертификата
LOG_FILE_PATH - путь до файла логирования
ADMINS_FILE_PATH - путь до файла, в котором находятся чат_ИД администраторов
Теперь все готово к запуску бота, для этого переходим в папку app (в которой лежит файл Dockerfile) и запускаем:
python3 -m adult
Если кто-то хочет все завернуть в docker, то в файлах с проектом лежит Dockerfile для сборки образа и docker-compose.yml манифест для запуска базы и приложеньки. Если запуск планируется с помощью docker, то прийдется немного переписать часть с webhook
