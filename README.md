###mongodb
docker run -it --rm -d --hostname mongodb  --name mongodb --network project -p 27017:27017 mongo:3.2

###RABBITMQ
docker run -it -d -p 15672:15672 -p 5672:5672 -e RABBITMQ_DEFAULT_USER=admin -e RABBITMQ_DEFAULT_PASS=admin --hostname rabbitmq --name rabbitmq --network project rabbitmq:3-management
