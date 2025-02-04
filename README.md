# SmartApp Framework

**SmartApp Framework** - это Python-фреймворк, который позволяет создавать смартапы для виртуальных ассистентов Салют. 


## Оглавление
   * [Конфигурация](#Конфигурация)
     * [Фреймворк и смартапы](#Фреймворк)
     * [Инструменты фреймворка](#Инструменты)
     * [Рекомендованные требования](#Рекомендованные)
   * [Настройка фреймворка](#Настройка)
     * [Обновление фреймворка](#Обновление)
     * [Установка фреймворка](#Установка)
     * [Создание проекта](#Создание)
     * [Тестирование онлайн](#Тестирование) 
     * [Тестирование офлайн](#Тестирование)    
   * [Документация](#Документация)
   * [Обратная связь](#Обратная)

____

# Конфигурация

## Фреймворк и смартапы

Смартап - это приложение для виртуального ассистента Салют. С помощью смартапов пользователи могут вызвать такси, узнать погоду, управлять устройствами умного дома, записаться в салон красоты и совершить прочие действия, которые можно доверить ассистенту. 

Виртуальный ассистент понимает текущие намерения пользователя и для каждой его реплики подбирает соответствующий запрос на выполнение ([интент](https://developer.sberdevices.ru/docs/ru/platform_services/nlu_guidelines/intents_overview)). Поведение смартапа для различных интентов описывается с помощью сценариев. Интенты и сценарии связываются через смартапы, написанные на SmartApp Framework. 


## Инструменты фреймворка

Фреймворк содержит следующие инструменты:

* инструменты для создания сценариев;
* решения для автоматического тестирования;
* демо-приложение для просмотра примеров реализации; 
* готовые механизмы для слот-филлинга и извлечения сущностей из текста. 


## Рекомендованные требования

* Linux, Mac OS или Windows (необходима установка [Conda](https://docs.conda.io/en/latest/)).
* 512 МБ свободной памяти.
* Python 3.6.8 - 3.9.6.

____



# Настройка фреймворка

## Обновление фреймворка

Для перехода на новую версию фреймворка выполните в терминале следующие команды:

```bash
python3 -m pip uninstall -y smart-app-framework
python3 -m pip install git+https://github.com/sberdevices/smart_app_framework@main
```

При переходе на версию фреймворка >=1.0.7.rc4 со старым смартапом необходимо в директории смартапа из файла ```static/.text_normalizer_resources/static_workdata.json``` удалить строки 'Ё на Е'.

## Установка фреймворка

Для установки фреймворка выполните в терминале следующую команду:

```bash
python3 -m pip install git+https://github.com/sberdevices/smart_app_framework@main
```

## Установка MacOS на M1

```
brew install librdkafka openssl@1.1 hdf5 python@3.9

python3 -m venv venv && source venv/bin/activate

pip3 install Cython numpy pkgconfig

export PKG_CONFIG_PATH="$(brew --prefix)/opt/openssl@1.1/lib/pkgconfig:$(brew --prefix)/opt/librdkafka/lib/pkgconfig"


GRPC_PYTHON_BUILD_SYSTEM_OPENSSL=1 \
GRPC_PYTHON_BUILD_SYSTEM_ZLIB=1 \
CFLAGS="-I$(brew --prefix)/opt/hdf5/include $(pkg-config --cflags rdkafka openssl)" \
LDFLAGS="-L$(brew --prefix)/opt/hdf5/lib $(pkg-config --libs rdkafka openssl)" \
python3 setup.py build


GRPC_PYTHON_BUILD_SYSTEM_OPENSSL=1 \
GRPC_PYTHON_BUILD_SYSTEM_ZLIB=1 \
CFLAGS="-I$(brew --prefix)/opt/hdf5/include $(pkg-config --cflags rdkafka openssl)" \
LDFLAGS="-L$(brew --prefix)/opt/hdf5/lib $(pkg-config --libs rdkafka openssl)" \
python3 setup.py install


CFLAGS="$(pkg-config --cflags rdkafka openssl)" \
LDFLAGS="$(pkg-config --libs rdkafka openssl)" \
pip3 wheel confluent_kafka==1.7.0
```

## Создание проекта

Для создания проекта выполните в терминале следующую команду:
```bash
python3 -m smart_kit create_app <YOUR_APP_NAME>
```
После этого в текущей директории появится каталог с проектом. Он уже содержит в себе всё необходимое для запуска минимального приложения, включая базовый сценарий hello_scenario. Описание сценариев и форм можно найти в <YOUR_APP_NAME>/static/references/.


## Тестирование онлайн

Для тестирования онлайн вам понадобится мобильное приложение Сбер Салют или собственное устройство, на котором будет запускаться смартап. Для такого тестирования:

1. Запустите в терминале dev сервер:

```bash
python3 manage.py run_app
```

2. Передайте в интернет порт. Для этого потребуется внешний IP-адрес. Если у вас его нет, воспользуйтесь специальными сервисами (например, Ngrok).
3. Зарегистрируйтесь в кабинете разработчика - [SmartApp Studio](https://smartapp-studio.sberdevices.ru/login).
4. Создайте в [SmartApp Studio](https://smartapp-studio.sberdevices.ru/login) свой смартап. 
5. Перейдите в настройки смартапа и укажите в поле "Настройки вебхука" адрес вашего сервера. Сохраните изменения.
6. Запустите свой смартап с помощью фразы "Запусти <имя приложения>". 

В терминале должны появиться записи о входящем сообщении, а ассистент ответит приветствием согласно сценарию hello_scenario.


## Тестирование офлайн

Ниже представлен пример команды для терминала при тестировании офлайн и пример ответа, который выводится на экране: 
```console
localhost:~$ python <YOUR_APP_NAME>/manage.py local_testing
Текущий сценарий: hello_scenario
Привет! Введите help или ? для вызова списка команд.
> set intent run_app // смена интента на другой. По умолчанию имя сценария совпадает с именем интента
intent = run_app
> Привет
pronounceText: Как тебя зовут?
```


____



# Документация

Вы можете ознакомиться с подробной документацией по работе со SmartApp Framework в [справочнике разработчика](https://developers.sber.ru/docs/ru/salute/python-framework/overview).



# Обратная связь

C вопросами и предложениями пишите нам по адресу developer@sberdevices.ru или вступайте в 
наш Telegram канал - [SmartMarket Community](https://t.me/smartmarket_community). 
