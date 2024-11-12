![изображение](https://github.com/user-attachments/assets/0c1b9825-4a30-4446-9989-58e6f76cb26e)# Kascheev
# Docker

![image](https://github.com/kascheev-1/Kascheev/blob/main/cat-computer.gif)

## Быстрый старт

Перед началой установки, нужно установить Linux Oracle на VirtualBox, для этого нужно:

Иметь образ Linux
Выделить 2+ ядер.
Выделать 4096+ МБ оперативы.
При установки операционной системы, нужно будет выбрать английский язык.

Далее переходим к установке docker с использованием grafana, вводим следующий набор команд:

1. `sudo yum install wget`

• устанавливает утилиту wget на вашу систему

![изображение](https://github.com/user-attachments/assets/4572afee-f959-4372-8e4a-1ad9d0f968c1)


2. `sudo wget -P /etc/yum.repos.d/ https://download.docker.com/linux/centos/docker-ce.repo`

• Скачиваем файл репозитория

![изображение](https://github.com/user-attachments/assets/643caac8-9e46-415b-880f-0b8da7446a1c)

3. `sudo yum install docker-ce docker-ce-cli containerd.io`

• Устанавливаем docker

![изображение](https://github.com/user-attachments/assets/517659df-7e4d-4f74-b014-4d02ef1142f1)

4. `sudo systemctl enable docker --now`

• Запускаем его и разрешаем автозапуск

5. `sudo yum install curl`

• Для этого сначала убедимся в наличие пакета curl

6. `COMVER=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep 'tag_name' | cut -d\" -f4)`

• Объявление переменной COMVER, полученной в результате curl запроса, хранящей в себе номер последней
версии Docker Compose

![изображение](https://github.com/user-attachments/assets/d1c0406f-6879-4638-90c8-87787f7ea1ac)

7. `sudo curl -L "https://github.com/docker/compose/releases/download/$COMVER/docker-compose-$(uname -s)-$(uname -m)" -o /usr/bin/docker-compose`                        

• Теперь скачиваем скрипт docker-compose последней версии, используя объявленную ранее переменную и помещаем его в каталог /usr/bin

![изображение](https://github.com/user-attachments/assets/5fad1638-b5db-42e5-a3bc-89a1abcdd806)

8. `sudo chmod +x /usr/bin/docker-compose`

• Предоставление прав на выполнение файла docker-compose.

9. `docker-compose --version`

• Проверка установленной версии Docker Compose.

![изображение](https://github.com/user-attachments/assets/8495485c-0c15-4590-b556-7c265306b264)

• Можно скачать git прямо из командной строки прописав Y

10. `git clone https://github.com/skl256/grafana_stack_for_docker.git`

• выдаст ошибку и предложит скачать git, согласиться и продолжить

![изображение](https://github.com/user-attachments/assets/f946153f-376b-41c0-b0fe-f70ae9c74aff)

11. `cd grafana_stack_for_docker`
    
• переход в папку

12.`sudo mkdir -p /mnt/common_volume/swarm/grafana/config`

• команда создаёт полный путь /mnt/common_volume/swarm/grafana/config, включая все необходимые промежуточные каталоги, если они ещё не существуют.

13. `sudo mkdir -p /mnt/common_volume/grafana/{grafana-config,grafana-data,prometheus-data}`

• команда создаёт структуру каталогов для Grafana и связанных с ней компонентов, если они ещё не существуют.

14. `sudo chown -R $(id -u):$(id -g) {/mnt/common_volume/swarm/grafana/config,/mnt/common_volume/grafana}`

• все файлы и каталоги в указанных директориях будут переданы в собственность текущему пользователю и его группе

15.` touch /mnt/common_volume/grafana/grafana-config/grafana.ini`

• файл grafana.ini уже существует, команда обновит его временные метки (время последнего доступа и изменения). Если файл не существует, команда создаст новый пустой файл с указанным именем по указанному пути.

16. `cp config/* /mnt/common_volume/swarm/grafana/config/`

• команда копирует все файлы и подкаталоги из директории config в директорию /mnt/common_volume/swarm/grafana/config/

17. `mv grafana.yaml docker-compose.yaml `

• команда переименовывает файл grafana.yaml в docker-compose.yaml. Ничего не покажет, но можно проверить при помощи команды ls

18.` sudo docker compose up -d`

• команда создает и запускает контейнеры в фоновом режиме, используя конфигурацию из файла docker-compose.yml, с правами суперпользователя.

![изображение](https://github.com/user-attachments/assets/af7d90e0-e394-4193-9e13-164bfd63a137)

![изображение](https://github.com/user-attachments/assets/ce4bac0a-e6ff-41ca-b93e-f78b7eb34fbc)

19.` sudo vi docker-compose.yaml`

•команда открывает файл docker-compose.yaml в текстовом редакторе vi с правами суперпользователя, что позволяет вам редактировать его содержимое.

• Нас перекинет в текстовый редактор

• Что-бы что-то изменить в тесковом редакторе нажмите insert на клавиатуре

• Что бы сохранить что-то в этом документе нажимаем Esc пишем :wq! В этом текставом редакторе мы должны поставить node-exporter после services

20. `sudo vi prometheus.yaml `

• команда открывает файл prometheus.yaml в текстовом редакторе vi с правами суперпользователя.

• /mnt/common_volume/swarm/grafana/config/prometheus.yaml - исправить targets: на exporter:9100,

## Grafana

* переходим на сайт `localhost:3000`
    * User & Password GRAFANA: `admin`
    * Код графаны: `3000`
    * Код прометеуса: `http://prometheus:9090`
* в меню выбираем вкладку Dashboards и создаем Dashboard
    * ждем кнопку +Add visualization, а после "Configure a new data source"
    * выбираем Prometheus
    * Connection
    * `http://prometheus:9090`
* Authentication
    * Basic authentication
        * User: `admin`
        * Password: `admin`
        * Нажимаем на Save & test и должно показывать зелёную галочку
* в меню выбираем вкладку Dashboards и создаем Dashboard
    * ждем кнопку "Import dashboard"
    * Find and import dashboards for common applications at grafana.com/dashboards: 1860 //ждем кнопку Load
    * Select Prometheus ждем кнопку "Import"

![изображение](https://github.com/user-attachments/assets/e576a1be-d4c3-4755-bc16-d5855d517ec0)

## VictoriaMetrics

Для начала изменим docker-compose.yaml

1. `cd grafana_stack_for_docker`

• команда cd grafana_stack_for_docker изменяет текущий рабочий каталог на каталог grafana_stack_for_docker.

2. `sudo vi docker-compose.yaml`

• команда sudo открывает файл docker-compose.yaml в редакторе vi с правами суперпользователя.

![image](https://github.com/user-attachments/assets/1a957374-26c9-4b6f-96c1-0741d8d1b745)

В самом текстовом редакторе после prometheus вставляем

![image](https://github.com/user-attachments/assets/b25ebd84-0173-4e2c-9fe5-c94b7c290a37)

Захом в connection
там где мы писали http//:prometheus:9090 пишем http:victoriametrics:9090 И заменяем имя из "Prometheus-2" в "Vika"
нажимаем на dashboards add visualition выбираем "Vika"
снизу меняем на "code"
Переходим в терминал и пишем

3. `echo -e "# TYPE OILCOINT_metric1 gauge\nOILCOINT_metric1 0" | curl --data-binary @- http://localhost:8428/api/v1/import/prometheus  `

• команда отправляет бинарные данные (например, метрики в формате Prometheus) на локальный сервер, который слушает на порту 8428.

4. `curl -G 'http://localhost:8428/api/v1/query' --data-urlencode 'query=OILCOINT_metric1'`

• команда делает запрос к API для получения данных по метрике OILCOINT_metric1

• команда выводит текст, который может быть использован для определения метрики в формате, совместимом с Prometheus

• команда выводит информацию о типе и значении этой метрики в формате, который может быть использован системой мониторинга Prometheus.

Значение 0 меняем на любое другое

Копируем переменную OILCOINT_metric1 и вставляем в query

Нажимаем run

![image](https://github.com/user-attachments/assets/e6d1a7a9-3bc7-43da-a19c-c3304a535cb6)

![image](https://github.com/user-attachments/assets/b1fc3cd1-9abf-451d-9779-60872a566a98)

Копируем переменную OILCOINT_metric1 и вставляем в code

![image](https://github.com/user-attachments/assets/739a9d3f-163b-40df-bc74-f5b76613a5f5)




  


 
