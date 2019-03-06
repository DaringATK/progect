 ## переменные 

	### переменные crawler:
	- ENV MONGO mongodb
	- ENV MONGO_PORT 27017
	- ENV RMQ_HOST rabbitmq
	- ENV RMQ_QUEUE derp
	- ENV RMQ_USERNAME derp
	- ENV RMQ_PASSWORD derp
	- ENV CHECK_INTERVAL 60
	- ENV EXCLUDE_URLS '.*github.com'

	### переменные UI:
	- ENV MONGO mongodb
	- ENV MONGO_PORT 27017

## Старт приложения с помощью  docker
	### network	
docker run -it -d --rm --network project --name crawler --hostname crawler crawler:0.0.1

	### mongodb
docker run -it --rm -d --hostname mongodb  --name mongodb --network project -p 27017:27017 mongo:3.2

	### RABBITMQ
docker run -it -d -p 15672:15672 -p 5672:5672 -e RABBITMQ_DEFAULT_USER=admin -e RABBITMQ_DEFAULT_PASS=admin --hostname rabbitmq --name rabbitmq --network project rabbitmq:3-management

	### ui
docker run -it -d --rm -p 8000:8000 --network project --hostname ui --name ui ui:0.0.1

 ## старт приложения в ручном режиме
1) Поднимаем кластер куба 
• Тип машины - небольшая машина (1,7 ГБ) (для экономии ресурсов)
• Размер - 2
• Базовая аутентификация - отключена
• Устаревшие права доступа - отключено
• Панель управления Kubernetes - отключено
• создать правило VPS:
    Название - произвольно, но понятно
    • Целевые экземпляры - все экземпляры в сети
    • Диапазоны IP-адресов источников  - 0.0.0.0/0
    Протоколы и порты - Указанные протоколы и порты
    tcp:30000-32767
2) создание диска для монги 
gcloud compute disks create --size=25GB --zone=us-central1-a crawler-mongo-disk
#Диск для production
gcloud compute disks create --size=25GB --zone=us-central1-a mongo-disk-production
#Диск для staging
gcloud compute disks create --size=25GB --zone=us-central1-a mongo-disk-staging

3) kubectl apply -f mongo-volume.yml
4) kubectl apply -f mongo-deployment.yml
5) kubectl apply -f mongo-service.yml
6) kubectl apply -f rabbitmq-deployment.yml
7) kubectl apply -f rabbitmq-service.yml
8) kubectl apply -f rabbitmq-ingress.yml
9) kubectl apply -f ui-crawler-deployment.yml
10) kubectl apply -f ui-crawler-service.yml
11) kubectl apply -f crawler-deployment.yml
12) kubectl apply -f crawler-service.yml

старт мониторинга
kubectl apply -f tiller.yml
helm init --service-account tiller
kubectl get pods -n kube-system --selector app=helm
helm install stable/nginx-ingress --name nginx
kubectl get svc
sudo nano /etc/hosts (/etc/hosts<35.239.145.155 reddit reddit-prometheus reddit-grafana reddit-non-prod production reddit-kibana staging prod)
helm upgrade prom . -f custom_values.yml --install
helm upgrade --install grafana stable/grafana --set "adminPassword=admin" --set "service.type=NodePort" --set "ingress.enabled=true" --set "ingress.hosts={reddit-grafana}"

  ### Start Gitlab
 * cd kubernetis/Chart/gitlab-omnibus
 * helm install --name gitlab . -f values.yaml

 ### Узнать ip gitlab
 * kubectl get service -n nginx-ingress nginx

В hosts файл добавить 
 * 35.197.206.71 gitlab-gitlab production dyavolmgn-ui-feature-3 staging

 ## Gitlab-ci
В gitlab созданно три проекта:
 - ui
 - crawler
 - reddit-deploy

 ## Проект ui
В проекте ui хранится код. Настроен ci, в котором билдится docker образ. И проходят тесты. Для разработчиков имеется имеется возможность review, и развертывание тестового контура.
В файле VERSION выставляется версия образа, по умолчанию latest.

 ## Проект crawler
В проекте crawler хранится код. Настроен ci, в котором билдится docker образ. И проходят тесты. Для разработчиков имеется имеется возможность review, и развертывание тестового контура.
В файле VERSION выставляется версия образа, по умолчанию latest.

 ## Проект reddit-deploy
В проекте reddit-deploy необходим для запуска стендов staging и production. Деплой staging автоматический, production мануальный.
При деплое устанавливается/обновляется:
	- ui
	- crawler
	- rebbitmq
	- mongodb
Развертывание происходит в кубернетис, при помощи helm-2.12.2.


	### Доступ:
	* Staging
	- http://staging/

	* Production
	- http://production/
