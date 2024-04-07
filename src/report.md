# Simple Docker

## Part 1. Готовый докер

* Вначале выполнил команды: ```sudo apt install docker``` и ```sudo apt install docker.io```
* Взял официальный докер-образ с nginx и выкачал его при помощи команды ```sudo docker pull```.

    ![OS](screnshots/Part-1.1.png)

* Проверил наличие докер-образа через ```docker images```.

    ![OS](screnshots/Part-1.2.png)

* Запустил докер-образ через ```docker run -d [image_id|repository]```.

    ![OS](screnshots/Part-1.3.png)

* Проверил, что образ запустился через ```docker ps```.

    ![OS](screnshots/Part-1.4.png)

* Посмотрел информацию о контейнере через ```docker inspect [container_id|container_name]```.

* По выводу команды определил и поместил в отчёт 
    размер контейнера:

    ![OS](screnshots/Part-1.5.png)

    список замапленных портов:

    ![OS](screnshots/Part-1.6.png)

    и ip контейнера:

    ![OS](screnshots/Part-1.7.png)

* Остановил докер образ через ```docker stop [container_id|container_name]```.

    ![OS](screnshots/Part-1.8.png)

* Проверил, что образ остановился через ```docker ps```.

    ![OS](screnshots/Part-1.9.png)

**Запустил докер с портами 80 и 443 в контейнере, замапленными на такие же порты на локальной машине, через команду run.**

* Поскольку докер запущен на виртуальной машине, то чтобы зайти на сервер nginx с браузера основной машины, нужно настроить виртуальный адаптер хоста:

1) в ```"File/Network Manager"``` выбираем Host-only Networks и создаем сеть vboxnet0

    ![OS](screnshots/Part-1.11.png)

2) Настроим ```Сеть в VirtualBox``` на Host-only Network

    ![OS](screnshots/Part-1.12.png)

3) Виртуальному адаптеру назначен ip адрес 192.168.56.2 -> Проверим соединение с основной машины до виртуальной машины командой ```ping```

    ![OS](screnshots/Part-1.13.png)

4) Запустил докер с портами 80 и 443 в контейнере, замапленными на такие же порты на локальной машине, через команду ```run``` -> проверил соединение командой ```curl```

    ![OS](screnshots/Part-1.14.png)

5) Открыл на браузере основной машины сервер nginx через виртуальный адаптер по адресу 192.168.56.2

    ![OS](screnshots/Part-1.15.png)

* Перезапустил докер контейнер через ```docker restart [container_id|container_name]```    
* Командой ```curl проверил соединение с сервером```

    ![OS](screnshots/Part-1.16.png)

## Part 2. Операции с контейнером

* Прочитал конфигурационный файл nginx.conf внутри докер контейнера через команду ```exec```.

    * Зашел в контейнер

    ![OS](screnshots/Part-2.1.png)

    * перешел в директорию nginx

    ![OS](screnshots/Part-2.2.png)

    * открыл файл конфига

    ![OS](screnshots/Part-2.3.png)

    * вышел из контейнера

    ![OS](screnshots/Part-2.4.png)

* Создал на локальной машине файл *nginx.conf*.

* Настроил в нем по пути */status* отдачу страницы статуса сервера nginx.

    ![OS](screnshots/Part-2.5.png)

* Скопировал созданный файл *nginx.conf* внутрь докер-образа через команду ```docker cp```.

    ![OS](screnshots/Part-2.6.png)

* Перезапустил nginx внутри докер-образа через команду ```exec```.

    ![OS](screnshots/Part-2.7.png)

* Проверил, что по адресу *localhost:80/status* отдается страничка со статусом сервера nginx.

    ![OS](screnshots/Part-2.8.png)

* Экспортировал контейнер в файл *container.tar* через команду ```export```.
* Остановил контейнер.

    ![OS](screnshots/Part-2.9.png)

* Удалил образ через ```docker rmi [image_id|repository]```, не удаляя перед этим контейнеры.

    ![OS](screnshots/Part-2.10.png)

* Удали остановленный контейнер.

    ![OS](screnshots/Part-2.11.png)

* Импортировал контейнер обратно через команду ```import```.

    ![OS](screnshots/Part-2.12.png)

* Запустил импортированный контейнер.

    ![OS](screnshots/Part-2.13.png)

* Проверил, что по адресу *localhost:80/status* отдается страничка со статусом сервера nginx.

    ![OS](screnshots/Part-2.14.png)

## Part 3. Мини веб-сервер

* Напишем мини-сервер на C и FastCgi, который будет возвращать простейшую страничку с надписью Hello World!.

* Чтобы создать свой мини веб-сервер, необходимо создать .c файл, в котором будет описана логика сервера (в нашем случае - вывод сообщения Hello World!), а также конфиг nginx.conf, который будет проксировать все запросы с порта 81 на порт 127.0.0.1:8080

* Запустим контейнер на порту 81

    ![OS](screnshots/Part-3.1.png)

* Перейдем в контейнер с помощью команды ```sudo docker exec -it ``` предварительно узнав название командой ```sudo docker ps -a```

    ![OS](screnshots/Part-3.2.1.png)

    ![OS](screnshots/Part-3.2.png)

* Обновим систему командой ```apt update``` и установим необходимы пакеты ```apt install gcc vim spawn-fcgi libfcgi-dev```

    ![OS](screnshots/Part-3.3.png)
 
* В директории ```home``` создадим файл и напишем следующий код на Си

    ![OS](screnshots/Part-3.4.png)

* Изменим файл ```/etc/nginx/nginx.conf``` следующим образом (поставил порт 81 на прослушивание и подключил fastcgi), т.е проксирование всех запросов с порта 81 на 127.0.0.1:8080

    ![OS](screnshots/Part-3.5.png)

    ![OS](screnshots/Part-3.6.png)

* Перейдем в директорию ```home``` и скомпилируем файл Си

    ![OS](screnshots/Part-3.7.png)

* Запустим испольняемы файл на порту 8080 используя spawn-fcgi и перечитаем файл конфигурации nginx без остановки работы командой ```nginx -s reload```

    ![OS](screnshots/Part-3.8.png)

* Командой ```curl localhost:81``` убедимся, что сервер корректно отвечает на запрос

    ![OS](screnshots/Part-3.9.png)

* Подключив виртуальный адаптер хоста убедимся в корректной работе сервера через браузер

    ![OS](screnshots/Part-3.10.png)

    ![OS](screnshots/Part-3.11.png)


**Если сервер работает некоректно или возвращает ошибку, то сноси контейнер с образом и устанавливай по новой !!!**

## Part 4. Свой докер
* Напишем докер-образ для созданного сервера, который будет:

    1) собирать исходники мини сервера на FastCgi из Части 3;

    2) запускать его на 8080 порту;

    3) копировать внутрь образа написанный ./nginx/nginx.conf;

    4) запускать nginx.

* nginx можно установить внутрь докера самостоятельно, а можно воспользоваться готовым образом с nginx'ом, как базовым. Используем второй способ.

* Dockerfile и run.sh

    ![OS](screnshots/Part-4.8.png)

    ![OS](screnshots/Part-4.9.png)

* Собрал написанный докер-образ через docker build при этом указав имя и тег командой ```sudo docker build -t miniserver:1 .```

    ![OS](screnshots/Part-4.1.png)

* Проверил через docker images, что все собралось корректно ```sudo docker images```.

    ![OS](screnshots/Part-4.2.png)

* Запустил собранный докер-образ с маппингом 81 порта на 80 на локальной машине и маппингом файла nginx.conf внутрь контейнера по адресу, где лежат конфигурационные файлы nginx'а (см. Часть 2).
```sudo docker run -d -p 80:81 -v $(pwd)/nginx.conf:/etc/nginx/nginx.conf miniserver:1```

    ![OS](screnshots/Part-4.3.png)

* Проверил, что по localhost:80 доступна страничка написанного мини сервера.

    ![OS](screnshots/Part-4.4.png)

* Проверил что в данный момент в ```localhost:80/status``` не отображается статус

    ![OS](screnshots/Part-4.5.png)

* Дописал в /nginx.conf проксирование странички /status, по которой надо отдавать статус сервера nginx.

    ![OS](screnshots/Part-4.6.png)    

* Перезапусти докер-образ. Если всё сделано верно, то, после сохранения файла и перезапуска контейнера, конфигурационный файл внутри докер-образа должен обновиться самостоятельно без лишних действий

* Проверил, что теперь по localhost:80/status отдается страничка со статусом nginx

    ![OS](screnshots/Part-4.7.png)

## Part 5. Dockle

* После написания образа проверим его на безопасность

* Установим докл с удаленного репозитория командой ```curl -L -o dockle.deb https://github.com/goodwithtech/dockle/releases/download/v0.4.10/dockle_0.4.10_Linux-64bit.deb``` далее введем команду для установки пакета ```sudo dpkg -i dockle.deb```.

    ![OS](screnshots/Part-5.1.png)

    ![OS](screnshots/Part-5.2.png)

* Просканировал образ из предыдущего задания командой ```dockle [image_id|repository]```.

    ![OS](screnshots/Part-5.3.png)

* Исправил Dockerfile.

    ![OS](screnshots/Part-5.6.png)

    * выполним команду ```sudo docker build -t server:1 .``` с уже измененным докер файлом

        ![OS](screnshots/Part-5.4.png)

    * Докл после изменения докер файла

        ![OS](screnshots/Part-5.7.png)

* Предупреждения об ошибках докл и их описания:

    * CIS-DI-0001
    > Создайте пользователя для контейнера
    > Создайте пользователя, не являющегося пользователем root, для контейнера в Dockerfile для образа контейнера.

    > Хорошей практикой является запуск контейнера от имени пользователя, не имеющего прав root, если это возможно.

    * CIS-DI-0005
    > Включите доверие к контенту для Docker. По умолчанию доверие к контенту отключено. Его следует включить. ```export DOCKER_CONTENT_TRUST=1``` перед пулом или билдом контейнера


    * CIS-DI-0006
    > Добавить HEALTHCHECK инструкция к изображению контейнера
    > Добавить HEALTHCHECK инструкция в ваших образах контейнеров docker для выполнения проверки работоспособности запущенных контейнеров.
    > На основе сообщенного состояния работоспособности движок docker затем может завершить работу с нерабочими контейнерами и создать экземпляры новых.
        Dockerfile:
            > HEALTHCHECK --interval=5m --timeout=3s \
            > CMD curl -f http://localhost/ || exit 1

    * CIS-DI-0008
    > Подтвердите безопасность setuid и setgid Файлы
    > Удаление setuid и setgid разрешения в изображениях предотвратили бы атаки с повышением привилегий в контейнерах.
    > setuid и setgid разрешения могут быть использованы для повышения привилегий.
        Dockerfile:
            > chmod u-s setuid-file
            > chmod g-s setgid-file

    * DKL-DI-0005

    > Очистить apt-get кэши

    > Использование apt-get clean && rm -rf /var/lib/apt/lists/* после apt-get install.

        > https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#apt-get

        > Кроме того, когда вы очищаете apt cache путем удаления /var/lib/apt/lists это уменьшает размер изображения, поскольку кэш apt не хранится в слое. Поскольку RUN утверждение начинается с apt-get update, кэш пакета всегда обновляется перед запуском apt-get install.


## Part 6. Базовый Docker Compose

* Установил docker-compose командой ```sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose```

    ![OS](screnshots/Part-6.1.png)

* Написал файл docker-compose.yml, с помощью которого:

1) Поднимется докер-контейнер из Части 5 (он должен работать в локальной сети, т.е. не нужно использовать инструкцию EXPOSE и мапить порты на локальную машину).

2) Поднимется докер-контейнер с nginx, который будет проксировать все запросы с 8080 порта на 81 порт первого контейнера.

    ![OS](screnshots/Part-6.4.png)

* Замапил 8080 порт второго контейнера на 80 порт локальной машины.

* Остановил все запущенные контейнеры командой ```sudo docker-compose down```

* Собрал и запустил проект с помощью команд ```sudo docker-compose build``` и ```sudo docker-compose up```.

    ![OS](screnshots/Part-6.2.png)

* Проверил, что в браузере по localhost:80 отдается написанная тобой страничка, как и ранее (через виртуальный адаптер, т.к докер стоит на виртуальной машине).

    ![OS](screnshots/Part-6.3.png)

* Более подробный гайд по docker-compose можно увидеть по этой ссылке https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-compose-on-ubuntu-20-04

