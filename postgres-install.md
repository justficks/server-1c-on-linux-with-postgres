[< Назад на главную](README.md)

# Установка PostgresSql для 1с

Скачиваем установочный файл и запускаем установку:

```
curl -o pgpro-repo-add.sh https://repo.postgrespro.ru/pg1c-14/keys/pgpro-repo-add.sh
sudo sh pgpro-repo-add.sh
apt-get install postgrespro-1c-14
```

Чтобы найти имя демона PostgreSQL выполним команду

```
systemctl --type=service | grep postgres

# Пример вывода:
#     postgrespro-1c-14.service
```

Теперь останавливаем сервис и удаляем созданный по умолчанию кластер

```
# Останавливаем PostgreSQL
sudo systemctl stop postgrespro-1c-14

# Удаляем файлы ранее созданного при установке кластера
# Вместо 1c-14 может быть другое название каталога, в зависимости от версии.
rm -r /var/lib/pgpro/1c-14/data/*

# Инициализируем новый кластер для 1С с нужной локалью (не обязательно, если по умолчанию локаль в системе "ru_RU.UTF-8").
sudo /opt/pgpro/1c-14/bin/pg-setup initdb --tune=1c --locale=ru_RU.UTF-8

# Запускаем PostgreSQL
sudo systemctl start postgrespro-1c-14
```

Готово. Дополнительно, но только в качестве примера, сделаем дополнительные шаги.

- Разрешим подключение к СУБД с любых адресов. Для этого в файле конфигурации сервера (/var/lib/pgpro/1c-14/data/postgresql.conf) изменим строчку:

```
# listen_addresses = 'localhost'
listen_addresses = '*'
```

- Также разрешим подключение для всех пользователей по логину и паролю. В файле (/var/lib/pgpro/1c-14/data/pg_hba.conf) изменим разрешения для IPv4:

```
# Было
# # IPv4 local connections:
# host    all             all             127.0.0.1/32            md5

# Стало
# IPv4 local connections:
host    all             all             0.0.0.0/0               password
host    all             all             127.0.0.1/32            md5
```

Подключение к базе выглядит следующим образом и тут же меняем пароль учетной записи "postgres", созданной по умолчанию::

```
sudo su postgres

psql

postgres=# \password # Вводим пароль
```

Затем выходим из консоли управления psql:

```
postgres=# \q
```

И перезагружаем сервер для применения настроек первого конфигурационного файла:

```
systemctl restart postgrespro-1c-14
```
