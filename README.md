# Exorde Participation Module CLI v1.3.4.2 (Docker-RU) Guide

### Минимальные требования: 

* 2 или больше физических ядра CPU 
* 1 Гб памяти SSD
* 4 Гб оперативной памяти
* 100 мб подключение интернета

## Официальные ссылки на проект:

[Еxplorer](https://explorer.exorde.network/)

[Таблица лидеров](https://explorer.exorde.network/leaderboard)

[Сайт](https://exorde.network/)

[Дискорд](https://discord.gg/y9F5Qrtk)

[Телеграм](https://t.me/exorde)

---

### Устанавливаем Docker. Нужна версия не старее `Docker version 20.10.20`

Перед началом работы переходим в папку `root`:

```
cd /root
```

1. Обновляем индексы пакетов `apt` с помощью `update`:

```
sudo apt update
```
2. Устанавливаем набор пакетов, необходимых для доступа к репозиторию Docker по HTTPS:

```
sudo apt install apt-transport-https ca-certificates software-properties-common curl 
```

3. Теперь нужно добавить в apt GPG-ключ для работы с репозиторием Docker. GPG-ключи используются для проверки подписей программного обеспечения. Выполняем эту команду:

```
curl -f -s -S -L https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

4. Добавляем репозиторий Docker в локальный список:

```
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
```

5. Ещё раз обновим индекс пакетов:

```
sudo apt update
```

6. Установим докер. Параметры `-y` в автоматическом режиме ответит на все вопросы установщика `Yes`:

```
sudo apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin -y
```

7. Проверим статус Docker: статус должен выглядеть так `Active: active (running) since Mon 2022-11-07 11:43:24 UTC; 3h 48min ago` 

```
sudo systemctl status docker
```

Если выбивает ошибку, повторить пункт №6

---

## Запуск ноды:

Команда запускает модуль в фоновом режиме и она работает постоянно.

```
docker run --restart unless-stopped -d --pull always --name exorde-cli exordelabs/exorde-cli -m <your-main-ethereum-address> -l 2
```

Сразу после запуска может показываеть версию 1.3.4a, и ошибку [Faucet] `Auto-Faucet n° 176  Failure... retrying. [Faucet] selecting Auto-Faucet n° 433` нужно подождать 30 мин или более, что бы нода синхронизировалась и потом сделать рестартр ноды.

* Пример:

```
docker run --restart unless-stopped -d --pull always --name exorde-cli exordelabs/exorde-cli -m 0x16f17726399DfF6fc84AD013BD9bCB70F39b42d3 -l 2
```

Данной командой можно создать несколько контейнеров с уникальным адресом воркера.

* Примечание к коду:

>`YOUR_MAIN_ADDRESS` - это адресс с кошелька Метамаск "Сеть Ethereum Mainnet", должен быть действительным (желательно новый и пустой).

>`YOUR_NAME` - ето название ноды, каждое должно быть уникальным. 

>`LOGGING` - это уровень обработки логов в модуле (это не столь важно на данном обновлении, по статистике берите 2 или 3):

* 1 = без логов
* 2 = общая обработка логов
* 3 = валидация + сбор и анализ логов
* 4 = детальная валидация + сбор и анализ логов (для устранение ошибок)
---

### Полезные команды:

Аргумент пакетной обработки нод `$(docker ps -a -q)` используйте его когда нужно перезапустить, удалить, стартонуть все ноды. Пример: `docker start $(docker ps -a -q)`

Проверка логов:

```
docker logs --follow  id контейнера
```

* Пример:

```
docker logs --follow  1f77bd5b66e1
```

Проверка ревардов:

```
docker logs --follow exorde_1 -n 100 | grep "CURRENT REWARDS"
```

*exorde_1 - это название вшего контейнера.

Посмотреть нагрузку на сервер:

```
top
```

Обзор созданых контейнеров:

```
docker ps -a
```

Остановить модуль:

```
docker stop имя/id контейнера
```

Удалить ноду:

```
docker stop имя/id контейнера
docker rm имя/id контейнера
```

Перезагрузить модуль:

```
docker restart имя/id контейнера
```

Переименовать ноду:

```
docker rename oldname newname
```

---

