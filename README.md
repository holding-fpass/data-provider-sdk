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

### Exemplo 1: user.interaction type view
Evento de interação do usuário que assistiu o conteúdo de um curso referente ao início do video no segundo 50 e termino no segundo 60.

```json
{
  "eventId": "ca542dca-3649-4ca4-8142-8f1761fa822f",
  "eventDate": "2022-08-29T19:13:13.783Z",
  "eventType": "user.interaction",
  "resourceId": "cc070573-fc2e-4dd2-bacb-0ac7eac655db",
  "resourceType": "interaction",
  // Identificador externo enviado pelo partner
  "ownerExternalId": "2e16f9d7-cb32-43fd-ade2-93a5ecea460a",
  // Dados de classificação do usuário feita pelo partner
  "ownerData": {
    "tags": [
      "NivelABC",
      "GrupoXPTO",
    ]
  },
  "data": {
    // Dados do evento do tipo interaction
    "ownerId": "5g16f9d7-cb32-43fd-ade2-93a5ecea467a",
    "ownerWhitelabel": "company-xpto",
    "resourceId": "cc070573-fc2e-4dd2-bacb-0ac7eac655db",
    "resourceType": "interaction",
    "type": "view",
    "parentId": "8b46be82-d8f6-4b62-86cb-926f4865e9df",
    "parentType": "course",
    "productId": "jj070573-fc2e-4dd2-bacb-0ac7eac655ca",
    "productType": "content",
    "status": "created",
    "timestamp": "2022-08-29T19:13:13.783Z",
    "whitelabel": "company-xpto",
    // Dados extras específicos do evento interaction tipo view
    "mediaStart": 50,
    "mediaEnd": 60,
    "mediaCount": 60,
  }
}
```

### Exemplo 2: user.interaction type leave
Evento de interação do usuário saiu do player que estava assistindo determinado conteúdo no segundo 60.

```json
{
  "eventId": "ca542dca-3649-4ca4-8142-8f1761fa822f",
  "eventDate": "2022-08-29T19:13:13.783Z",
  "eventType": "user.interaction",
  "resourceId": "cc070573-fc2e-4dd2-bacb-0ac7eac655db",
  "resourceType": "interaction",
  // Identificador externo enviado pelo partner
  "ownerExternalId": "2e16f9d7-cb32-43fd-ade2-93a5ecea460a",
  // Dados de classificação do usuário feita pelo partner
  "ownerData": {
    "tags": [
      "NivelABC",
      "GrupoXPTO",
    ]
  },
  "data": {
    // Dados do evento do tipo interaction
    "ownerId": "5g16f9d7-cb32-43fd-ade2-93a5ecea467a",
    "ownerWhitelabel": "company-xpto",
    "resourceId": "cc070573-fc2e-4dd2-bacb-0ac7eac655db",
    "resourceType": "interaction",
    "type": "leave",
    "parentId": "8b46be82-d8f6-4b62-86cb-926f4865e9df",
    "parentType": "course",
    "productId": "jj070573-fc2e-4dd2-bacb-0ac7eac655ca",
    "productType": "content",
    "status": "created",
    "timestamp": "2022-08-29T19:13:13.783Z",
    "whitelabel": "company-xpto",
    // Dados extras específicos do evento interaction tipo leave
    "mediaEnd": 60,
  }
}
```

### Exemplo 3: user.interaction type open
Evento de interação do usuário abriu o player para assistir determinado conteúdo.

```json
{
  "eventId": "ca542dca-3649-4ca4-8142-8f1761fa822f",
  "eventDate": "2022-08-29T19:13:13.783Z",
  "eventType": "user.interaction",
  "resourceId": "cc070573-fc2e-4dd2-bacb-0ac7eac655db",
  "resourceType": "interaction",
  // Identificador externo enviado pelo partner
  "ownerExternalId": "2e16f9d7-cb32-43fd-ade2-93a5ecea460a",
  // Dados de classificação do usuário feita pelo partner
  "ownerData": {
    "tags": [
      "NivelABC",
      "GrupoXPTO",
    ]
  },
  "data": {
    // Dados do evento do tipo interaction
    "ownerId": "5g16f9d7-cb32-43fd-ade2-93a5ecea467a",
    "ownerWhitelabel": "company-xpto",
    "resourceId": "cc070573-fc2e-4dd2-bacb-0ac7eac655db",
    "resourceType": "interaction",
    "type": "open",
    "parentId": "8b46be82-d8f6-4b62-86cb-926f4865e9df",
    "parentType": "course",
    "productId": "jj070573-fc2e-4dd2-bacb-0ac7eac655ca",
    "productType": "content",
    "status": "created",
    "timestamp": "2022-08-29T19:13:13.783Z",
    "whitelabel": "company-xpto",
  }
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

- Sucesso: 200 ou 201
- Falha: demais

### MongoDB

Por meio de string de conexão informada pelo _Partner_, a aplicação _FPASS_ faz a inserção de dados na base do _Partner_.

- Ocorre a cada interação feita pelo usuário na aplicação _FPASS_
- A confirmação do recebimento se dá pelo sucesso da operação.

### Google Cloud Storage

Por meio de dados informados pelo _Partner_, a aplicação _FPASS_ faz a persistencia de arquivo JSON gerado no bucket do _Partner_.

- Ocorre a cada interação feita pelo usuário na aplicação _FPASS_
- A confirmação do recebimento se dá pelo sucesso da operação.
