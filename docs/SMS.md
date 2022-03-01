# Funcionalidades disponíveis para os serviços SMS

**Nota:** Por ser extremamente similar com o modo SMM nas chamadas de API, o modo SMS não terá exemplos com o Insomnia. Apenas em Python. Caso não tenha muita familiariade, com a linguagem, consulte a seção SMM pois temos exemplos com o Insomnia, então basta replicar os métodos para esta seção de forma análoga.


A url base para os serviços SMM é: **https://www.fruiitbots.com/api/v1/sms**

## Obtenha a lista de serviços disponíveis

### Exemplo em Python
```{.py3 linenums="1"} 
from httpx import post

data = {
    "key": "YOUR_API_KEY",
    "action": "services"
}

response = post(
    url='https://www.fruiitbots.com/api/v1/sms',
    data=data
)

print(response.json())
```


### Exemplo de resposta 

```
{
	"country_1": {
		"service_name_1": [
			{
				"service_id": 1,
				"option_name": "Opção 1",
				"rate": 2.3,
				"currency": "BRL",
			},
			{
				"service_id": 2,
				"option_name": "Opção 2",
				"rate": 3.3,
				"currency": "BRL",
			},
			
			
		],
		
		"service_name_2": [
			{
				"service_id": 3,
				"option_name": "Opção 1",
				"rate": 2.3,
				"currency": "BRL",
			},
			{
				"service_id": 4,
				"option_name": "Opção 2",
				"rate": 3.3,
				"currency": "BRL",
			},
		]
	},
	"country_2": {
	
		"service_name_1": [
			{
				"service_id": 5,
				"option_name": "Opção 1",
				"rate": 2.3,
				"currency": "BRL",
			},
			
			{
				"service_id": 6,
				"option_name": "Opção 2",
				"rate": 2.3,
				"currency": "BRL",
			},
		
		],
		"service_name_2": [
			{
				"service_id": 7,
				"option_name": "Opção 1",
				"rate": 2.3,
				"currency": "BRL",
			},
			
			{
				"service_id": 8,
				"option_name": "Opção 2",
				"rate": 2.3,
				"currency": "BRL",
			},
		
		],
		"service_name_3": [
			{
				"service_id": 9,
				"option_name": "Opção 1",
				"rate": 2.3,
				"currency": "BRL",
			},
			
			{
				"service_id": 10,
				"option_name": "Opção 2",
				"rate": 2.3,
				"currency": "BRL",
			},
		
		],
	
	}


}

   
```

## Obtenha a lista de países disponíveis

### Exemplo em Python
```{.py3 linenums="1"} 
from httpx import post

data = {
    "key": "YOUR_API_KEY",
    "action": "countries"
}

response = post(
    url='https://www.fruiitbots.com/api/v1/sms',
    data=data
)

print(response.json())
```
### Exemplo de resposta
```
[
    "russia",
    "brazil",
    "uruguay",
    "usa",
    "burundi",
] 
   
```


## Status padrões das requisições

**Nota:** Eventualmente, novos retornos podem ser incluídos no sistema e não necessariamente serão registrados aqui nesta documentação, de forma breve. Portanto, realize o catalogo de novos retornos da API em seu backend.

`sms_received` - O número recebeu um SMS (logo impossibilitado de ser cancelado o pedido)

`canceled` - O pedido foi cancelado

`waiting_sms` - Aguardando SMS

`finished_by_time` - Pedido finalizado automaticamente após expirar o prazo de utilização.

`finished_by_user` - Pedido finalizado pelo usuário.

`trash` - Número descartado pelo sistema, indisponível para compra ou re-compra.

`service_not_found` - Serviço Não encontrado

`order_not_found` - Pedido Não encontrado

`invalid_api_key` - Chave de API inválida

`invalid_param` - Parâmetro de requisição inválido

`missing_data` - Faltou enviar dados no corpo da requisição.

`unauthorized` - Sua chave não tem autoridade para realizar o tipo de ação requerido.


## Comprar número

### Exemplo em Python
```{.py3 linenums="1"} 
from httpx import post

data = {
    "key": "YOUR_API_KEY",
    "action": "buy",
	"service": 11,
}

response = post(
    url='https://www.fruiitbots.com/api/v1/sms',
    data=data
)

print(response.json())
```

### Exemplo de resposta 
Atenção para `created_at` e `expiration` que por sua vez são retornados em formato de [timestamp](https://hkotsubo.github.io/blog/2019-05-02/o-que-e-timestamp) de forma que fique fácil para manipular em diferentes linguagens. Você pode, também, converter manualmente em sites como [timestamp.online](https://timestamp.online/) e o [freeformatter](https://www.freeformatter.com/epoch-timestamp-to-date-converter.html).
```
{
    "status": "waiting_sms",
    "order_id": 20339,
    "telephone_number": "+5573983184911",
    "country": "brazil",
    "created_at": 1611524701,
    "expiration": 1611525301,
    "rate": 2.3,
    "currency": "BRL" 
}  
   
```
## Cancelar pedido
**Nota:** Só é possível cancelar pedidos apenas se não tiver chegado alguma mensagem e o tempo de expiração ainda não tenha se esgotado.

### Exemplo em Python
```{.py3 linenums="1"} 
from httpx import post

data = {
    "key": "YOUR_API_KEY",
    "action": "cancel",
	"order_id": 1,
}

response = post(
    url='https://www.fruiitbots.com/api/v1/sms',
    data=data
)

print(response.json())
```

### Exemplo de resposta 

```
{
    "status": "canceled",
}  
   
```

## Finalizar pedido
**Nota:** Só finalize o pedido se tiver recebido o SMS, caso não tiver recebido e o pedido for finalizado ele será descontado do seu saldo SEM REEMBOLSO.

### Exemplo em Python
```{.py3 linenums="1"} 
from httpx import post

data = {
    "key": "YOUR_API_KEY",
    "action": "finish",
	"order_id": 1,
}

response = post(
    url='https://www.fruiitbots.com/api/v1/sms',
    data=data
)

print(response.json())
```

### Exemplo de resposta 

```
{
    "status": "finished_by_user",
}  
   
```


## Ler caixa de mensagens

### Exemplo em Python
```{.py3 linenums="1"} 
from httpx import post

data = {
    "key": "YOUR_API_KEY",
	"order_id": 1,
	"action": "read",
}

response = post(
    url='https://www.fruiitbots.com/api/v1/sms',
    data=data
)

print(response.json())
```

### Exemplo de resposta 

```
{
    "telephone_number": "+5573983184911",
    "status": "sms_received",
    "inbox": [
        {
            "body_message": "319768 is your Instagram code",
            "sms_code": "319768"
        },
        {
            "body_message": "035462 is your Instagram code",
            "sms_code": "035462"
        }
    ]
 
}  
   
```

## Readquirir um número utilizado anteriormente

**Nota 1:** números restaurados possuem suas caixas esvaziadas, logo não é possível recuperar mensagens antigas.


### Exemplo em Python
```{.py3 linenums="1"} 
from httpx import post

data = {
    "key": "YOUR_API_KEY",
    "action": "rebuy"
	"order_id": 20339,
}

response = post(
    url='https://www.fruiitbots.com/api/v1/sms',
    data=data
)

print(response.json())
```

### Exemplo de resposta 

```
{
    "status": "waiting_sms",
    "order_id": 20340,
    "telephone_number": "+5573983184911",
    "country": "brazil",
    "created_at": 1609470000,
    "expiration": 1609470600,
    "rate": 2.3,
    "currency" : "BRL" 
}
```
  
   
