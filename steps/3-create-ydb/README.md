# Yandex Database
## Создание базы

Создадим базу данных YDB с именем `for-serverless-shortener` и типом serverless используя для этого флаг `--serverless`:

    yc ydb database create for-serverless-shortener --serverless --folder-id $FOLDER_ID
    yc ydb database list

Сразу получим и сохраним значение endpoint и database в значение переменной `YDB_ENDPOINT` и `YDB_DATABASE` они нужны будут для подключения нашей функции. В выводе следующей команды есть значение endpoint оно обычно состоит из :

    yc ydb database get --name for-serverless-shortener

echo "export YDB_ENDPOINT=ydb.serverless.yandexcloud.net:2135" >> ~/.bashrc && . ~/.bashrc
echo "export YDB_DATABASE=/ru-central1/b1ga4gj7agij03ln6aov/etn4ctcbnsculqb83qiq" >> ~/.bashrc && . ~/.bashrc

    echo "export YDB_ENDPOINT=<YDB_ENDPOINT>" >> ~/.bashrc && . ~/.bashrc
    echo $YDB_ENDPOINT

    echo "export YDB_DATABASE=<YDB_DATABASE>" >> ~/.bashrc && . ~/.bashrc
    echo $YDB_DATABASE

Для дальнейшей работы воспользуемся

    curl https://storage.yandexcloud.net/yandexcloud-ydb/install.sh | bash

С помощью CLI Yandex.Cloud создадим авторизованный ключ сервисного аккаунта `serverless-shortener`:

    yc iam key create \
    --service-account-name <имя_сервисного_аккаунта> \
    --output <имя_файла>

    yc iam key create \
    --service-account-name serverless-shortener \
    --output serverless-shortener.sa

Проверим работоспособность с помощью команды:

    ydb \
    --endpoint <эндпоинт> \
    --database <база данных> \
    --sa-key-file <путь к файлу с ключом>\
    discovery whoami \
    --groups

При запуске команды YDB CLI в параметре `--sa-key-file` укажите путь к файлу с авторизованным ключом доступа сервисного аккаунта. Чтобы не указывать этот параметр при каждом вызове команды, сохраните путь к файлу в переменную окружения `SA_KEY_FILE`. 

    echo "export SA_KEY_FILE=$PWD/serverless-shortener.sa" >> ~/.bashrc && . ~/.bashrc  
    echo $SA_KEY_FILE

    ydb \
    --endpoint $YDB_ENDPOINT \
    --database $YDB_DATABASE \
    --sa-key-file $SA_KEY_FILE \
    discovery whoami \
    --groups

Воспользуемся SQL скриптом для создания таблицы из файла `links.yql`. Запустим создание таблицы и проверим результат создания:

    ydb \
    --endpoint $YDB_ENDPOINT \
    --database $YDB_DATABASE \
    --sa-key-file $SA_KEY_FILE \
    scripting yql --file links.yql

    ydb \
    --endpoint $YDB_ENDPOINT \
    --database $YDB_DATABASE \
    --sa-key-file $SA_KEY_FILE \
    scheme describe links
