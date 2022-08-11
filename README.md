# DATA FPASS

Definições para integração de dados com sistemas da plataforma FPASS.

## Responsability

Garantir a entrega de dados entre sistemas entre parceiros (Partners) e plataforma (FPASS).

## Overview

![Overview](https://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.githubusercontent.com/holding-fpass/data-provider-sdk/main/uml/data-overview-v2.0.0.iuml)

## Evento

Cada evento é criado mediante uma interação do usuário na plataforma _FPASS_. Segue a seguinte entrutura:

```json
{
  "eventId": "{eventId}",
  "eventDate": "{isoDate}",
  "eventType": "{eventType}",
  "resourceId": "{uuid}",
  "resourceType": "{resourceType}",
  "parentId": "{parentId}",
  "parentType": "{parentType}",
  "ownerExternalId": "{ownerExternalId}",
  "data": "{data}"
}
```

### Definições

- eventId: Identificação única em formato UUIDv4
- eventDate: Iso date
- eventType: Enumerador de tipos
- resourceId: Identificação da entidade
- resourceType: Tipo da entidade
- parentId: Identificação da entidade pai
- parentType: Tipo da entidade pai
- ownerExternalId: Identificação externa do usuário
- data: Dados adcionais (a depender do tipo de evento)

## Data forwarding

O encaminhamento dos eventos de dados se dá ao _Partner_ pelas seguintes formas abaixo segundo a configuração feita na instancia da aplicação _FPASS_.

### Webhook

Por meio de url informada pelo _Partner_, a aplicação _FPASS_ faz o envio por postagem de dados.

- Ocorre a cada interação feita pelo usuário na aplicação _FPASS_
- A confirmação do recebimento se dá pelos códigos HTTP recebidos de sucesso [200, 201] ou falha [demais].

Request (POST)

```bash
curl --location -g --request POST 'https://api.{partner}.com/jwt' \
--header 'Content-Type: application/json' \
--data-raw '{
  "eventId": "{eventId}",
  "eventDate": "{isoDate}",
  "eventType": "{eventType}",
  "resourceId": "{uuid}",
  "resourceType": "{resourceType}",
  "parentId": "{parentId}",
  "parentType": "{parentType}",
  "ownerExternalId": "{ownerExternalId}",
  "data": "{data}"
}'
```

Resposta esperada

- Sucesso: 201 ou 201
- Falha: demais

### MongoDB

Por meio de string de conexão informada pelo _Partner_, a aplicação _FPASS_ faz a inserção de dados na base do _Partner_.

- Ocorre a cada interação feita pelo usuário na aplicação _FPASS_
- A confirmação do recebimento se dá pelo sucesso da operação.

### Google Cloud Storage

Por meio de dados informados pelo _Partner_, a aplicação _FPASS_ faz a persistencia de arquivo JSON gerado no bucket do _Partner_.

- Ocorre a cada interação feita pelo usuário na aplicação _FPASS_
- A confirmação do recebimento se dá pelo sucesso da operação.
