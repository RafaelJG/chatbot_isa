# Instruções para implantação do chatbot ISA


Para implantação do chatbot ISA, são necessários os seguintes componentes:

## Banco de dados

O modelo do banco de dados utilizado é o seguinte:

![chatbot_isa_white2](https://user-images.githubusercontent.com/4268510/204142994-372be35e-67dc-42e5-8d67-7ed50c348224.png)

O script de criação do banco está disponível [aqui](https://github.com/RafaelJG/chatbot_isa/blob/main/chatbot_isa.sql)
## Webhook
O Código desse componente está disponível [aqui](https://github.com/RafaelJG/isa-reativo-webhook).


Esse serviço pode ser executado por container docker com os seguintes comandos:

`docker build -t docker_isa_webhook:latest .  `

`docker stop docker_isa_webhook `

`docker rm docker_isa_webhook `

`docker run --network=chatbot --env FLASK_ENV=Dev -t -d -p $PORT:5000 --restart=always --name=docker_isa_webhook docker_isa_webhook:latest`

`docker logs --follow docker_isa_webhook `

**IMPORTANTE**

Para o funcionamento correto desse serviço, é necessária a configuração de DNS e Webserver com SSL para a $PORT desejada para o serviço. Isso é necessário para sua integração com o Dialogflow.

Os detalhes de conexão com banco de dados, endereços e usuários podem ser definidos alterando o arquivo [settings.py](https://github.com/RafaelJG/isa-reativo-webhook/blob/main/config/settings.py)



## Messages


O Código desse componente está disponível [aqui](https://github.com/RafaelJG/isa-messages).


Esse serviço pode ser executado por container docker com os seguintes comandos:

`docker build -t docker_isa_messages:latest . `
`docker stop docker_isa_messages `
`docker rm docker_isa_messages`
`docker run --network=chatbot --env FLASK_ENV=Dev -t -d -p $PORT:5000 --restart=always --name=docker_isa_messages docker_isa_messages:latest`
`docker logs --follow docker_isa_messages`

**IMPORTANTE**

Para o funcionamento correto desse serviço, é necessária a configuração de DNS e Webserver com SSL para a $PORT desejada para o serviço. Isso é necessário para sua integração com os outros serviços do chatbot e disponibilizar o canal de comunicação.

Os detalhes de conexão com banco de dados, endereços e usuários podem ser definidos alterando o arquivo [settings.py](https://github.com/RafaelJG/isa-messages/blob/master/config/settings.py)

## Integração com Telegram

O Código desse componente está disponível [aqui](https://github.com/RafaelJG/isa-telegram).


Esse serviço pode ser executado por container docker com os seguintes comandos:

`docker build -t isa_telegram:latest . `
`docker stop isa_telegram`
`docker rm isa_telegram`
`docker run  --env FLASK_ENV=Dev -t -d -p 9003:5000 --restart=always --name=isa_telegram isa_telegram:latest`
`docker logs --follow isa_telegram`

Esse serviço não necessita de configuração para servidor Web ou DNS, mas necessita ser integrado com um Bot do telegram utilizando uma chave de API. Mais detalhes de como realizar essa integração podem ser acessados [aqui](https://telegram.me/BotFather)

## Plugin Web

O Código desse componente está disponível [aqui](https://github.com/RafaelJG/webchat-plugin-isa).


Esse serviço pode ser executado por container docker com os seguintes comandos:

`docker build -t webchat_isa:latest . `
`docker stop webchat_isa`
`docker rm webchat_isa`
`docker run  --network=chatbot --env FLASK_ENV=Dev -t -d -p 9005:8080 --restart=always --name=webchat_isa webchat_isa:latest `
`docker logs --follow webchat_isa`

**IMPORTANTE**

Para o funcionamento correto desse serviço, é necessária a configuração de DNS e Webserver com SSL para a $PORT desejada para o serviço. Isso é necessário para sua integração com os outros serviços do chatbot e disponibilizar o canal de comunicação. Também é necessária a configuração para conexão com os serviços do messages.

## Dialogflow

O agente do dialogflow utilizado no chatbot ISA pode ser importado utilizando o arquivo disponibilizado [aqui](https://github.com/RafaelJG/chatbot_isa/blob/main/ISA_DEV.zip).
É importante atentar para os detalhes de endereços de serviços de intergração e as chaves de acesso utilizadas para a configuração e conexão do serviço do dialogflow com os outros componentes do chatbot.


## Qualquer dúvida em relação a implantação, documentação, ou configuração dos serviços, ou caso necessite de acesso em algum repositório privado, entre em contato no e-mail rafael.jg94@gmail.com 
