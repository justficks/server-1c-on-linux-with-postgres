[< Назад на главную](README.md)

## Устанавлиаем соединени 1с сервера (кластера) с PostgreSQL

1. Запускаем ./ras (кластер серверов):

```
./ras --daemon cluster
```

2. Получаем информацию о запущенном кластере. Если команда ниже ничего не вывела, то я сделал просто и полностью удалил 1с. [Вот инструкция](1c-uninstall.md)

```
./rac cluster list
```

2. Создаем администратора кластера

```
./rac cluster admin --cluster=CLUSTER_UID_BELOW register --name=admin --pwd=YOUR_PASSWORD --auth=pwd
```

3. Создаем информационную базу 1c в PostgreSQL:

```
./rac infobase create --cluster=CLUSTER_UID --cluster-user=admin --cluster-pwd=SECRET_PASSWORD_CLUSTER --create-database --name=demo --descr=MyTestBaseOnLinuxPower --dbms=PostgreSQL --db-server=localhost --db-name=demo --locale=ru --db-user=postgres --db-pwd=SECRET_PASSWORD_PGSQL --license-distribution=allow
```

4. Проверка:

```
./rac infobase --cluster=CLUSTER_UID --cluster-user=admin --cluster-pwd=SECRET_PASSWORD_CLUSTER summary list
```
