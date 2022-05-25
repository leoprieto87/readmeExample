# OrderForm Mediator

> Esta aplicação foi desenvolvida como parte do MVP do projeto de OrderForm Mediator da Whirlpool. E tem a finalidade de validar o acesso de usuários em regras específicas do Vendor na VTEX.


## 💻 Pré-requisitos

Antes de começar, verifique se você atende aos seguintes requisitos:

* Node 14.18.0 ou posterior;

# Iniciando na VTEX

Certificar que o app está habilitado para ser instalado na conta, conforme esse form:
https://docs.google.com/forms/d/e/1FAIpQLSfhuhFxvezMhPEoFlN9yFEkUifGQlGP4HmJQgx6GP32WZchBw/viewform

## Instalando o APP

Primeiro verifique se está corretamente logado em sua workspace:
```
$ vtex login {account-name}
```

Para instalar a aplicação é necessário o comando:
``` node
$ vtex install {appVendor}.{appName}
```
Após a instalação é preciso realizar o comando 'link' da vtex:
``` node
$ vtex link
```

## ☕ Utilizando 

Para utilizar o serviço  necessário acessar o endpoit: 

``` javascript
https://{workspace}--{account}.myvtex.com/orderformmediator/validator

```

## Headers

``` javascript
 contentJson: {
    'Content-Type': 'application/json'
},
acceptJson: {
    accept: 'application/json'
},
VtexIdclientAutCookie: {authToken}
```     
## Body

``` json
{
	"rules": [
		{
			"id": 1,
			"externalTrigger": false
		}
	],
	"orderForm": {"CONTEÚDO DO ORDERFORM"}
}
```
<p>
    <strong>RULES:</strong> Refere-se ao id da regra que deseja consultar.
    Nesse momento, o ID 1 irá se referir a regra de ‘rejection_list’ cadastrada nessa entidade, pra isso, criaremos uma entidade no masterData chamada rules.
</p>
<p>
    <strong>ORDERFORM:</strong> Dados referente ao pedido que está sendo feito, contendo email de usuários, informações de produtos, etc.
</p>



## Retornos e tratamentos

``` javascript
response[ 
 {
 ruleId:1, 
 status: 'aproved', 
 aditionalInfo: '' 
 }
] 
```

<table>

<strong>Sucesso</strong>

``` javascript
ruleId: this.id,
statusDescription: "denied",
aditionalInfo: dataFromRejectionEntity.aditionalInfo || aditionalInfoDenie
```

<strong>Negado</strong>

``` javascript
ruleId: this.id,
statusDescription: "denied",
aditionalInfo: dataFromRejectionEntity.aditionalInfo || aditionalInfoDenie

```
<strong>Negado</strong>

``` javascript
ruleId: this.id,
statusDescription: "denied",
aditionalInfo: dataFromRejectionEntity.aditionalInfo || aditionalInfoDenie
```
<p>
    O campo <strong>aditionalInfo</strong> é referente a alguma informação que o usuário possa conter, sendo preenchido no momento do cadastro na tabela de <strong>rejection_list</strong>, caso não tenha, retornaremos um objeto vazio.
</p>

## POSTMAN

