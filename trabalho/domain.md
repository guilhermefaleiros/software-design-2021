# EVENTOS DE DOMÍNIO

## ON_STOCK_UPDATED

Descrição: realizado quando algum remédio tem seu estoque atualizado para determinada localização.

```
{
    "type": "INCOME" | "OUTCOME",
    "drugId": "UUID",
    "locationId":  "UUID",
    "value": "number"
}
```

## ON_DRUG_REMOVED

Descrição: realizado quando algum remédio é removido do catálogo para determinada localização.

```
{
    "drugId": "UUID",
    "locationId":  "UUID"
}
```

## ON_DRUG_ADDED

Descrição: realizado quando algum remédio é adicionado do catálogo de uma determinada localização.

```
{
    "drugId": "UUID",
    "locationId":  "string",
    "initialQuantity: "number"
}
```

# MODELAGEM - BANCO DE LEITURA

Esta modelagem referente ao banco de leitura, que terá os dados persistidos de maneira conveniente para busca de medicamentos e suas informações necessárias.

```
{
    "stock": [
        {
            "locationId": "string",
            "drugId": "string",
            "quantity": "number"
        }
    ],
    "atc": [
        {
            "id": "UUID",
            "code": "string",
            "name": "string",
            "therapeuticGroups": [
                {
                    "id": "UUID",
                    "name": "string",
                    "pharmacologicalGroups": [
                        {
                            "id": "UUID",
                            "name": "string",
                            "drugs": [
                                "DRUG_ID_1",
                                "DRUG_ID_2",
                                "DRUG_ID_3"
                            ]
                        }
                    ]
                }
            ]
        }
    ],
    "cities": [
        {
            "id": "UUID",
            "name": "string"
        }
    ],
    "drugs": [
        {
            "id": "UUID",
            "genericName": "string",
            "activeIngredients": [
                {
                    "id": "UUID",
                    "name": "string"
                }
            ],
            "locations": [
                {
                    "id": "UUID",
                    "name": "string",
                    "cityId": "UUID",
                    "latitude": "string",
                    "longitude": "string"
                }
            ],
            "documentation": {
                "requiredToRequest": "string",
                "requiredToDistributionPoint": "string"
            }
        }
    ]
}
```

# MODELAGEM CACHE

A seguinte representa os dados que serão persistidos em cache para acesso rápido.

Tempo de expiração para cada chave:
* ```list.act => 1 mês```
* ```list.therapeutic-groups#{actId} => 1 semana```
* ```list.pharmacological-groups#{therapeuticGroupId} => 1 semana```
* ```list.drugs#{pharmacologicalGroupId} => 1 semana```

```
{
    "list.act": [
        {
            "id": "string",
            "name": "string"
        }
    ],
    "list.therapeutic-groups#{actId}": [
        {
            "id": "string",
            "name": "string"
        }
    ],
    "list.pharmacological-groups#{therapeuticGroupId}": [
        {
            "id": "string",
            "name": "string"
        }
    ],
    "list.drugs#{pharmacologicalGroupId}": [
        {
            "id": "UUID",
            "genericName": "string",
            "activeIngredients": [
                {
                    "id": "UUID",
                    "name": "string"
                }
            ],
            "locationPoints": [
                {
                    "id": "UUID",
                    "name": "string",
                    "latitude": "string",
                    "longitude": "string",
                }
            ],
            "documentation": {
                "requiredToRequest": "string",
                "requiredToDistributionPoint": "string"
            }
        }
    ]
}
```