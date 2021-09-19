# Yandex API Gateway
## Создание спецификации

Создадим спецификацию `for-serverless-shortener.yml` и используем ее для инициализации:

    yc serverless api-gateway create \
    --name for-serverless-shortener \
    --spec=for-serverless-shortener-1.yml \
    --description "for serverless shortener"

В результате успешного создания API-шлюза получим `domain`:
    
    yc serverless api-gateway list
    yc serverless api-gateway get --name for-serverless-shortener

Скопируем служебный домен, чтобы проверить работоспособность API-шлюза и нашего приложения целиком. Вставьте адрес в браузер. Должно получиться следующее:

    https://<ID API Gateway>.apigw.yandexcloud.net/

Добавляйте адреса сайтов, они будут сохранятся в базу данных. А вам будет доступна ссылка за которой будет скрываться оригиналььный адрес. Ваше приложение полностью работоспособно теперь вы умеете использовать serverless стек технологий Yandex.Cloud.
Мы создали приложение с использование Cloud Functions, API GW, Object Storage, Yandex Database. Конечно вы можете развивать его в разные стороны, расширяя функциональность по необходимости.  

