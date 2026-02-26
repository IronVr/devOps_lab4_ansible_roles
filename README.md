# devOps_lab4_ansible_roles

## Подготовка среды и сетевая настройка
* Выполнена в devOps_lab2_ansible
---
## Автоматизация с Ansible

Все действия выполняются на **первой** виртуальной машине:

1. **Подготовка структуры проекта:**
   * Создан файл инвентаря `inventory.ini` с указанием IP целевой ВМ.
   * Создана роль `nginx-vhosts` со следующей структурой:
     ```
     roles/
     └── nginx-vhosts/
         ├── tasks/
         │   └── main.yml
         ├── handlers/
         │   └── main.yml
         └── templates/
             ├── index.html.j2
             └── site.conf.j2
     ```

2. **Разработка Playbook и роли:**
   * В `playbook.yml` определён список виртуальных хостов (сайтов) через переменную `sites`:
     ```yaml
     sites:
       - mehmat.ru
       - fizfak.ru
       - etis.com
     ```
   * Роль `nginx-vhosts` включает задачи:
     - установка пакета `nginx`,
     - создание корневых каталогов `/var/www/<сайт>`,
     - копирование индексной страницы из шаблона `index.html.j2`,
     - развёртывание конфигурации виртуального хоста из шаблона `site.conf.j2` в `/etc/nginx/conf.d/`,
     - удаление стандартного сайта,
     - запуск и включение сервиса Nginx.
   * Добавлен обработчик `restart nginx` для перезапуска сервиса при изменении конфигурации или индексных файлов.

3. **Запуск:**
   * Плейбук выполняется скриптом с содержимым:
     ```bash
     ansible-playbook -i inventory.ini playbook.yml
     ```

4. **Проверка результатов:**

   * Проверена доступность сайтов с помощью `curl` с подменой заголовка `Host`:
     ```bash
     curl 10.0.2.16 -H 'Host: mehmat.ru'
     curl 10.0.2.16 -H 'Host: fizfak.ru'
     curl 10.0.2.16 -H 'Host: etis.com'
     ```

## Пример работы

* Проверка через curl:
<img width="818" height="546" alt="image" src="https://github.com/user-attachments/assets/09a4dd12-171e-4d29-b864-4a6911f9a708" />

* Проверка через браузер: 
** Необходимо поправить файл hosts на локальной машине
<img width="757" height="277" alt="image" src="https://github.com/user-attachments/assets/62d2ee9e-4ceb-4d3c-b05d-89d098de3137" />
