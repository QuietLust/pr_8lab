# Руководство по сборке и использованию myRPC

## Описание

Проект `myRPC` представляет собой механизм удалённого вызова команд, состоящий из клиентского и серверного приложений. Включает следующие компоненты:
- **myRPC-client** — консольное приложение для отправки команд на сервер.
- **myRPC-server** — серверное приложение, принимающее команды и выполняющее их.

Проект разработан для взаимодействия с удалённым сервером с использованием сокетов, поддерживая как потоковые (TCP), так и датаграммные (UDP) соединения.

## Системные требования

- Компилятор GCC
- Утилита `make`
- Поддержка разработки deb-пакетов (`dpkg-deb`)

## Сборка проекта

Для сборки `myRPC-client` и `myRPC-server` используйте Makefile, который автоматизирует процесс компиляции и создания deb-пакетов.

### Сборка myRPC-client

1. Перейдите в директорию с исходным кодом `myRPC-client`.
2. Выполните команду:
   ```sh
   make
   ```
   Это создаст исполняемый файл `myRPC-client` в текущей директории.

3. Для создания deb-пакета выполните команду:
   ```sh
   make deb
   ```
   Это создаст deb-пакет в директории `deb/myrpc-client/`.

4. Чтобы очистить директорию от скомпилированных файлов, используйте команду:
   ```sh
   make clean
   ```

### Сборка myRPC-server

1. Перейдите в директорию с исходным кодом `myRPC-server`.
2. Выполните команду:
```sh
make
```
   Это создаст исполняемый файл `myRPC-server` в текущей директории.

3. Для создания deb-пакета выполните команду:
```sh
make deb
```
   Это создаст deb-пакет в директории `deb/myrpc-server/`.

4. Чтобы очистить директорию от скомпилированных файлов, используйте команду:
```sh
make clean
```

## Установка и настройка

После сборки и создания deb-пакетов можно установить клиент и сервер с помощью команды `dpkg`:

```sh
sudo dpkg -i deb/myrpc-client_1.0-1_amd64.deb
sudo dpkg -i deb/myrpc-server_1.0-1_amd64.deb
sudo dpkg -i deb/libmysyslog_1.0-1_amd64.deb
```

### Настройка сервера

1. Создайте конфигурационный файл `/etc/myRPC/myRPC.conf` для настройки параметров сервера. Пример файла:
```conf
sudo mkdir -p /etc/myRPC
echo -e "port=5465\nsocket_type=stream" | sudo tee /etc/myRPC/myRPC.conf
```

2. Создайте файл пользователей `/etc/myRPC/users.conf`, в котором указаны разрешённые пользователи:
```conf
echo "sa" | sudo tee /etc/myRPC/users.conf
```

## Использование

### myRPC-client

Клиентское приложение `myRPC-client` позволяет отправлять команды на сервер для выполнения. Примеры использования:

1. Отправка команды с использованием потокового сокета (TCP):
```sh
# TCP-соединение
myrpc-client -h 127.0.0.1 -p 5465 -s -c "date"

# UDP-соединение
myrpc-client -h 127.0.0.1 -p 5465 -d -c "date"
```

### myrpc-server

Серверное приложение `myrpc-server` запускается на сервере и ожидает подключения от клиентов. Для запуска сервера выполните команду:

```sh
sudo myrpc-server
```

## Логирование и отладка

Сервер `myrpc-server` использует библиотеку `libmysyslog` для ведения журнала событий. Все события записываются в файл `/var/log/myrpc.log`. Логи содержат информацию о подключениях клиентов, выполнении команд и возможных ошибках.




