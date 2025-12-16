# Global Party System API

API System pour la gestion des parties (clients) dans Global Data via Supabase.

## Description

Cette API fournit un accès standardisé aux parties (clients individuels et organisations) dans le service Global Data. Elle gère également les identifiants externes pour la liaison entre systèmes.

## Endpoints

### GET /api/global/parties
Récupère la liste des parties avec filtres.

**Query Parameters:**
- `globalId` (optional): ID global unique
- `partyType` (optional): Type de partie (Individual, Organization)
- `lastName` (optional): Nom de famille
- `limit` (optional, default: 100)
- `offset` (optional, default: 0)

### POST /api/global/parties
Upsert d'une partie (création ou mise à jour).

### GET /api/global/parties/{partyGlobalId}
Récupère une partie par son Global ID (inclut les identifiants externes).

### POST /api/global/parties/lookup
Recherche une partie par identifiant externe.

**Body:**
```json
{
  "externalId": {
    "system": "CoreBanking",
    "value": "CUST001"
  }
}
```

## Configuration

Voir README.md de Core Banking Customers System API pour la configuration de base.

### Tables Utilisées

- `global_parties`: Table principale des parties
- `external_ids`: Table de liaison pour les identifiants externes

## Architecture Technique

### Flows Business-Logic

- `get-parties-business-logic`: Liste des parties avec external IDs
- `upsert-party-business-logic`: Upsert avec gestion des external IDs
- `get-party-by-global-id-business-logic`: Récupération par Global ID
- `lookup-party-business-logic`: Recherche par external ID

## Exemples de Requêtes

### POST /api/global/parties (Upsert)

```bash
curl -X POST http://localhost:8081/api/global/parties \
  -H "Content-Type: application/json" \
  -d '{
    "globalId": "550e8400-e29b-41d4-a716-446655440000",
    "partyType": "Individual",
    "firstName": "John",
    "lastName": "Doe",
    "taxId": "123-45-6789",
    "externalIds": [{
      "system": "CoreBanking",
      "value": "CUST001"
    }, {
      "system": "Salesforce",
      "value": "003xx000004TmiQAAS"
    }]
  }'
```

### POST /api/global/parties/lookup

```bash
curl -X POST http://localhost:8081/api/global/parties/lookup \
  -H "Content-Type: application/json" \
  -d '{
    "externalId": {
      "system": "CoreBanking",
      "value": "CUST001"
    }
  }'
```

