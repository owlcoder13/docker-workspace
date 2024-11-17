# Установка
Положи свои id_rsa и id_rsa.pub в папку main, чтобы можно было по ssh присоединяться

# Как закинуть дамп в базу?

docker cp dump.sql postgres:~/dump.sql
docker exec postgres bash

и далее накатываешь дамп куда нужно