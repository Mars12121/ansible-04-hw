# Домашнее задание к занятию "Работа с roles" - Морозов Александр



#### 1. Создайте в старой версии playbook файл `requirements.yml` и заполните его содержимым:
[requirements.yml](playbook%2Frequirements.yml)

   ```yaml
   ---
     - src: git@github.com:AlexeySetevoi/ansible-clickhouse.git
       scm: git
       version: "1.13"
       name: clickhouse 
   ```

#### 2. При помощи `ansible-galaxy` скачайте себе эту роль.

Ответ:
![alt text](https://github.com/Mars12121/ansible-04-hw/blob/main/img/1.png)

#### 3. Создайте новый каталог с ролью при помощи:
`ansible-galaxy role init vector-role`

Ответ:
![alt text](https://github.com/Mars12121/ansible-04-hw/blob/main/img/2.png)

#### 4. На основе tasks из старого playbook заполните новую role. Разнесите переменные между `vars` и `default`. 
#### 5. Перенести нужные шаблоны конфигов в `templates`.
#### 6. Опишите в `README.md` обе роли и их параметры. Пример качественной документации ansible role [по ссылке](https://github.com/cloudalchemy/ansible-prometheus).
#### 7. Повторите шаги 3–6 для LightHouse. Помните, что одна роль должна настраивать один продукт.

Ответ:
![alt text](https://github.com/Mars12121/ansible-04-hw/blob/main/img/3.png)

#### vector-role:

role: [vector-role/tasks/main.yml](playbook%2Froles%2Fvector-role%2Ftasks%2Fmain.yml)

handlers: [vector-role/handlers/main.yml](playbook%2Froles%2Fvector-role%2Fhandlers%2Fmain.yml)

шаблон `templates/vector/vector.yaml.j2` перемещен в каталог роли [vector-role/templates/vector.yaml.j2](playbook%2Froles%2Fvector-role%2Ftemplates%2Fvector.yaml.j2)

vars: [vector-role/vars/main.yml](playbook%2Froles%2Fvector-role%2Fvars%2Fmain.yml)

в переменные роли вынесены параметры `пользователь и его группа в ОС, - владелец ПО vector`, переопределение которых на других окружениях не требуется:  
```yaml
vector_os_user: "vector"
vector_os_group: "vector"
```
defaults: [vector-role/defaults/main.yml](playbook%2Froles%2Fvector-role%2Fdefaults%2Fmain.yml)

в переменные по умолчанию вынесены параметры `версия vector`, `архтектура ОС`, 
`временный рабочий каталог для размещения дистрибутива с vector`, `каталог кофигов для vector`, 
которые с большой вероятностью могут быть переопределены по мере изменения версии vector; рабочий каталог и каталог 
конфигов тоже может быть изменен на других окружениях.
```yaml
vector_version: "0.21.1"
vector_os_arh: "x86_64"
vector_workdir: "/tmp/vector"
vector_data_dir: "/var/lib/vector"
```

#### lighthouse-role: 

role: [lighthouse-role/tasks/main.yml](playbook/roles/lighthouse-role/tasks/main.yml)  
handlers: [lighthouse-role/handlers/main.yml](playbook/roles/lighthouse-role/handlers/main.yml)  
шаблон `templates/nginx/ligthouse.conf.j2` перемещен в каталог роли [lighthouse-role/templates/nginx/ligthouse.conf.j2](playbook/roles/lighthouse-role/templates/nginx/ligthouse.conf.j2)  
vars:  [lighthouse-role/vars/main.yml](playbook/roles/lighthouse-role/vars/main.yml)  
в переменные роли вынесены параметры `репозиторий с исходным кодом lighthouse`, `пакеты, обязательные к установке`, 
являющиеся неотъемлемой частью роли и поэтому не требующие возможности переопределения:  
```yaml
lighthouse_code_src: "https://github.com/VKCOM/lighthouse.git"
lighthouse_code_src_version: d701335
lighthouse_packages:
  - nginx
  - git
```
defaults:  [lighthouse-role/defaults/main.yml](playbook/roles/lighthouse-role/defaults/main.yml)  
в переменные по умолчанию вынесены параметры `каталог установки lighthouse`, `порт веб-сервера`, `имя конфига веб-сервера`, 
которые с большой вероятностью могут быть переопределены на других окружениях.
```yaml
lighthouse_data_dir: "/lighthouse/"
lighthouse_nginx_port: 8080
lighthouse_nginx_conf: "lighthouse.conf"
```


#### 8. Выложите все roles в репозитории. Проставьте теги, используя семантическую нумерацию. Добавьте roles в `requirements.yml` в playbook.

Ответ:

Выгружаем роль "Vector role" в удаленный репозиторий
![alt text](https://github.com/Mars12121/ansible-04-hw/blob/main/img/4.png)

Выгружаем роль "Lighthouse role" в удаленный репозиторий
![alt text](https://github.com/Mars12121/ansible-04-hw/blob/main/img/5.png)

Добавляем роли в `requirements.yml` в playbook
![alt text](https://github.com/Mars12121/ansible-04-hw/blob/main/img/6.png)

#### 9. Переработайте playbook на использование roles. Не забудьте про зависимости LightHouse и возможности совмещения `roles` с `tasks`.

Ответ:
[site.yml](playbook%2Fsite.yml)

Работа playbook и предварительное скачивание требуемых ролей, описанных в `requirements.yml`:
![alt text](https://github.com/Mars12121/ansible-04-hw/blob/main/img/6.png)

#### 10. Выложите playbook в репозиторий.
#### 11. В ответе дайте ссылки на оба репозитория с roles и одну ссылку на репозиторий с playbook.

---