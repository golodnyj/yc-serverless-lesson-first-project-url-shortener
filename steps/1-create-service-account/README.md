# Service Account
## Создание аккаунта

Создайте сервисный аккаунт с именем `serverless-shortener`: 

    export SERVICE_ACCOUNT_SHORTENER_ID=$(yc iam service-account create --name serverless-shortener \
    --description "service account for serverless" \
    --format json | jq -r .)

Проверьте текущий список сервисных аккаунтов:
     
    yc iam service-account list

После проверки запишите ID, созданного сервисного аккаунта, в переменную `SERVICE_ACCOUNT_SHORTENER_ID`:

    echo "export SERVICE_ACCOUNT_SHORTENER_ID=<ID>" >> ~/.bashrc && . ~/.bashrc  
    echo $SERVICE_ACCOUNT_SHORTENER_ID

## Назначение роли сервисному аккаунту

Добавим вновь созданному сервисному аккаунту роль `editor`, `storage.viewer` и `ydb.admin`:

    echo "export FOLDER_ID=$(yc config get folder-id)" >> ~/.bashrc && . ~/.bashrc 
    echo $FOLDER_ID

    echo "export OAUTH_TOKEN=$(yc config get token)" >> ~/.bashrc && . ~/.bashrc
    echo $OAUTH_TOKEN

    echo "export CLOUD_ID=$(yc config get cloud-id)" >> ~/.bashrc && . ~/.bashrc
    echo $CLOUD_ID

    yc resource-manager folder add-access-binding $FOLDER_ID \
    --subject serviceAccount:$SERVICE_ACCOUNT_SHORTENER_ID \
    --role editor

    yc resource-manager folder add-access-binding $FOLDER_ID \
    --subject serviceAccount:$SERVICE_ACCOUNT_SHORTENER_ID \
    --role ydb.admin

    yc resource-manager folder add-access-binding $FOLDER_ID \
    --subject serviceAccount:$SERVICE_ACCOUNT_SHORTENER_ID \
    --role storage.viewer