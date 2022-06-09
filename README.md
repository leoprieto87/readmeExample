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

## Headers

``` javascript
 contentJson: {
    'Content-Type': 'application/json'
},
acceptJson: {
    accept: 'application/json'
}
```  

## ☕ Endpoint de validação 

Para utilizar o serviço de validação acesse o endpoit: 

``` javascript
https://{workspace}--{account}.myvtex.com/orderformmediator/validator

```
<p>
    <strong>GET</strong> <br />
    <strong>body:</strong> <br />
</p>

``` json
{
	"rules": [
		{
			"id": "c1574e4c-e732-11ec-835d-16d255cd11e9",
			"externalTrigger": false
		}
	],
	"orderForm": {
		"clientProfileData": {
				"email": "testando2@testando.com.br"
		}
	}
}
```

<p>
    <strong>RULES:</strong> Refere-se ao id da regra que deseja consultar.
    Nesse momento, o ID 1 irá se referir a regra de ‘rejection_list’ cadastrada nessa entidade, pra isso, criaremos uma entidade no masterData chamada rules.
</p>
<p>
    <strong>ORDERFORM:</strong> Dados referente ao pedido que está sendo feito, contendo email de usuários, informações de produtos, etc.
    No exemplo acima utilizamos somente o campo E-mail, porém pode ser passado o OrderForm completo.
</p>

## Retornos da validação

| Retornos |  Body  | Info |
| ----- | -------- | ------ |
| Aprovado  | `'aproved'` | "Usuário foi aceito na regra solicitada" |
| Negado   |  `'denied'`  |   "O usuário está na lista de rejeição e não pode ser aprovado." |
| Indefinido   | `'undefined'` | "RejectionList validation error" <br /> "Rule id not found" <br /> "Input validation failed. Not valid rule pattern." <br /> Isso ocorre pois o e-mail ou o id da regra solicitada pode estar com erro ou não foi encontrada.  |
| Requisisão inválida   | `'bad request'` | A regra pôde ser instanciada mas houve uma falha no seu processo interno de execução |


<strong>Aproved</strong>

``` javascript
ruleId: this.id,
statusDescription: "approved",
aditionalInfo: ""
```

<strong>Denied</strong>

``` javascript
ruleId: this.id,
statusDescription: "denied",
aditionalInfo: dataFromRejectionEntity.aditionalInfo ||"O usuário está na lista de rejeição e não pode ser aprovado."

```

<strong>Undefined</strong>

``` javascript
ruleId: this.id,
statusDescription: "undefined",
aditionalInfo: "RejectionList validation error"
```

<strong>Bad Request</strong>

``` javascript
ruleId: this.id,
statusDescription: "bad request",
aditionalInfo: ""
```
<p>
    O campo <strong>aditionalInfo</strong> é referente a alguma informação que o usuário possa conter, sendo preenchido no momento do cadastro na tabela de <strong>rejection_list</strong>, caso não tenha, retornaremos um objeto vazio.
</p>


## ☕ Endpoint de Rules 

Para utilizar o serviço de Rules acesse o endpoit: 

``` javascript
https://{workspace}--{account}.myvtex.com/orderformmediator/rules
```
<p>
    <strong>HEADERS</strong> <br />
</p>

``` json
{
    "x-vtex-api-appkey": "[your-vtex-api-appkey]",
    "x-vtex-api-apptoken": "[your-vtex-api-apptoken]"
}
```

<p>
    <strong>GET</strong> <br />
    Para utilizar o método GET não é necessário um Body
</p>

![GET](https://whirlpoolappstore.vtexassets.com/arquivos/Rules-GET-Body.png)

<p>
    <strong>Response</strong> <br />
</p>

``` json
[
	{
		"id": "c1f84c6b-e732-11ec-835d-12de90dd8255",
		"name": "rejectionList_copy",
		"endpoint": null,
		"active": false
	}
]
```
![GET](https://whirlpoolappstore.vtexassets.com/arquivos/Rules-GET-Response.png)
<!-- PRINT GET -->

<p>
    <strong>POST</strong> <br />
    <strong>Body:</strong> <br />
</p>

``` json
	{
		"name": "Nova Regra",
		"endpoint": null,
		"active": true
	}
```
![POST](https://whirlpoolappstore.vtexassets.com/arquivos/Rules-POST-Body.png)

<p>
    <strong>Response</strong> <br />
</p>

``` json
[
	{
		"id": "c1f84c6b-e732-11ec-835d-12de90dd8255",
		"name": "rejectionList_copy",
		"endpoint": null,
		"active": false
	}
]
```
![POST](https://whirlpoolappstore.vtexassets.com/arquivos/Rules-POST-Response.png)
<!-- PRINT POST -->

<p>
    <strong>PATCH</strong> <br />
    <strong>Body:</strong> <br />
</p>

``` json
    {
        "id": "e2113265-e81f-11ec-835d-162544423325",
        "name": "Nova Regra Atualizada",
        "endpoint": null,
        "active": false
	}
```
![PATCH](https://whirlpoolappstore.vtexassets.com/arquivos/Rules-PATCH-Body.png)

<p>
    <strong>Response</strong> <br />
</p>

![PATCH](https://whirlpoolappstore.vtexassets.com/arquivos/Rules-PATCH-Response.png)
<!-- PRINT PATCH -->

<p>
    <strong>DEL</strong> <br />
    <strong>Body:</strong> <br />
</p>

``` json
    	{
		"id": "36db21b3-e81f-11ec-835d-0ee0eccb1eb3"
	}
```
![DELETE](https://whirlpoolappstore.vtexassets.com/arquivos/Rules-DEL-Body.png)

<p>
    <strong>Response</strong> <br />
</p>

![DELETE](https://whirlpoolappstore.vtexassets.com/arquivos/Rules-DEL-Response.png)
<!-- PRINT DEL -->

## Utilizando a aplicação

Para utilizar a aplicação via software (Postman, Insomnia, etc.) 
Import o JSON abaixo para o seu programa de preferência

``` json
{
    "_type": "export",
    "__export_format": 4,
    "__export_date": "2022-06-09T18:42:07.510Z",
    "__export_source": "insomnia.desktop.app:v2022.3.0",
    "resources": [{
        "_id": "req_f02c5588f5424b4eb75180dbe2cb1c02",
        "parentId": "fld_655ccdca740c45e19be3f8cd984b21d1",
        "modified": 1654716480659,
        "created": 1652798963138,
        "url": "{{ _.base_ulr }}/user-identification-services",
        "name": "user-identification-services",
        "description": "",
        "method": "GET",
        "body": {},
        "parameters": [],
        "headers": [{
            "id": "pair_ced4177cb78e4765b447824b0dc70047",
            "name": "x-vtex-api-appkey",
            "value": "vtexappkey-compracertamkpqa-BGKNMC",
            "description": ""
        }, {
            "id": "pair_f591547080524c36bfed9f80282c016e",
            "name": "x-vtex-api-apptoken",
            "value": "KAZRKRCQFGFIEUDBOZGISLZBVTASEMVUQHGMXNRMINENGQGHFMZARRFZBYWQRKMTLKSXNREAITZFGLUENSNRNVDZBHKYRAVYAKCWYJUNIYRXRASQDSDYWUGQGSVFGFJH",
            "description": ""
        }],
        "authentication": {},
        "metaSortKey": -1650231415249.5,
        "isPrivate": false,
        "settingStoreCookies": true,
        "settingSendCookies": true,
        "settingDisableRenderRequestBody": false,
        "settingEncodeUrl": true,
        "settingRebuildPath": true,
        "settingFollowRedirects": "global",
        "_type": "request"
    }, {
        "_id": "fld_655ccdca740c45e19be3f8cd984b21d1",
        "parentId": "fld_5a85f29018ba4e56aa90bb78c2020f32",
        "modified": 1654716299619,
        "created": 1652798948032,
        "name": "user-identification-services",
        "description": "",
        "environment": {},
        "environmentPropertyOrder": null,
        "metaSortKey": -1652798948032,
        "_type": "request_group"
    }, {
        "_id": "fld_5a85f29018ba4e56aa90bb78c2020f32",
        "parentId": "fld_07527411fe59462183d4cf741daa0ba7",
        "modified": 1654718326386,
        "created": 1647963746777,
        "name": "orderformmediator",
        "description": "",
        "environment": {
            "base_ulr": "https://whirlpoolappstore.myvtex.com/orderformmediator"
        },
        "environmentPropertyOrder": {
            "&": ["base_ulr"]
        },
        "metaSortKey": -1647963746777,
        "_type": "request_group"
    }, {
        "_id": "fld_07527411fe59462183d4cf741daa0ba7",
        "parentId": "wrk_12c2b5256773445eb62fa3098896fc7e",
        "modified": 1654714294956,
        "created": 1647963708923,
        "name": "Whilrpool",
        "description": "",
        "environment": {},
        "environmentPropertyOrder": {},
        "metaSortKey": -1647963708923,
        "_type": "request_group"
    }, {
        "_id": "wrk_12c2b5256773445eb62fa3098896fc7e",
        "parentId": null,
        "modified": 1654719144821,
        "created": 1628102005300,
        "name": "ORDERFORM",
        "description": "",
        "scope": "collection",
        "_type": "workspace"
    }, {
        "_id": "req_7eaaa3af3beb4c638a13d87d7e7c8e0c",
        "parentId": "fld_655ccdca740c45e19be3f8cd984b21d1",
        "modified": 1654799448634,
        "created": 1653052283379,
        "url": "{{ _.base_ulr }}/user-identification-services",
        "name": "user-identification-services",
        "description": "",
        "method": "PATCH",
        "body": {
            "mimeType": "application/json",
            "text": "{\n\t\"id\": \"0b207a17-e762-11ec-835d-16a7ac2ae94b\",\n\t\"serviceName\": \"service-4 update\",\n\t\"connectionData\": {\"x-api-key\":\"abc\"},\n\t\"active\": true\n}"
        },
        "parameters": [],
        "headers": [{
            "id": "pair_ced4177cb78e4765b447824b0dc70047",
            "name": "x-vtex-api-appkey",
            "value": "vtexappkey-compracertamkpqa-BGKNMC",
            "description": ""
        }, {
            "id": "pair_f591547080524c36bfed9f80282c016e",
            "name": "x-vtex-api-apptoken",
            "value": "KAZRKRCQFGFIEUDBOZGISLZBVTASEMVUQHGMXNRMINENGQGHFMZARRFZBYWQRKMTLKSXNREAITZFGLUENSNRNVDZBHKYRAVYAKCWYJUNIYRXRASQDSDYWUGQGSVFGFJH",
            "description": ""
        }, {
            "name": "Content-Type",
            "value": "application/json",
            "id": "pair_e6f9a609f26c4610beb6b8c752f555d9"
        }],
        "authentication": {},
        "metaSortKey": -1650231415240.125,
        "isPrivate": false,
        "settingStoreCookies": true,
        "settingSendCookies": true,
        "settingDisableRenderRequestBody": false,
        "settingEncodeUrl": true,
        "settingRebuildPath": true,
        "settingFollowRedirects": "global",
        "_type": "request"
    }, {
        "_id": "req_5e8054c4d3dd4b649181625f803843f8",
        "parentId": "fld_655ccdca740c45e19be3f8cd984b21d1",
        "modified": 1654799447943,
        "created": 1653077123284,
        "url": "{{ _.base_ulr }}/user-identification-services",
        "name": "user-identification-services",
        "description": "",
        "method": "DELETE",
        "body": {
            "mimeType": "application/json",
            "text": "{\n\t\"id\": \"0b207a17-e762-11ec-835d-16a7ac2ae94b\"\n}"
        },
        "parameters": [],
        "headers": [{
            "id": "pair_ced4177cb78e4765b447824b0dc70047",
            "name": "x-vtex-api-appkey",
            "value": "vtexappkey-compracertamkpqa-BGKNMC",
            "description": ""
        }, {
            "id": "pair_f591547080524c36bfed9f80282c016e",
            "name": "x-vtex-api-apptoken",
            "value": "KAZRKRCQFGFIEUDBOZGISLZBVTASEMVUQHGMXNRMINENGQGHFMZARRFZBYWQRKMTLKSXNREAITZFGLUENSNRNVDZBHKYRAVYAKCWYJUNIYRXRASQDSDYWUGQGSVFGFJH",
            "description": ""
        }, {
            "name": "Content-Type",
            "value": "application/json",
            "id": "pair_7595ed61127d452aa4cfd2510a1138c0"
        }],
        "authentication": {},
        "metaSortKey": -1650231415235.4375,
        "isPrivate": false,
        "settingStoreCookies": true,
        "settingSendCookies": true,
        "settingDisableRenderRequestBody": false,
        "settingEncodeUrl": true,
        "settingRebuildPath": true,
        "settingFollowRedirects": "global",
        "_type": "request"
    }, {
        "_id": "req_e45afe35eb9247a9996ce68b5d3da7b0",
        "parentId": "fld_655ccdca740c45e19be3f8cd984b21d1",
        "modified": 1654799447493,
        "created": 1652798973602,
        "url": "{{ _.base_ulr }}/user-identification-services",
        "name": "user-identification-services",
        "description": "",
        "method": "POST",
        "body": {
            "mimeType": "application/json",
            "text": "{\n\t\"serviceName\": \"service-4\",\n\t\"active\": true,\n\t\"connectionData\": {\"x-api-key\": \"abc\"}\n}"
        },
        "parameters": [],
        "headers": [{
            "id": "pair_ced4177cb78e4765b447824b0dc70047",
            "name": "x-vtex-api-appkey",
            "value": "vtexappkey-compracertamkpqa-BGKNMC",
            "description": ""
        }, {
            "id": "pair_f591547080524c36bfed9f80282c016e",
            "name": "x-vtex-api-apptoken",
            "value": "KAZRKRCQFGFIEUDBOZGISLZBVTASEMVUQHGMXNRMINENGQGHFMZARRFZBYWQRKMTLKSXNREAITZFGLUENSNRNVDZBHKYRAVYAKCWYJUNIYRXRASQDSDYWUGQGSVFGFJH",
            "description": ""
        }, {
            "name": "Content-Type",
            "value": "application/json",
            "id": "pair_7f2bb2b46a0b4cb5ad9019cce7758f05"
        }],
        "authentication": {},
        "metaSortKey": -1650231415199.5,
        "isPrivate": false,
        "settingStoreCookies": true,
        "settingSendCookies": true,
        "settingDisableRenderRequestBody": false,
        "settingEncodeUrl": true,
        "settingRebuildPath": true,
        "settingFollowRedirects": "global",
        "_type": "request"
    }, {
        "_id": "req_e03e043fe32047178f0dfb1f7bb20028",
        "parentId": "fld_4e9dcdf47ad64171aed83254b9c2fbde",
        "modified": 1654715563507,
        "created": 1651673528063,
        "url": "{{ _.base_ulr }}/rules",
        "name": "rules",
        "description": "",
        "method": "GET",
        "body": {},
        "parameters": [],
        "headers": [{
            "id": "pair_ced4177cb78e4765b447824b0dc70047",
            "name": "x-vtex-api-appkey",
            "value": "vtexappkey-compracertamkpqa-BGKNMC",
            "description": ""
        }, {
            "id": "pair_f591547080524c36bfed9f80282c016e",
            "name": "x-vtex-api-apptoken",
            "value": "KAZRKRCQFGFIEUDBOZGISLZBVTASEMVUQHGMXNRMINENGQGHFMZARRFZBYWQRKMTLKSXNREAITZFGLUENSNRNVDZBHKYRAVYAKCWYJUNIYRXRASQDSDYWUGQGSVFGFJH",
            "description": ""
        }],
        "authentication": {},
        "metaSortKey": -1650231415268.25,
        "isPrivate": false,
        "settingStoreCookies": true,
        "settingSendCookies": true,
        "settingDisableRenderRequestBody": false,
        "settingEncodeUrl": true,
        "settingRebuildPath": true,
        "settingFollowRedirects": "global",
        "_type": "request"
    }, {
        "_id": "fld_4e9dcdf47ad64171aed83254b9c2fbde",
        "parentId": "fld_5a85f29018ba4e56aa90bb78c2020f32",
        "modified": 1651673485508,
        "created": 1651673485508,
        "name": "rules",
        "description": "",
        "environment": {},
        "environmentPropertyOrder": null,
        "metaSortKey": -1651673485508,
        "_type": "request_group"
    }, {
        "_id": "req_3af4a478f02340e99d41b0aab932fc05",
        "parentId": "fld_4e9dcdf47ad64171aed83254b9c2fbde",
        "modified": 1654799332656,
        "created": 1654715468636,
        "url": "{{ _.base_ulr }}/rules",
        "name": "rules",
        "description": "",
        "method": "PATCH",
        "body": {
            "mimeType": "application/json",
            "text": "\t{\n\t\t\"id\": \"e2113265-e81f-11ec-835d-162544423325\",\n\t\t\"name\": \"Nova Regra Atualizada\",\n\t\t\"endpoint\": null,\n\t\t\"active\": false\n\t}"
        },
        "parameters": [],
        "headers": [{
            "id": "pair_ced4177cb78e4765b447824b0dc70047",
            "name": "x-vtex-api-appkey",
            "value": "vtexappkey-compracertamkpqa-BGKNMC",
            "description": ""
        }, {
            "id": "pair_f591547080524c36bfed9f80282c016e",
            "name": "x-vtex-api-apptoken",
            "value": "KAZRKRCQFGFIEUDBOZGISLZBVTASEMVUQHGMXNRMINENGQGHFMZARRFZBYWQRKMTLKSXNREAITZFGLUENSNRNVDZBHKYRAVYAKCWYJUNIYRXRASQDSDYWUGQGSVFGFJH",
            "description": ""
        }, {
            "name": "Content-Type",
            "value": "application/json",
            "id": "pair_825c80cf92674b9da2ba3662918f92b2"
        }],
        "authentication": {},
        "metaSortKey": -1650231415258.875,
        "isPrivate": false,
        "settingStoreCookies": true,
        "settingSendCookies": true,
        "settingDisableRenderRequestBody": false,
        "settingEncodeUrl": true,
        "settingRebuildPath": true,
        "settingFollowRedirects": "global",
        "_type": "request"
    }, {
        "_id": "req_2af5fb8df63d485e9f1ee3d1df7b4a3a",
        "parentId": "fld_4e9dcdf47ad64171aed83254b9c2fbde",
        "modified": 1654799350062,
        "created": 1654715782023,
        "url": "{{ _.base_ulr }}/rules",
        "name": "rules",
        "description": "",
        "method": "DELETE",
        "body": {
            "mimeType": "application/json",
            "text": "\t{\n\t\t\"id\": \"36db21b3-e81f-11ec-835d-0ee0eccb1eb3\"\n\t}"
        },
        "parameters": [],
        "headers": [{
            "id": "pair_ced4177cb78e4765b447824b0dc70047",
            "name": "x-vtex-api-appkey",
            "value": "vtexappkey-compracertamkpqa-BGKNMC",
            "description": ""
        }, {
            "id": "pair_f591547080524c36bfed9f80282c016e",
            "name": "x-vtex-api-apptoken",
            "value": "KAZRKRCQFGFIEUDBOZGISLZBVTASEMVUQHGMXNRMINENGQGHFMZARRFZBYWQRKMTLKSXNREAITZFGLUENSNRNVDZBHKYRAVYAKCWYJUNIYRXRASQDSDYWUGQGSVFGFJH",
            "description": ""
        }, {
            "name": "Content-Type",
            "value": "application/json",
            "id": "pair_825c80cf92674b9da2ba3662918f92b2"
        }],
        "authentication": {},
        "metaSortKey": -1650231415254.1875,
        "isPrivate": false,
        "settingStoreCookies": true,
        "settingSendCookies": true,
        "settingDisableRenderRequestBody": false,
        "settingEncodeUrl": true,
        "settingRebuildPath": true,
        "settingFollowRedirects": "global",
        "_type": "request"
    }, {
        "_id": "req_b785f935b98c4984bd2ed04574646b4b",
        "parentId": "fld_4e9dcdf47ad64171aed83254b9c2fbde",
        "modified": 1654799330921,
        "created": 1651673503386,
        "url": "{{ _.base_ulr }}/rules",
        "name": "rules",
        "description": "",
        "method": "POST",
        "body": {
            "mimeType": "application/json",
            "text": "\t{\n\t\t\"name\": \"Nova Regra\",\n\t\t\"endpoint\": null,\n\t\t\"active\": true\n\t}"
        },
        "parameters": [],
        "headers": [{
            "id": "pair_ced4177cb78e4765b447824b0dc70047",
            "name": "x-vtex-api-appkey",
            "value": "vtexappkey-compracertamkpqa-BGKNMC",
            "description": ""
        }, {
            "id": "pair_f591547080524c36bfed9f80282c016e",
            "name": "x-vtex-api-apptoken",
            "value": "KAZRKRCQFGFIEUDBOZGISLZBVTASEMVUQHGMXNRMINENGQGHFMZARRFZBYWQRKMTLKSXNREAITZFGLUENSNRNVDZBHKYRAVYAKCWYJUNIYRXRASQDSDYWUGQGSVFGFJH",
            "description": ""
        }, {
            "name": "Content-Type",
            "value": "application/json",
            "id": "pair_7f2bb2b46a0b4cb5ad9019cce7758f05"
        }],
        "authentication": {},
        "metaSortKey": -1650231415218.25,
        "isPrivate": false,
        "settingStoreCookies": true,
        "settingSendCookies": true,
        "settingDisableRenderRequestBody": false,
        "settingEncodeUrl": true,
        "settingRebuildPath": true,
        "settingFollowRedirects": "global",
        "_type": "request"
    }, {
        "_id": "req_c10df8e9cd20474ea2d828eeee3a89f2",
        "parentId": "fld_5a85f29018ba4e56aa90bb78c2020f32",
        "modified": 1654714748715,
        "created": 1647963984082,
        "url": "{{ _.base_ulr }}/validator",
        "name": "validator",
        "description": "",
        "method": "POST",
        "body": {
            "mimeType": "application/json",
            "text": "{\n\t\"rules\": [\n\t\t{\n\t\t\t\"id\": \"5032e420-c76f-11ec-835d-163b125ee3a1\",\n\t\t\t\"externalTrigger\": false\n\t\t}\n\t],\n\t\"orderForm\": {\n\t\t\"clientProfileData\": {\n\t\t\t\t\"email\": \"testando2@testando.com.br\"\n\t\t}\n\t}\n}"
        },
        "parameters": [],
        "headers": [{
            "id": "pair_ced4177cb78e4765b447824b0dc70047",
            "name": "",
            "value": "",
            "description": ""
        }, {
            "name": "Content-Type",
            "value": "application/json",
            "id": "pair_82d9c4c990344082b046d969e2f9cce6"
        }],
        "authentication": {},
        "metaSortKey": -1647963984082,
        "isPrivate": false,
        "settingStoreCookies": true,
        "settingSendCookies": true,
        "settingDisableRenderRequestBody": false,
        "settingEncodeUrl": true,
        "settingRebuildPath": true,
        "settingFollowRedirects": "global",
        "_type": "request"
    }, {
        "_id": "env_9d37eb52059c2360a24731ba06b29530dd1faa95",
        "parentId": "wrk_12c2b5256773445eb62fa3098896fc7e",
        "modified": 1654714497477,
        "created": 1628102005376,
        "name": "Base Environment",
        "data": {},
        "dataPropertyOrder": {},
        "color": null,
        "isPrivate": false,
        "metaSortKey": 1628102005376,
        "_type": "environment"
    }, {
        "_id": "jar_9d37eb52059c2360a24731ba06b29530dd1faa95",
        "parentId": "wrk_12c2b5256773445eb62fa3098896fc7e",
        "modified": 1654714373401,
        "created": 1628102005380,
        "name": "Default Jar",
        "_type": "cookie_jar"
    }, {
        "_id": "spc_3151c439afa44e2ca5abaa05a4b2200f",
        "parentId": "wrk_12c2b5256773445eb62fa3098896fc7e",
        "modified": 1628102005301,
        "created": 1628102005301,
        "fileName": "Insomnia",
        "contents": "",
        "contentType": "yaml",
        "_type": "api_spec"
    }, {
        "_id": "env_e544ae42820746ec858991b03e8544d7",
        "parentId": "env_9d37eb52059c2360a24731ba06b29530dd1faa95",
        "modified": 1654718548156,
        "created": 1654714517512,
        "name": "orderformmediator",
        "data": {
            "base_ulr": "https://{workspace}--{account}.myvtex.com/orderformmediator"
        },
        "dataPropertyOrder": {
            "&": ["base_ulr"]
        },
        "color": null,
        "isPrivate": false,
        "metaSortKey": 1654714517512,
        "_type": "environment"
    }]
}
```
