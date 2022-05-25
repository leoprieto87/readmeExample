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

| Retornos |  Body  | Info |
|:-----|:--------:|------:|
|  Aprovado  | `'aproved'` | $1600 |
| Negado   |  `denied`  |   $12 |
| L2   | _italic_ |    $1 |

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

Para incluir um e-mail na Rejection List, utilize o curl para configurar via Postman:

``` curl
curl --request POST \
  --url https://compracertamkpqa--compracertamkpqa.myvtex.com/validator \
  --header 'Content-Type: application/json' \
  --data '{
	"rules": [
		{
			"id": 1,
			"externalTrigger": false
		}
	],
	"orderForm": {
		"orderFormId": "b96db71ed88c48aa961d9f83fef409b5",
		"salesChannel": "1",
		"loggedIn": false,
		"isCheckedIn": false,
		"storeId": null,
		"checkedInPickupPointId": null,
		"allowManualPrice": false,
		"canEditData": true,
		"userProfileId": null,
		"userType": null,
		"ignoreProfileData": false,
		"value": 35999,
		"messages": [],
		"items": [],
		"selectableGifts": [],
		"totalizers": [
		],
		"shippingData": {
			"address": {
				"addressType": "residential",
				"receiverName": "Testando Corebiz",
				"addressId": "4915104690000",
				"isDisposable": true,
				"postalCode": "01130-000",
				"city": "São Paulo",
				"state": "SP",
				"country": "BRA",
				"street": "Rua Anhaia",
				"number": "55",
				"neighborhood": "Bom Retiro",
				"complement": "",
				"reference": null,
				"geoCoordinates": [
					-46.64506149291992,
					-23.5242919921875
				]
			},
			"logisticsInfo": [
				{
					"itemIndex": 0,
					"selectedSla": "Retirada Funcionário",
					"selectedDeliveryChannel": "delivery",
					"addressId": "4915104690000",
					"slas": [
						{
							"id": "Retirada Funcionário",
							"deliveryChannel": "delivery",
							"name": "Retirada Funcionário",
							"deliveryIds": [
								{
									"courierId": "124efcc",
									"warehouseId": "1_1",
									"dockId": "118863c",
									"courierName": "CDSP Retira (Retirada) v20200603",
									"quantity": 1,
									"kitItemDetails": []
								}
							],
							"shippingEstimate": "0bd",
							"shippingEstimateDate": null,
							"lockTTL": null,
							"availableDeliveryWindows": [],
							"deliveryWindow": null,
							"price": 0,
							"listPrice": 0,
							"tax": 0,
							"pickupStoreInfo": {
								"isPickupStore": false,
								"friendlyName": null,
								"address": null,
								"additionalInfo": null,
								"dockId": null
							},
							"pickupPointId": null,
							"pickupDistance": 0,
							"polygonName": "",
							"transitTime": "0bd"
						},
						{
							"id": "Normal",
							"deliveryChannel": "delivery",
							"name": "Normal",
							"deliveryIds": [
								{
									"courierId": "11f5703",
									"warehouseId": "1_1",
									"dockId": "15e4fb3",
									"courierName": "CDSP Total Standard.1 (Normal) v20220202",
									"quantity": 1,
									"kitItemDetails": []
								}
							],
							"shippingEstimate": "3bd",
							"shippingEstimateDate": null,
							"lockTTL": null,
							"availableDeliveryWindows": [],
							"deliveryWindow": null,
							"price": 863,
							"listPrice": 863,
							"tax": 0,
							"pickupStoreInfo": {
								"isPickupStore": false,
								"friendlyName": null,
								"address": null,
								"additionalInfo": null,
								"dockId": null
							},
							"pickupPointId": null,
							"pickupDistance": 0,
							"polygonName": "",
							"transitTime": "2bd"
						},
						{
							"id": "Econômica",
							"deliveryChannel": "delivery",
							"name": "Econômica",
							"deliveryIds": [
								{
									"courierId": "19be782",
									"warehouseId": "1_1",
									"dockId": "1122b4c",
									"courierName": "CDSP Mandaê F-Combate.01 (Econômica) v20211019",
									"quantity": 1,
									"kitItemDetails": []
								}
							],
							"shippingEstimate": "4bd",
							"shippingEstimateDate": null,
							"lockTTL": null,
							"availableDeliveryWindows": [],
							"deliveryWindow": null,
							"price": 884,
							"listPrice": 884,
							"tax": 0,
							"pickupStoreInfo": {
								"isPickupStore": false,
								"friendlyName": null,
								"address": null,
								"additionalInfo": null,
								"dockId": null
							},
							"pickupPointId": null,
							"pickupDistance": 0,
							"polygonName": "",
							"transitTime": "3bd"
						},
						{
							"id": "Urgente",
							"deliveryChannel": "delivery",
							"name": "Urgente",
							"deliveryIds": [
								{
									"courierId": "182344d",
									"warehouseId": "1_1",
									"dockId": "118863c",
									"courierName": "CDSP Loggi Corp (Urgente) v20211005",
									"quantity": 1,
									"kitItemDetails": []
								}
							],
							"shippingEstimate": "3h",
							"shippingEstimateDate": null,
							"lockTTL": null,
							"availableDeliveryWindows": [],
							"deliveryWindow": null,
							"price": 1990,
							"listPrice": 1990,
							"tax": 0,
							"pickupStoreInfo": {
								"isPickupStore": false,
								"friendlyName": null,
								"address": null,
								"additionalInfo": null,
								"dockId": null
							},
							"pickupPointId": null,
							"pickupDistance": 0,
							"polygonName": "B2C001-05KM",
							"transitTime": "3h"
						},
						{
							"id": "ShiptoStore (hig011)",
							"deliveryChannel": "pickup-in-point",
							"name": "ShiptoStore (hig011)",
							"deliveryIds": [
								{
									"courierId": "1470685",
									"warehouseId": "1_1",
									"dockId": "1601a07",
									"courierName": "CDSP Total Retira Loja (ShiptoStore) v20211006",
									"quantity": 1,
									"kitItemDetails": []
								}
							],
							"shippingEstimate": "6bd",
							"shippingEstimateDate": null,
							"lockTTL": null,
							"availableDeliveryWindows": [],
							"deliveryWindow": null,
							"price": 0,
							"listPrice": 1080,
							"tax": 0,
							"pickupStoreInfo": {
								"isPickupStore": true,
								"friendlyName": "Shoulder Pátio Higienopolis",
								"address": {
									"addressType": "pickup",
									"receiverName": null,
									"addressId": "hig011",
									"isDisposable": true,
									"postalCode": "01238-001",
									"city": "São Paulo",
									"state": "SP",
									"country": "BRA",
									"street": "Avenida Higienópolis",
									"number": "618",
									"neighborhood": "Higienópolis",
									"complement": "LOJA 162/163/164 - LOJA SHOULDER",
									"reference": null,
									"geoCoordinates": [
										-46.65763,
										-23.54225
									]
								},
								"additionalInfo": "",
								"dockId": "1601a07"
							},
							"pickupPointId": "1_hig011",
							"pickupDistance": 2.372589349746704,
							"polygonName": "",
							"transitTime": "2bd"
						},
						{
							"id": "ShiptoStore (cno009)",
							"deliveryChannel": "pickup-in-point",
							"name": "ShiptoStore (cno009)",
							"deliveryIds": [
								{
									"courierId": "1470685",
									"warehouseId": "1_1",
									"dockId": "1601a07",
									"courierName": "CDSP Total Retira Loja (ShiptoStore) v20211006",
									"quantity": 1,
									"kitItemDetails": []
								}
							],
							"shippingEstimate": "6bd",
							"shippingEstimateDate": null,
							"lockTTL": null,
							"availableDeliveryWindows": [],
							"deliveryWindow": null,
							"price": 0,
							"listPrice": 1382,
							"tax": 0,
							"pickupStoreInfo": {
								"isPickupStore": true,
								"friendlyName": "Shoulder Center Norte",
								"address": {
									"addressType": "pickup",
									"receiverName": null,
									"addressId": "cno009",
									"isDisposable": true,
									"postalCode": "02047-050",
									"city": "São Paulo",
									"state": "SP",
									"country": "BRA",
									"street": "Travessa Casalbuono",
									"number": "120",
									"neighborhood": "Vila Guilherme",
									"complement": "LOJA 617 / LOJA SHOULDER",
									"reference": null,
									"geoCoordinates": [
										-46.61838,
										-23.51553
									]
								},
								"additionalInfo": "",
								"dockId": "1601a07"
							},
							"pickupPointId": "1_cno009",
							"pickupDistance": 2.889571189880371,
							"polygonName": "",
							"transitTime": "2bd"
						},
						{
							"id": "ShiptoStore (bou020)",
							"deliveryChannel": "pickup-in-point",
							"name": "ShiptoStore (bou020)",
							"deliveryIds": [
								{
									"courierId": "1470685",
									"warehouseId": "1_1",
									"dockId": "1601a07",
									"courierName": "CDSP Total Retira Loja (ShiptoStore) v20211006",
									"quantity": 1,
									"kitItemDetails": []
								}
							],
							"shippingEstimate": "6bd",
							"shippingEstimateDate": null,
							"lockTTL": null,
							"availableDeliveryWindows": [],
							"deliveryWindow": null,
							"price": 0,
							"listPrice": 1080,
							"tax": 0,
							"pickupStoreInfo": {
								"isPickupStore": true,
								"friendlyName": "Shoulder Bourbon",
								"address": {
									"addressType": "pickup",
									"receiverName": null,
									"addressId": "bou020",
									"isDisposable": true,
									"postalCode": "05005-001",
									"city": "São Paulo",
									"state": "SP",
									"country": "BRA",
									"street": "Rua Palestra Itália",
									"number": "500",
									"neighborhood": "Perdizes",
									"complement": "2o. Piso / LOJA 137 - LOJA SHOULDER",
									"reference": null,
									"geoCoordinates": [
										-46.68095,
										-23.5267
									]
								},
								"additionalInfo": "",
								"dockId": "1601a07"
							},
							"pickupPointId": "1_bou020",
							"pickupDistance": 3.668722152709961,
							"polygonName": "",
							"transitTime": "2bd"
						},
						{
							"id": "ShiptoStore (CSP064)",
							"deliveryChannel": "pickup-in-point",
							"name": "ShiptoStore (CSP064)",
							"deliveryIds": [
								{
									"courierId": "1470685",
									"warehouseId": "1_1",
									"dockId": "1601a07",
									"courierName": "CDSP Total Retira Loja (ShiptoStore) v20211006",
									"quantity": 1,
									"kitItemDetails": []
								}
							],
							"shippingEstimate": "6bd",
							"shippingEstimateDate": null,
							"lockTTL": null,
							"availableDeliveryWindows": [],
							"deliveryWindow": null,
							"price": 0,
							"listPrice": 1080,
							"tax": 0,
							"pickupStoreInfo": {
								"isPickupStore": true,
								"friendlyName": "Shopping Cidade São Paulo",
								"address": {
									"addressType": "pickup",
									"receiverName": null,
									"addressId": "CSP064",
									"isDisposable": true,
									"postalCode": "01310-000",
									"city": "São Paulo",
									"state": "SP",
									"country": "BRA",
									"street": "Avenida Paulista",
									"number": "1230",
									"neighborhood": "Bela Vista",
									"complement": "LOJA SUC 0109 - PAVMTO TERREO - LOJA SHOULDER                               ",
									"reference": null,
									"geoCoordinates": [
										-46.65269,
										-23.56352
									]
								},
								"additionalInfo": "",
								"dockId": "1601a07"
							},
							"pickupPointId": "1_CSP064",
							"pickupDistance": 4.430738925933838,
							"polygonName": "",
							"transitTime": "2bd"
						},
						{
							"id": "ShiptoStore (OFR016)",
							"deliveryChannel": "pickup-in-point",
							"name": "ShiptoStore (OFR016)",
							"deliveryIds": [
								{
									"courierId": "1470685",
									"warehouseId": "1_1",
									"dockId": "1601a07",
									"courierName": "CDSP Total Retira Loja (ShiptoStore) v20211006",
									"quantity": 1,
									"kitItemDetails": []
								}
							],
							"shippingEstimate": "6bd",
							"shippingEstimateDate": null,
							"lockTTL": null,
							"availableDeliveryWindows": [],
							"deliveryWindow": null,
							"price": 0,
							"listPrice": 1080,
							"tax": 0,
							"pickupStoreInfo": {
								"isPickupStore": true,
								"friendlyName": "Shoulder Oscar Freire",
								"address": {
									"addressType": "pickup",
									"receiverName": null,
									"addressId": "OFR016",
									"isDisposable": true,
									"postalCode": "01426-001",
									"city": "São Paulo",
									"state": "SP",
									"country": "BRA",
									"street": "Rua Oscar Freire",
									"number": "819",
									"neighborhood": "Cerqueira César",
									"complement": "LOJA SHOULDER",
									"reference": null,
									"geoCoordinates": [
										-46.67572,
										-23.5551
									]
								},
								"additionalInfo": "",
								"dockId": "1601a07"
							},
							"pickupPointId": "1_OFR016",
							"pickupDistance": 4.637185096740723,
							"polygonName": "",
							"transitTime": "2bd"
						},
						{
							"id": "ShiptoStore (eld042)",
							"deliveryChannel": "pickup-in-point",
							"name": "ShiptoStore (eld042)",
							"deliveryIds": [
								{
									"courierId": "1470685",
									"warehouseId": "1_1",
									"dockId": "1601a07",
									"courierName": "CDSP Total Retira Loja (ShiptoStore) v20211006",
									"quantity": 1,
									"kitItemDetails": []
								}
							],
							"shippingEstimate": "6bd",
							"shippingEstimateDate": null,
							"lockTTL": null,
							"availableDeliveryWindows": [],
							"deliveryWindow": null,
							"price": 0,
							"listPrice": 1080,
							"tax": 0,
							"pickupStoreInfo": {
								"isPickupStore": true,
								"friendlyName": "Shoulder Eldorado",
								"address": {
									"addressType": "pickup",
									"receiverName": null,
									"addressId": "eld042",
									"isDisposable": true,
									"postalCode": "05402-600",
									"city": "São Paulo",
									"state": "SP",
									"country": "BRA",
									"street": "Avenida Rebouças",
									"number": "3970",
									"neighborhood": "Pinheiros",
									"complement": "LOJA Nº 210B 1º PISO - LOJA SHOULDER",
									"reference": null,
									"geoCoordinates": [
										-46.69571,
										-23.57246
									]
								},
								"additionalInfo": "",
								"dockId": "1601a07"
							},
							"pickupPointId": "1_eld042",
							"pickupDistance": 7.4392499923706055,
							"polygonName": "",
							"transitTime": "2bd"
						},
						{
							"id": "ShiptoStore (MOO087)",
							"deliveryChannel": "pickup-in-point",
							"name": "ShiptoStore (MOO087)",
							"deliveryIds": [
								{
									"courierId": "1470685",
									"warehouseId": "1_1",
									"dockId": "1601a07",
									"courierName": "CDSP Total Retira Loja (ShiptoStore) v20211006",
									"quantity": 1,
									"kitItemDetails": []
								}
							],
							"shippingEstimate": "6bd",
							"shippingEstimateDate": null,
							"lockTTL": null,
							"availableDeliveryWindows": [],
							"deliveryWindow": null,
							"price": 0,
							"listPrice": 1080,
							"tax": 0,
							"pickupStoreInfo": {
								"isPickupStore": true,
								"friendlyName": "Shoulder Mooca Plaza",
								"address": {
									"addressType": "pickup",
									"receiverName": null,
									"addressId": "MOO087",
									"isDisposable": true,
									"postalCode": "03126-000",
									"city": "São Paulo",
									"state": "SP",
									"country": "BRA",
									"street": "Rua Capitão Pacheco e Chaves",
									"number": "313",
									"neighborhood": "Vila Prudente",
									"complement": "Pavmto 1 luc / Suc 1034  - LOJA SHOULDER ",
									"reference": null,
									"geoCoordinates": [
										-46.59379,
										-23.58135
									]
								},
								"additionalInfo": "",
								"dockId": "1601a07"
							},
							"pickupPointId": "1_MOO087",
							"pickupDistance": 8.219890594482422,
							"polygonName": "",
							"transitTime": "2bd"
						},
						{
							"id": "ShiptoStore (jki037)",
							"deliveryChannel": "pickup-in-point",
							"name": "ShiptoStore (jki037)",
							"deliveryIds": [
								{
									"courierId": "1470685",
									"warehouseId": "1_1",
									"dockId": "1601a07",
									"courierName": "CDSP Total Retira Loja (ShiptoStore) v20211006",
									"quantity": 1,
									"kitItemDetails": []
								}
							],
							"shippingEstimate": "6bd",
							"shippingEstimateDate": null,
							"lockTTL": null,
							"availableDeliveryWindows": [],
							"deliveryWindow": null,
							"price": 0,
							"listPrice": 1080,
							"tax": 0,
							"pickupStoreInfo": {
								"isPickupStore": true,
								"friendlyName": "Shoulder JK Iguatemi",
								"address": {
									"addressType": "pickup",
									"receiverName": null,
									"addressId": "jki037",
									"isDisposable": true,
									"postalCode": "04543-011",
									"city": "São Paulo",
									"state": "SP",
									"country": "BRA",
									"street": "Avenida Presidente Juscelino Kubitschek",
									"number": "2041",
									"neighborhood": "Vila Nova Conceição",
									"complement": " Lojas Nº 364/365 2º Pavimento  - LOJA SHOULDER",
									"reference": null,
									"geoCoordinates": [
										-46.69004,
										-23.59073
									]
								},
								"additionalInfo": "",
								"dockId": "1601a07"
							},
							"pickupPointId": "1_jki037",
							"pickupDistance": 8.694519996643066,
							"polygonName": "",
							"transitTime": "2bd"
						},
						{
							"id": "ShiptoStore (VOL051)",
							"deliveryChannel": "pickup-in-point",
							"name": "ShiptoStore (VOL051)",
							"deliveryIds": [
								{
									"courierId": "1470685",
									"warehouseId": "1_1",
									"dockId": "1601a07",
									"courierName": "CDSP Total Retira Loja (ShiptoStore) v20211006",
									"quantity": 1,
									"kitItemDetails": []
								}
							],
							"shippingEstimate": "6bd",
							"shippingEstimateDate": null,
							"lockTTL": null,
							"availableDeliveryWindows": [],
							"deliveryWindow": null,
							"price": 0,
							"listPrice": 1080,
							"tax": 0,
							"pickupStoreInfo": {
								"isPickupStore": true,
								"friendlyName": "Shoulder Vila Olímpia",
								"address": {
									"addressType": "pickup",
									"receiverName": null,
									"addressId": "VOL051",
									"isDisposable": true,
									"postalCode": "04547-000",
									"city": "São Paulo",
									"state": "SP",
									"country": "BRA",
									"street": "Rua Olimpíadas",
									"number": "360",
									"neighborhood": "Itaim Bibi",
									"complement": "LOJA 203 - LOJA SHOULDER",
									"reference": null,
									"geoCoordinates": [
										-46.68642,
										-23.59539
									]
								},
								"additionalInfo": "",
								"dockId": "1601a07"
							},
							"pickupPointId": "1_VOL051",
							"pickupDistance": 8.959437370300293,
							"polygonName": "",
							"transitTime": "2bd"
						},
						{
							"id": "ShiptoStore (vlo049)",
							"deliveryChannel": "pickup-in-point",
							"name": "ShiptoStore (vlo049)",
							"deliveryIds": [
								{
									"courierId": "1470685",
									"warehouseId": "1_1",
									"dockId": "1601a07",
									"courierName": "CDSP Total Retira Loja (ShiptoStore) v20211006",
									"quantity": 1,
									"kitItemDetails": []
								}
							],
							"shippingEstimate": "6bd",
							"shippingEstimateDate": null,
							"lockTTL": null,
							"availableDeliveryWindows": [],
							"deliveryWindow": null,
							"price": 0,
							"listPrice": 1080,
							"tax": 0,
							"pickupStoreInfo": {
								"isPickupStore": true,
								"friendlyName": "Shoulder Villa-Lobos",
								"address": {
									"addressType": "pickup",
									"receiverName": null,
									"addressId": "vlo049",
									"isDisposable": true,
									"postalCode": "04795-100",
									"city": "São Paulo",
									"state": "SP",
									"country": "BRA",
									"street": "Avenida das Nações Unidas",
									"number": "4777      ",
									"neighborhood": "Vila Almeida",
									"complement": "LOTE A - LOJA 245/246 C - 2º PISO - LOJA SHOULDER",
									"reference": null,
									"geoCoordinates": [
										-46.73159,
										-23.54669
									]
								},
								"additionalInfo": "",
								"dockId": "1601a07"
							},
							"pickupPointId": "1_vlo049",
							"pickupDistance": 9.166008949279785,
							"polygonName": "",
							"transitTime": "2bd"
						}
					],
					"shipsTo": [
						"BRA"
					],
					"itemId": "2090417",
					"deliveryChannels": [
						{
							"id": "delivery"
						},
						{
							"id": "pickup-in-point"
						}
					]
				}
			],
			"selectedAddresses": [
				{
					"addressType": "residential",
					"receiverName": "Testando Corebiz",
					"addressId": "4915104690000",
					"isDisposable": true,
					"postalCode": "01130-000",
					"city": "São Paulo",
					"state": "SP",
					"country": "BRA",
					"street": "Rua Anhaia",
					"number": "55",
					"neighborhood": "Bom Retiro",
					"complement": "",
					"reference": null,
					"geoCoordinates": [
						-46.64506149291992,
						-23.5242919921875
					]
				}
			],
			"availableAddresses": [
				{
					"addressType": "residential",
					"receiverName": "Testando Corebiz",
					"addressId": "4915104690000",
					"isDisposable": true,
					"postalCode": "01130-000",
					"city": "São Paulo",
					"state": "SP",
					"country": "BRA",
					"street": "Rua Anhaia",
					"number": "55",
					"neighborhood": "Bom Retiro",
					"complement": "",
					"reference": null,
					"geoCoordinates": [
						-46.64506149291992,
						-23.5242919921875
					]
				}
			],
			"pickupPoints": [
				{
					"friendlyName": "Shoulder Pátio Higienopolis",
					"address": {
						"addressType": "pickup",
						"receiverName": null,
						"addressId": "hig011",
						"isDisposable": true,
						"postalCode": "01238-001",
						"city": "São Paulo",
						"state": "SP",
						"country": "BRA",
						"street": "Avenida Higienópolis",
						"number": "618",
						"neighborhood": "Higienópolis",
						"complement": "LOJA 162/163/164 - LOJA SHOULDER",
						"reference": null,
						"geoCoordinates": [
							-46.65763,
							-23.54225
						]
					},
					"additionalInfo": "",
					"id": "1_hig011",
					"businessHours": [
						{
							"DayOfWeek": 0,
							"OpeningTime": "14:00:00",
							"ClosingTime": "20:00:00"
						},
						{
							"DayOfWeek": 1,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 2,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 3,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 4,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 5,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 6,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						}
					]
				},
				{
					"friendlyName": "Shoulder Center Norte",
					"address": {
						"addressType": "pickup",
						"receiverName": null,
						"addressId": "cno009",
						"isDisposable": true,
						"postalCode": "02047-050",
						"city": "São Paulo",
						"state": "SP",
						"country": "BRA",
						"street": "Travessa Casalbuono",
						"number": "120",
						"neighborhood": "Vila Guilherme",
						"complement": "LOJA 617 / LOJA SHOULDER",
						"reference": null,
						"geoCoordinates": [
							-46.61838,
							-23.51553
						]
					},
					"additionalInfo": "",
					"id": "1_cno009",
					"businessHours": [
						{
							"DayOfWeek": 0,
							"OpeningTime": "14:00:00",
							"ClosingTime": "20:00:00"
						},
						{
							"DayOfWeek": 1,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 2,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 3,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 4,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 5,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 6,
							"OpeningTime": "00:00:00",
							"ClosingTime": "00:00:00"
						}
					]
				},
				{
					"friendlyName": "Shoulder Bourbon",
					"address": {
						"addressType": "pickup",
						"receiverName": null,
						"addressId": "bou020",
						"isDisposable": true,
						"postalCode": "05005-001",
						"city": "São Paulo",
						"state": "SP",
						"country": "BRA",
						"street": "Rua Palestra Itália",
						"number": "500",
						"neighborhood": "Perdizes",
						"complement": "2o. Piso / LOJA 137 - LOJA SHOULDER",
						"reference": null,
						"geoCoordinates": [
							-46.68095,
							-23.5267
						]
					},
					"additionalInfo": "",
					"id": "1_bou020",
					"businessHours": [
						{
							"DayOfWeek": 0,
							"OpeningTime": "14:00:00",
							"ClosingTime": "20:00:00"
						},
						{
							"DayOfWeek": 1,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 2,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 3,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 4,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 5,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 6,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						}
					]
				},
				{
					"friendlyName": "Shopping Cidade São Paulo",
					"address": {
						"addressType": "pickup",
						"receiverName": null,
						"addressId": "CSP064",
						"isDisposable": true,
						"postalCode": "01310-000",
						"city": "São Paulo",
						"state": "SP",
						"country": "BRA",
						"street": "Avenida Paulista",
						"number": "1230",
						"neighborhood": "Bela Vista",
						"complement": "LOJA SUC 0109 - PAVMTO TERREO - LOJA SHOULDER                               ",
						"reference": null,
						"geoCoordinates": [
							-46.65269,
							-23.56352
						]
					},
					"additionalInfo": "",
					"id": "1_CSP064",
					"businessHours": [
						{
							"DayOfWeek": 0,
							"OpeningTime": "14:00:00",
							"ClosingTime": "20:00:00"
						},
						{
							"DayOfWeek": 1,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 2,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 3,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 4,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 5,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 6,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						}
					]
				},
				{
					"friendlyName": "Shoulder Oscar Freire",
					"address": {
						"addressType": "pickup",
						"receiverName": null,
						"addressId": "OFR016",
						"isDisposable": true,
						"postalCode": "01426-001",
						"city": "São Paulo",
						"state": "SP",
						"country": "BRA",
						"street": "Rua Oscar Freire",
						"number": "819",
						"neighborhood": "Cerqueira César",
						"complement": "LOJA SHOULDER",
						"reference": null,
						"geoCoordinates": [
							-46.67572,
							-23.5551
						]
					},
					"additionalInfo": "",
					"id": "1_OFR016",
					"businessHours": [
						{
							"DayOfWeek": 0,
							"OpeningTime": "14:00:00",
							"ClosingTime": "20:00:00"
						},
						{
							"DayOfWeek": 1,
							"OpeningTime": "10:00:00",
							"ClosingTime": "20:00:00"
						},
						{
							"DayOfWeek": 2,
							"OpeningTime": "10:00:00",
							"ClosingTime": "20:00:00"
						},
						{
							"DayOfWeek": 3,
							"OpeningTime": "10:00:00",
							"ClosingTime": "20:00:00"
						},
						{
							"DayOfWeek": 4,
							"OpeningTime": "10:00:00",
							"ClosingTime": "20:00:00"
						},
						{
							"DayOfWeek": 5,
							"OpeningTime": "10:00:00",
							"ClosingTime": "20:00:00"
						},
						{
							"DayOfWeek": 6,
							"OpeningTime": "10:00:00",
							"ClosingTime": "20:00:00"
						}
					]
				},
				{
					"friendlyName": "Shoulder Eldorado",
					"address": {
						"addressType": "pickup",
						"receiverName": null,
						"addressId": "eld042",
						"isDisposable": true,
						"postalCode": "05402-600",
						"city": "São Paulo",
						"state": "SP",
						"country": "BRA",
						"street": "Avenida Rebouças",
						"number": "3970",
						"neighborhood": "Pinheiros",
						"complement": "LOJA Nº 210B 1º PISO - LOJA SHOULDER",
						"reference": null,
						"geoCoordinates": [
							-46.69571,
							-23.57246
						]
					},
					"additionalInfo": "",
					"id": "1_eld042",
					"businessHours": [
						{
							"DayOfWeek": 0,
							"OpeningTime": "14:00:00",
							"ClosingTime": "20:00:00"
						},
						{
							"DayOfWeek": 1,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 2,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 3,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 4,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 5,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 6,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						}
					]
				},
				{
					"friendlyName": "Shoulder Mooca Plaza",
					"address": {
						"addressType": "pickup",
						"receiverName": null,
						"addressId": "MOO087",
						"isDisposable": true,
						"postalCode": "03126-000",
						"city": "São Paulo",
						"state": "SP",
						"country": "BRA",
						"street": "Rua Capitão Pacheco e Chaves",
						"number": "313",
						"neighborhood": "Vila Prudente",
						"complement": "Pavmto 1 luc / Suc 1034  - LOJA SHOULDER ",
						"reference": null,
						"geoCoordinates": [
							-46.59379,
							-23.58135
						]
					},
					"additionalInfo": "",
					"id": "1_MOO087",
					"businessHours": [
						{
							"DayOfWeek": 0,
							"OpeningTime": "14:00:00",
							"ClosingTime": "20:00:00"
						},
						{
							"DayOfWeek": 1,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 2,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 3,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 4,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 5,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 6,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						}
					]
				},
				{
					"friendlyName": "Shoulder JK Iguatemi",
					"address": {
						"addressType": "pickup",
						"receiverName": null,
						"addressId": "jki037",
						"isDisposable": true,
						"postalCode": "04543-011",
						"city": "São Paulo",
						"state": "SP",
						"country": "BRA",
						"street": "Avenida Presidente Juscelino Kubitschek",
						"number": "2041",
						"neighborhood": "Vila Nova Conceição",
						"complement": " Lojas Nº 364/365 2º Pavimento  - LOJA SHOULDER",
						"reference": null,
						"geoCoordinates": [
							-46.69004,
							-23.59073
						]
					},
					"additionalInfo": "",
					"id": "1_jki037",
					"businessHours": [
						{
							"DayOfWeek": 0,
							"OpeningTime": "14:00:00",
							"ClosingTime": "20:00:00"
						},
						{
							"DayOfWeek": 1,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 2,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 3,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 4,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 5,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 6,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						}
					]
				},
				{
					"friendlyName": "Shoulder Vila Olímpia",
					"address": {
						"addressType": "pickup",
						"receiverName": null,
						"addressId": "VOL051",
						"isDisposable": true,
						"postalCode": "04547-000",
						"city": "São Paulo",
						"state": "SP",
						"country": "BRA",
						"street": "Rua Olimpíadas",
						"number": "360",
						"neighborhood": "Itaim Bibi",
						"complement": "LOJA 203 - LOJA SHOULDER",
						"reference": null,
						"geoCoordinates": [
							-46.68642,
							-23.59539
						]
					},
					"additionalInfo": "",
					"id": "1_VOL051",
					"businessHours": [
						{
							"DayOfWeek": 0,
							"OpeningTime": "14:00:00",
							"ClosingTime": "20:00:00"
						},
						{
							"DayOfWeek": 1,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 2,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 3,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 4,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 5,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 6,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						}
					]
				},
				{
					"friendlyName": "Shoulder Villa-Lobos",
					"address": {
						"addressType": "pickup",
						"receiverName": null,
						"addressId": "vlo049",
						"isDisposable": true,
						"postalCode": "04795-100",
						"city": "São Paulo",
						"state": "SP",
						"country": "BRA",
						"street": "Avenida das Nações Unidas",
						"number": "4777      ",
						"neighborhood": "Vila Almeida",
						"complement": "LOTE A - LOJA 245/246 C - 2º PISO - LOJA SHOULDER",
						"reference": null,
						"geoCoordinates": [
							-46.73159,
							-23.54669
						]
					},
					"additionalInfo": "",
					"id": "1_vlo049",
					"businessHours": [
						{
							"DayOfWeek": 0,
							"OpeningTime": "14:00:00",
							"ClosingTime": "20:00:00"
						},
						{
							"DayOfWeek": 1,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 2,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 3,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 4,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 5,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						},
						{
							"DayOfWeek": 6,
							"OpeningTime": "10:00:00",
							"ClosingTime": "22:00:00"
						}
					]
				}
			]
		},
		"clientProfileData": {
			"email": "testando2@testando.com.br",
			"firstName": "Testando",
			"lastName": "Corebiz",
			"document": "12356232702",
			"documentType": "cpf",
			"phone": "+5521969636218",
			"corporateName": null,
			"tradeName": null,
			"corporateDocument": null,
			"stateInscription": "",
			"corporatePhone": null,
			"isCorporate": false,
			"profileCompleteOnLoading": false,
			"profileErrorOnLoading": false,
			"customerClass": null
		},
		"paymentData": {
			"updateStatus": "updated",
			"installmentOptions": [
				{
					"paymentSystem": "1",
					"bin": null,
					"paymentName": null,
					"paymentGroupName": null,
					"value": 35999,
					"installments": [
						{
							"count": 1,
							"hasInterestRate": false,
							"interestRate": 0,
							"value": 35999,
							"total": 35999,
							"sellerMerchantInstallments": [
								{
									"id": "SHOULDER",
									"count": 1,
									"hasInterestRate": false,
									"interestRate": 0,
									"value": 35999,
									"total": 35999
								}
							]
						},
						{
							"count": 2,
							"hasInterestRate": false,
							"interestRate": 0,
							"value": 17999,
							"total": 35999,
							"sellerMerchantInstallments": [
								{
									"id": "SHOULDER",
									"count": 2,
									"hasInterestRate": false,
									"interestRate": 0,
									"value": 17999,
									"total": 35999
								}
							]
						},
						{
							"count": 3,
							"hasInterestRate": false,
							"interestRate": 0,
							"value": 11999,
							"total": 35999,
							"sellerMerchantInstallments": [
								{
									"id": "SHOULDER",
									"count": 3,
									"hasInterestRate": false,
									"interestRate": 0,
									"value": 11999,
									"total": 35999
								}
							]
						},
						{
							"count": 4,
							"hasInterestRate": false,
							"interestRate": 0,
							"value": 8999,
							"total": 35999,
							"sellerMerchantInstallments": [
								{
									"id": "SHOULDER",
									"count": 4,
									"hasInterestRate": false,
									"interestRate": 0,
									"value": 8999,
									"total": 35999
								}
							]
						},
						{
							"count": 5,
							"hasInterestRate": false,
							"interestRate": 0,
							"value": 7199,
							"total": 35999,
							"sellerMerchantInstallments": [
								{
									"id": "SHOULDER",
									"count": 5,
									"hasInterestRate": false,
									"interestRate": 0,
									"value": 7199,
									"total": 35999
								}
							]
						},
						{
							"count": 6,
							"hasInterestRate": false,
							"interestRate": 0,
							"value": 5999,
							"total": 35999,
							"sellerMerchantInstallments": [
								{
									"id": "SHOULDER",
									"count": 6,
									"hasInterestRate": false,
									"interestRate": 0,
									"value": 5999,
									"total": 35999
								}
							]
						},
						{
							"count": 7,
							"hasInterestRate": false,
							"interestRate": 0,
							"value": 5142,
							"total": 35999,
							"sellerMerchantInstallments": [
								{
									"id": "SHOULDER",
									"count": 7,
									"hasInterestRate": false,
									"interestRate": 0,
									"value": 5142,
									"total": 35999
								}
							]
						}
					]
				},
				{
					"paymentSystem": "2",
					"bin": null,
					"paymentName": null,
					"paymentGroupName": null,
					"value": 35999,
					"installments": [
						{
							"count": 1,
							"hasInterestRate": false,
							"interestRate": 0,
							"value": 35999,
							"total": 35999,
							"sellerMerchantInstallments": [
								{
									"id": "SHOULDER",
									"count": 1,
									"hasInterestRate": false,
									"interestRate": 0,
									"value": 35999,
									"total": 35999
								}
							]
						},
						{
							"count": 2,
							"hasInterestRate": false,
							"interestRate": 0,
							"value": 17999,
							"total": 35999,
							"sellerMerchantInstallments": [
								{
									"id": "SHOULDER",
									"count": 2,
									"hasInterestRate": false,
									"interestRate": 0,
									"value": 17999,
									"total": 35999
								}
							]
						},
						{
							"count": 3,
							"hasInterestRate": false,
							"interestRate": 0,
							"value": 11999,
							"total": 35999,
							"sellerMerchantInstallments": [
								{
									"id": "SHOULDER",
									"count": 3,
									"hasInterestRate": false,
									"interestRate": 0,
									"value": 11999,
									"total": 35999
								}
							]
						},
						{
							"count": 4,
							"hasInterestRate": false,
							"interestRate": 0,
							"value": 8999,
							"total": 35999,
							"sellerMerchantInstallments": [
								{
									"id": "SHOULDER",
									"count": 4,
									"hasInterestRate": false,
									"interestRate": 0,
									"value": 8999,
									"total": 35999
								}
							]
						},
						{
							"count": 5,
							"hasInterestRate": false,
							"interestRate": 0,
							"value": 7199,
							"total": 35999,
							"sellerMerchantInstallments": [
								{
									"id": "SHOULDER",
									"count": 5,
									"hasInterestRate": false,
									"interestRate": 0,
									"value": 7199,
									"total": 35999
								}
							]
						},
						{
							"count": 6,
							"hasInterestRate": false,
							"interestRate": 0,
							"value": 5999,
							"total": 35999,
							"sellerMerchantInstallments": [
								{
									"id": "SHOULDER",
									"count": 6,
									"hasInterestRate": false,
									"interestRate": 0,
									"value": 5999,
									"total": 35999
								}
							]
						},
						{
							"count": 7,
							"hasInterestRate": false,
							"interestRate": 0,
							"value": 5142,
							"total": 35999,
							"sellerMerchantInstallments": [
								{
									"id": "SHOULDER",
									"count": 7,
									"hasInterestRate": false,
									"interestRate": 0,
									"value": 5142,
									"total": 35999
								}
							]
						}
					]
				},
				{
					"paymentSystem": "4",
					"bin": null,
					"paymentName": null,
					"paymentGroupName": null,
					"value": 35999,
					"installments": [
						{
							"count": 1,
							"hasInterestRate": false,
							"interestRate": 0,
							"value": 35999,
							"total": 35999,
							"sellerMerchantInstallments": [
								{
									"id": "SHOULDER",
									"count": 1,
									"hasInterestRate": false,
									"interestRate": 0,
									"value": 35999,
									"total": 35999
								}
							]
						},
						{
							"count": 2,
							"hasInterestRate": false,
							"interestRate": 0,
							"value": 17999,
							"total": 35999,
							"sellerMerchantInstallments": [
								{
									"id": "SHOULDER",
									"count": 2,
									"hasInterestRate": false,
									"interestRate": 0,
									"value": 17999,
									"total": 35999
								}
							]
						},
						{
							"count": 3,
							"hasInterestRate": false,
							"interestRate": 0,
							"value": 11999,
							"total": 35999,
							"sellerMerchantInstallments": [
								{
									"id": "SHOULDER",
									"count": 3,
									"hasInterestRate": false,
									"interestRate": 0,
									"value": 11999,
									"total": 35999
								}
							]
						},
						{
							"count": 4,
							"hasInterestRate": false,
							"interestRate": 0,
							"value": 8999,
							"total": 35999,
							"sellerMerchantInstallments": [
								{
									"id": "SHOULDER",
									"count": 4,
									"hasInterestRate": false,
									"interestRate": 0,
									"value": 8999,
									"total": 35999
								}
							]
						},
						{
							"count": 5,
							"hasInterestRate": false,
							"interestRate": 0,
							"value": 7199,
							"total": 35999,
							"sellerMerchantInstallments": [
								{
									"id": "SHOULDER",
									"count": 5,
									"hasInterestRate": false,
									"interestRate": 0,
									"value": 7199,
									"total": 35999
								}
							]
						},
						{
							"count": 6,
							"hasInterestRate": false,
							"interestRate": 0,
							"value": 5999,
							"total": 35999,
							"sellerMerchantInstallments": [
								{
									"id": "SHOULDER",
									"count": 6,
									"hasInterestRate": false,
									"interestRate": 0,
									"value": 5999,
									"total": 35999
								}
							]
						},
						{
							"count": 7,
							"hasInterestRate": false,
							"interestRate": 0,
							"value": 5142,
							"total": 35999,
							"sellerMerchantInstallments": [
								{
									"id": "SHOULDER",
									"count": 7,
									"hasInterestRate": false,
									"interestRate": 0,
									"value": 5142,
									"total": 35999
								}
							]
						}
					]
				},
				{
					"paymentSystem": "8",
					"bin": null,
					"paymentName": null,
					"paymentGroupName": null,
					"value": 35999,
					"installments": [
						{
							"count": 1,
							"hasInterestRate": false,
							"interestRate": 0,
							"value": 35999,
							"total": 35999,
							"sellerMerchantInstallments": [
								{
									"id": "SHOULDER",
									"count": 1,
									"hasInterestRate": false,
									"interestRate": 0,
									"value": 35999,
									"total": 35999
								}
							]
						},
						{
							"count": 2,
							"hasInterestRate": false,
							"interestRate": 0,
							"value": 17999,
							"total": 35999,
							"sellerMerchantInstallments": [
								{
									"id": "SHOULDER",
									"count": 2,
									"hasInterestRate": false,
									"interestRate": 0,
									"value": 17999,
									"total": 35999
								}
							]
						},
						{
							"count": 3,
							"hasInterestRate": false,
							"interestRate": 0,
							"value": 11999,
							"total": 35999,
							"sellerMerchantInstallments": [
								{
									"id": "SHOULDER",
									"count": 3,
									"hasInterestRate": false,
									"interestRate": 0,
									"value": 11999,
									"total": 35999
								}
							]
						},
						{
							"count": 4,
							"hasInterestRate": false,
							"interestRate": 0,
							"value": 8999,
							"total": 35999,
							"sellerMerchantInstallments": [
								{
									"id": "SHOULDER",
									"count": 4,
									"hasInterestRate": false,
									"interestRate": 0,
									"value": 8999,
									"total": 35999
								}
							]
						},
						{
							"count": 5,
							"hasInterestRate": false,
							"interestRate": 0,
							"value": 7199,
							"total": 35999,
							"sellerMerchantInstallments": [
								{
									"id": "SHOULDER",
									"count": 5,
									"hasInterestRate": false,
									"interestRate": 0,
									"value": 7199,
									"total": 35999
								}
							]
						},
						{
							"count": 6,
							"hasInterestRate": false,
							"interestRate": 0,
							"value": 5999,
							"total": 35999,
							"sellerMerchantInstallments": [
								{
									"id": "SHOULDER",
									"count": 6,
									"hasInterestRate": false,
									"interestRate": 0,
									"value": 5999,
									"total": 35999
								}
							]
						},
						{
							"count": 7,
							"hasInterestRate": false,
							"interestRate": 0,
							"value": 5142,
							"total": 35999,
							"sellerMerchantInstallments": [
								{
									"id": "SHOULDER",
									"count": 7,
									"hasInterestRate": false,
									"interestRate": 0,
									"value": 5142,
									"total": 35999
								}
							]
						}
					]
				},
				{
					"paymentSystem": "9",
					"bin": null,
					"paymentName": null,
					"paymentGroupName": null,
					"value": 35999,
					"installments": [
						{
							"count": 1,
							"hasInterestRate": false,
							"interestRate": 0,
							"value": 35999,
							"total": 35999,
							"sellerMerchantInstallments": [
								{
									"id": "SHOULDER",
									"count": 1,
									"hasInterestRate": false,
									"interestRate": 0,
									"value": 35999,
									"total": 35999
								}
							]
						},
						{
							"count": 2,
							"hasInterestRate": false,
							"interestRate": 0,
							"value": 17999,
							"total": 35999,
							"sellerMerchantInstallments": [
								{
									"id": "SHOULDER",
									"count": 2,
									"hasInterestRate": false,
									"interestRate": 0,
									"value": 17999,
									"total": 35999
								}
							]
						},
						{
							"count": 3,
							"hasInterestRate": false,
							"interestRate": 0,
							"value": 11999,
							"total": 35999,
							"sellerMerchantInstallments": [
								{
									"id": "SHOULDER",
									"count": 3,
									"hasInterestRate": false,
									"interestRate": 0,
									"value": 11999,
									"total": 35999
								}
							]
						},
						{
							"count": 4,
							"hasInterestRate": false,
							"interestRate": 0,
							"value": 8999,
							"total": 35999,
							"sellerMerchantInstallments": [
								{
									"id": "SHOULDER",
									"count": 4,
									"hasInterestRate": false,
									"interestRate": 0,
									"value": 8999,
									"total": 35999
								}
							]
						},
						{
							"count": 5,
							"hasInterestRate": false,
							"interestRate": 0,
							"value": 7199,
							"total": 35999,
							"sellerMerchantInstallments": [
								{
									"id": "SHOULDER",
									"count": 5,
									"hasInterestRate": false,
									"interestRate": 0,
									"value": 7199,
									"total": 35999
								}
							]
						},
						{
							"count": 6,
							"hasInterestRate": false,
							"interestRate": 0,
							"value": 5999,
							"total": 35999,
							"sellerMerchantInstallments": [
								{
									"id": "SHOULDER",
									"count": 6,
									"hasInterestRate": false,
									"interestRate": 0,
									"value": 5999,
									"total": 35999
								}
							]
						},
						{
							"count": 7,
							"hasInterestRate": false,
							"interestRate": 0,
							"value": 5142,
							"total": 35999,
							"sellerMerchantInstallments": [
								{
									"id": "SHOULDER",
									"count": 7,
									"hasInterestRate": false,
									"interestRate": 0,
									"value": 5142,
									"total": 35999
								}
							]
						}
					]
				},
				{
					"paymentSystem": "16",
					"bin": null,
					"paymentName": null,
					"paymentGroupName": null,
					"value": 35999,
					"installments": [
						{
							"count": 1,
							"hasInterestRate": false,
							"interestRate": 0,
							"value": 35999,
							"total": 35999,
							"sellerMerchantInstallments": [
								{
									"id": "SHOULDER",
									"count": 1,
									"hasInterestRate": false,
									"interestRate": 0,
									"value": 35999,
									"total": 35999
								}
							]
						}
					]
				},
				{
					"paymentSystem": "72",
					"bin": null,
					"paymentName": null,
					"paymentGroupName": null,
					"value": 35999,
					"installments": [
						{
							"count": 1,
							"hasInterestRate": false,
							"interestRate": 0,
							"value": 35999,
							"total": 35999,
							"sellerMerchantInstallments": [
								{
									"id": "SHOULDER",
									"count": 1,
									"hasInterestRate": false,
									"interestRate": 0,
									"value": 35999,
									"total": 35999
								}
							]
						}
					]
				},
				{
					"paymentSystem": "125",
					"bin": null,
					"paymentName": null,
					"paymentGroupName": null,
					"value": 35999,
					"installments": [
						{
							"count": 1,
							"hasInterestRate": false,
							"interestRate": 0,
							"value": 35999,
							"total": 35999,
							"sellerMerchantInstallments": [
								{
									"id": "SHOULDER",
									"count": 1,
									"hasInterestRate": false,
									"interestRate": 0,
									"value": 35999,
									"total": 35999
								}
							]
						}
					]
				}
			],
			"paymentSystems": [
				{
					"id": 1,
					"name": "American Express",
					"groupName": "creditCardPaymentGroup",
					"validator": {
						"regex": "^3[47][0-9]{13}$",
						"mask": "9999 999999 99999",
						"cardCodeRegex": "^[0-9]{4}$",
						"cardCodeMask": "9999",
						"weights": [
							1,
							2,
							1,
							2,
							1,
							2,
							1,
							2,
							1,
							2,
							1,
							2,
							1,
							2,
							1,
							2,
							1,
							2,
							1
						],
						"useCvv": true,
						"useExpirationDate": true,
						"useCardHolderName": true,
						"useBillingAddress": true
					},
					"stringId": "1",
					"template": "creditCardPaymentGroup-template",
					"requiresDocument": true,
					"displayDocument": false,
					"isCustom": false,
					"description": null,
					"requiresAuthentication": false,
					"dueDate": "2022-04-20T17:11:01.3735119Z",
					"availablePayments": null
				},
				{
					"id": 2,
					"name": "Visa",
					"groupName": "creditCardPaymentGroup",
					"validator": {
						"regex": "^4[0-9]{15}$",
						"mask": "9999 9999 9999 9999",
						"cardCodeRegex": "^[0-9]{3}$",
						"cardCodeMask": "999",
						"weights": [
							2,
							1,
							2,
							1,
							2,
							1,
							2,
							1,
							2,
							1,
							2,
							1,
							2,
							1,
							2,
							1,
							2,
							1,
							2
						],
						"useCvv": true,
						"useExpirationDate": true,
						"useCardHolderName": true,
						"useBillingAddress": true
					},
					"stringId": "2",
					"template": "creditCardPaymentGroup-template",
					"requiresDocument": true,
					"displayDocument": false,
					"isCustom": false,
					"description": null,
					"requiresAuthentication": false,
					"dueDate": "2022-04-20T17:11:01.3735119Z",
					"availablePayments": null
				},
				{
					"id": 4,
					"name": "Mastercard",
					"groupName": "creditCardPaymentGroup",
					"validator": {
						"regex": "^((5(([1-2]|[4-5])[0-9]{8}|0((1|6)([0-9]{7}))|3(0(4((0|[2-9])[0-9]{5})|([0-3]|[5-9])[0-9]{6})|[1-9][0-9]{7})))|((508116)\\d{4,10})|((502121)\\d{4,10})|((589916)\\d{4,10})|(2[0-9]{15})|(67[0-9]{14})|(506387)\\d{4,10})",
						"mask": "9999 9999 9999 9999",
						"cardCodeRegex": "^[0-9]{3}$",
						"cardCodeMask": "999",
						"weights": [
							2,
							1,
							2,
							1,
							2,
							1,
							2,
							1,
							2,
							1,
							2,
							1,
							2,
							1,
							2,
							1,
							2,
							1,
							2
						],
						"useCvv": true,
						"useExpirationDate": true,
						"useCardHolderName": true,
						"useBillingAddress": true
					},
					"stringId": "4",
					"template": "creditCardPaymentGroup-template",
					"requiresDocument": true,
					"displayDocument": false,
					"isCustom": false,
					"description": null,
					"requiresAuthentication": false,
					"dueDate": "2022-04-20T17:11:01.3735119Z",
					"availablePayments": null
				},
				{
					"id": 8,
					"name": "Hipercard",
					"groupName": "creditCardPaymentGroup",
					"validator": {
						"regex": "^606282|^3841(?:[0|4|6]{1})0",
						"mask": "9999999999999999999",
						"cardCodeRegex": "^[0-9]{3}$",
						"cardCodeMask": "999",
						"weights": [
							2,
							1,
							2,
							1,
							2,
							1,
							2,
							1,
							2,
							1,
							2,
							1,
							2,
							1,
							2,
							1,
							1,
							1,
							1
						],
						"useCvv": true,
						"useExpirationDate": true,
						"useCardHolderName": true,
						"useBillingAddress": true
					},
					"stringId": "8",
					"template": "creditCardPaymentGroup-template",
					"requiresDocument": true,
					"displayDocument": false,
					"isCustom": false,
					"description": null,
					"requiresAuthentication": false,
					"dueDate": "2022-04-20T17:11:01.3735119Z",
					"availablePayments": null
				},
				{
					"id": 9,
					"name": "Elo",
					"groupName": "creditCardPaymentGroup",
					"validator": {
						"regex": "^(50(67(0[78]|1[5789]|2[012456789]|3[01234569]|4[0-7]|53|7[4-8])|9(0(0[0-9]|1[34]|2[0-7]|3[0359]|4[01235678]|5[01235789]|6[0-9]|7[01346789]|8[01234789]|9[123479])|1(0[34568]|4[6-9]|5[1-8]|8[356789])|2(2[0-2]|5[78]|6[1-9]|7[0-9]|8[01235678]|90)|357|4(0[7-9]|1[0-9]|2[0-2]|31|5[7-9]|6[0-6]|84)|55[01]|636|7(2[2-9]|6[5-9])))|4(0117[89]|3(1274|8935)|5(1416|7(393|63[12])))|6(27780|36368|5(0(0(3[12356789]|4[0-7]|7[78])|4(0[6-9]|1[0234]|2[2-9]|3[045789]|8[5-9]|9[0-9])|5(0[012346789]|1[0-9]|2[0-9]|3[0178]|5[2-9]|6[0-6]|7[7-9]|8[0-8]|9[1-8])|72[0-7]|9(0[1-9]|1[0-9]|2[0128]|3[89]|4[6-9]|5[0145]|6[235678]|71))|16(5[2-9]|6[0-9]|7[01456789])|50(0[0-9]|1[02345678]|36|5[1267]))))\\d{0,13}$",
						"mask": "9999 9999 9999 9999",
						"cardCodeRegex": "^[0-9]{3}$",
						"cardCodeMask": "999",
						"weights": [
							2,
							1,
							2,
							1,
							2,
							1,
							2,
							1,
							2,
							1,
							2,
							1,
							2,
							1,
							2,
							1,
							2,
							1,
							2
						],
						"useCvv": true,
						"useExpirationDate": true,
						"useCardHolderName": true,
						"useBillingAddress": true
					},
					"stringId": "9",
					"template": "creditCardPaymentGroup-template",
					"requiresDocument": true,
					"displayDocument": false,
					"isCustom": false,
					"description": null,
					"requiresAuthentication": false,
					"dueDate": "2022-04-20T17:11:01.3735119Z",
					"availablePayments": null
				},
				{
					"id": 16,
					"name": "Vale",
					"groupName": "giftCardPaymentGroup",
					"validator": {
						"regex": null,
						"mask": null,
						"cardCodeRegex": null,
						"cardCodeMask": null,
						"weights": null,
						"useCvv": false,
						"useExpirationDate": false,
						"useCardHolderName": false,
						"useBillingAddress": false
					},
					"stringId": "16",
					"template": "giftCardPaymentGroup-template",
					"requiresDocument": false,
					"displayDocument": false,
					"isCustom": false,
					"description": null,
					"requiresAuthentication": false,
					"dueDate": "2022-04-20T17:11:01.3735119Z",
					"availablePayments": null
				},
				{
					"id": 125,
					"name": "Pix",
					"groupName": "instantPaymentPaymentGroup",
					"validator": {
						"regex": null,
						"mask": null,
						"cardCodeRegex": null,
						"cardCodeMask": null,
						"weights": null,
						"useCvv": false,
						"useExpirationDate": false,
						"useCardHolderName": false,
						"useBillingAddress": false
					},
					"stringId": "125",
					"template": "instantPaymentPaymentGroup-template",
					"requiresDocument": false,
					"displayDocument": false,
					"isCustom": false,
					"description": null,
					"requiresAuthentication": false,
					"dueDate": "2022-04-20T17:11:01.3735119Z",
					"availablePayments": null
				},
				{
					"id": 72,
					"name": "PicPay",
					"groupName": "picPayPaymentGroup",
					"validator": {
						"regex": null,
						"mask": null,
						"cardCodeRegex": null,
						"cardCodeMask": null,
						"weights": null,
						"useCvv": false,
						"useExpirationDate": false,
						"useCardHolderName": false,
						"useBillingAddress": false
					},
					"stringId": "72",
					"template": "picPayPaymentGroup-template",
					"requiresDocument": false,
					"displayDocument": false,
					"isCustom": false,
					"description": null,
					"requiresAuthentication": false,
					"dueDate": "2022-04-20T17:11:01.3735119Z",
					"availablePayments": null
				}
			],
			"payments": [
				{
					"paymentSystem": "2",
					"bin": null,
					"accountId": null,
					"tokenId": null,
					"installments": null,
					"referenceValue": 35999,
					"value": 35999,
					"merchantSellerPayments": [
						{
							"id": "SHOULDER",
							"installments": null,
							"referenceValue": 35999,
							"value": 35999,
							"interestRate": null,
							"installmentValue": 0
						}
					]
				}
			],
			"giftCards": [],
			"giftCardMessages": [],
			"availableAccounts": [],
			"availableTokens": [],
			"availableAssociations": {}
		},
		"marketingData": {
			"utmSource": null,
			"utmMedium": null,
			"utmCampaign": null,
			"utmipage": null,
			"utmiPart": null,
			"utmiCampaign": null,
			"coupon": null,
			"marketingTags": [
				"vtexSocialSelling"
			]
		},
		"sellers": [
			{
				"id": "1",
				"name": "Shoulder eCommerce",
				"logo": ""
			}
		],
		"clientPreferencesData": {
			"locale": "pt-BR",
			"optinNewsLetter": false
		},
		"commercialConditionData": null,
		"storePreferencesData": {
			"countryCode": "BRA",
			"saveUserData": true,
			"timeZone": "E. South America Standard Time",
			"currencyCode": "BRL",
			"currencyLocale": 1046,
			"currencySymbol": "R$",
			"currencyFormatInfo": {
				"currencyDecimalDigits": 2,
				"currencyDecimalSeparator": ",",
				"currencyGroupSeparator": ".",
				"currencyGroupSize": 3,
				"startsWithCurrencySymbol": true
			}
		},
		"giftRegistryData": null,
		"openTextField": {
			"value": null
		},
		"invoiceData": null,
		"customData": null,
		"itemMetadata": {
			"items": [
				{
					"id": "2090417",
					"seller": "1",
					"name": "Kaftan jacquard fio dourado-off white-pp",
					"skuName": "Kaftan jacquard fio dourado-off white-pp",
					"productId": "2014666",
					"refId": "2124189000079PP",
					"ean": "2124189000079PP",
					"imageUrl": "http://shoulder.vteximg.com.br/arquivos/ids/1910663-200-255/212418900_0079_010-KAFTAN-JACQUARD-FIO-DOURADO.jpg?v=637717235334900000",
					"detailUrl": "/kaftan-jacquard-fio-dourado-212418900/p",
					"assemblyOptions": []
				}
			]
		},
		"hooksData": null,
		"ratesAndBenefitsData": {
			"rateAndBenefitsIdentifiers": [
				{
					"id": "a0080acf-c2bb-4dfa-a449-a1e759baa534",
					"name": "Frete Máximo",
					"featured": false,
					"description": "Promoção pra evitar cobranças de frete exorbitantes devido a inconsistências nas planilhas de frete.",
					"matchedParameters": {
						"slaIds": "econômica,normal"
					},
					"additionalInfo": null
				}
			],
			"teaser": []
		},
		"subscriptionData": null,
		"itemsOrdination": null,
		"recaptchaKey": "6LdAUOAUAAAAAG8GfSFnEdE2t6Hwq4wnIFzJKa4e"
	}
}'
```
