# Создание Функции

Ранее мы использовали рантайм `python37`, однако с версии рантайма `python39` библиотеки необходиомо указывать явно. Для работы с `requirements.txt` есть удобная python библиотека `pipreqs`. Для генерации `requirements.txt` с помощью `pipreqs` достаточно указать рабочий каталог. В большинстве интерпритаторов linux для указания текущего каталога есть переменная `$PWD`. Если файл `requirements.txt` уже есть и его нужно актуализировать то используется флаг `--force`, например:  

    pip install pipreqs
    pipreqs $PWD --print
    pipreqs $PWD --force

Находясь в директории с исходными файлами, упакуем все нужные нам файлы в zip-архив:

    zip src.zip index.py requirements.txt 

В переменные окружения функции необходимо добавить три переменные: Первая — `endpoint` — нужно указать протокол `grpcs://` и добавить значение Эндпоинт из секции YDB эндпоинт, обычно получается `grpcs://ydb.serverless.yandexcloud.net:2135`. Вторая — `database` — это значение поля База данных из секции YDB эндпоинт, начинаться с /ru-central1/.... мы ранее уже создали ее `YDB_DATABASE`. Третья — `USE_METADATA_CREDENTIALS` — выставите значение переменной в `1`.


Создадим нашу функцию `for-serverless-shortener`, при этом сразу зададим все необходимые переменные и сервисный аккаунт:

    yc serverless function create \
    --name for-serverless-shortener \
    --description "function for serverless-shortener"

    yc serverless function version create \
    --function-name for-serverless-shortener \
    --memory=256m \
    --execution-timeout=5s \
    --runtime=python37 \
    --entrypoint=index.handler \
    --service-account-id $SERVICE_ACCOUNT_SHORTENER_ID \
    --environment USE_METADATA_CREDENTIALS=1 \
    --environment endpoint=grpcs://ydb.serverless.yandexcloud.net:2135 \
    --environment database=$YDB_DATABASE \
    --source-path src.zip

    yc serverless function allow-unauthenticated-invoke for-serverless-shortener