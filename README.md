# Лабораторная работа #6 – Установка корпоративной вики и системы управления задачами

## Введение
Цель данной лабораторной работы — изучить процесс установки корпоративной вики и системы управления задачами на локальный сервер , оценить их основные возможности, особенности настройки и, опционально, попробовать их интеграцию. Данное задание направлено на практическое освоение инструментов управления знаниями и проектами, которые используются в корпоративной среде.

**Выбранные системы:**
- Корпоративная вики: [MediaWiki](https://www.mediawiki.org/wiki/Manual:Installation_guide)
- Система управления задачами: [Redmine](https://www.redmine.org/projects/redmine/wiki/RedmineInstall)

## Процесс установки

### Установка MediaWiki
1. **Загрузка MediaWiki**  
   Скачайте последнюю версию MediaWiki с [официального сайта](https://www.mediawiki.org/wiki/Download).
   
2. **Настройка серверного окружения**  
   Убедитесь, что у вас установлен веб-сервер (например, Apache) и база данных (например, MySQL). Для этого выполните следующие команды:

   ```bash
   sudo apt update
   sudo apt install apache2 mysql-server php php-mysql libapache2-mod-php
   sudo systemctl enable apache2
   sudo systemctl enable mysql
   ```
3. **Установка MediaWiki**
После скачивания распакуйте архив и переместите файлы в директорию веб-сервера:

```bash
sudo tar -xvzf mediawiki-x.y.z.tar.gz -C /var/www/html/
```
4. **Настройка базы данных**
Создайте базу данных и пользователя:

```bash
sudo mysql -u root -p
CREATE DATABASE mediawiki;
CREATE USER 'wikiuser'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON mediawiki.* TO 'wikiuser'@'localhost';
FLUSH PRIVILEGES;
```
5. **Конфигурация MediaWiki**
Перейдите в браузере по адресу http://localhost/mediawiki и следуйте инструкциям для завершения установки.

6. **Настройки конфигурации**
Внесите необходимые изменения в LocalSettings.php, такие как настройки базы данных:

```php
$wgDBserver = "localhost";
$wgDBname = "mediawiki";
$wgDBuser = "wikiuser";
$wgDBpassword = "password";
```
### Установка Redmine
1. **Загрузка Redmine**
Скачайте и установите Redmine с официального сайта.

2. **Установка зависимостей**
Убедитесь, что у вас установлены все необходимые компоненты для работы Redmine:

```bash
sudo apt install ruby rails sqlite3 libmysqlclient-dev libmagickwand-dev
```
3. **Настройка базы данных**
Создайте базу данных для Redmine:

```bash
sudo mysql -u root -p
CREATE DATABASE redmine CHARACTER SET utf8mb4;
CREATE USER 'redmine'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON redmine.* TO 'redmine'@'localhost';
FLUSH PRIVILEGES;
```
4. **Конфигурация Redmine**
Отредактируйте конфигурацию базы данных в файле config/database.yml:

```yaml
production:
  adapter: mysql2
  database: redmine
  host: localhost
  username: redmine
  password: "password"
  encoding: utf8mb4
```
5. **Запуск Redmine**
Перейдите в каталог с Redmine и выполните миграцию базы данных:

```bash
bundle install --without development test
rake generate_secret_token
RAILS_ENV=production rake db:migrate
```
6. **Запуск серверов**
Запустите сервер Redmine:

```bash
sudo service apache2 restart
```
## Функциональный анализ
### MediaWiki
Основные функции: создание и форматирование страниц, работа с категориями и шаблонами, управление доступом.
Управление доступом: поддерживаются роли администратора, редактора, читателя. Права доступа можно настроить для каждой страницы.
### Redmine
Основные функции: создание задач, назначение исполнителей, использование Kanban-доски и диаграмм Gantt.
Управление доступом: можно настроить права для разных ролей (например, администратор, менеджер, исполнитель).

## Заключение
MediaWiki: Отличный инструмент для организации знаний и документации, обладает гибкостью в настройках и поддерживает множество расширений.
Redmine: Мощная система управления задачами, поддерживает гибкую настройку и интеграцию с другими инструментами.
Рекомендации: Для комплексных проектов рекомендуется использовать обе системы в сочетании для эффективного управления задачами и знаниями.
## Ссылки на использованные источники
[MediaWiki Installation Guide](https://www.mediawiki.org/wiki/Manual:Installing_MediaWiki)

[Redmine Installation Guide](https://www.redmine.org/projects/redmine/wiki/RedmineInstall)
