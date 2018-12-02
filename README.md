## Запуск приложения:

Выкачиваем проект из ветки master.
Создаем базу данных **PostgreSQL** с названием **sinam_db**
и пользователя с логином **sinam_user** и паролем **admin**.

Запускаем в базе скрипты из файла **resources/init_tables.sql**
Создастся две таблицы: **users** и **tasks** 
Также в таблице **users** создаётся юзер с ролью **ADMIN** 

Проверяем не занят ли порт **8082**, либо прописываем свой в **application.properties**
Запускаем приложение с помощью IDE либо мавена.

## Общее описание приложения:

По умолчанию у нас существует юзер с ролью **ADMIN** и id 1.
Данный юзер может создать новых юзеров с любой ролью.
Новые юзеры с ролью **ADMIN** также могут созвать других юзеров.
Для создания новых юзеров, получения доступа к операциям в которых нужны
привелегии админа(к примеру получение **Black list**-a), нужно передавать 
контроллеру в качестве параметра айди юзера от чьего имени 
должна проходить операция. К примеру:
`http://localhost:8082/users/save?currentUserId=1`
Данный функционал имитирует реальную систему с секъюрити и токенами 
на основе данных о юзере.
В случае если currentUserId принадлежит юзеру с ролью **USER** вернется 
сообщение об ошибке. 

## Описание REST API

**Создание нового юзера:**
`[host:port]/users/save` 

**POST** - запрос в который нужно передать юзера в качестве request body и 
currentUserId(должен принадлежать **ADMIN**-у для корректного проведения операции).
id юзеру можно не указывать, т к в базе действует автоинкремент.
В случае удачного проведения вернёт нового юзера вместе с его id.

**Активация юзера**
`[host:port]/activate/{id}`

**PUT** - запрос  в который нужно передать id юзера которого хотите активировать 
в качестве path variable и currentUserId(должен принадлежать **ADMIN**-у для 
корректного проведения операции).

**Возращения списка юзеров у которых аккаунт деактивирован "black_list"**
`[host:port]/black_list`

**GET** - запрос в который нужно передать currentUserId(должен принадлежать **ADMIN**-у для 
 корректного проведения операции) в качестве request param т.к список деактивированных
 юзеров доступны только админу
 
**Создание нового задания:**
`[host:port]/tasks/save` 

**POST** - запрос в который нужно передать задание в качестве request body и 
currentUserId(должен принадлежать **ADMIN**-у для корректного проведения операции).


**Возвращение списка заданий определенного юзера**
`[host:port]/get_user_tasks/{userId}`

**GET** - запрос в который нужно передать id юзера чьи задания хотите просмотреть
в качестве path variable

**Возвращение списка всех заданий**
`[host:port]/all`

**GET** - запрос который возвращает список всех заданий