# shoploc-platform
Point d’entrée du projet — architecture, schémas et orchestration Docker

## Démarrer la plateforme

Prérequis : Docker et Docker Compose.

Les dépôts des services doivent être clonés à côté de celui-ci :

```
ShopLoc/
├── shoploc-platform/
├── service-merchants/
├── api-gateway/
└── frontend/
```

Puis, depuis ce dépôt :

```bash
docker compose up --build
```

L'application est accessible sur http://localhost

## Composants

| Service | Rôle | Accès |
|---|---|---|
| frontend | Application web React | port 80 |
| api-gateway | Point d'entrée unique, Spring Cloud Gateway | port 8080 |
| service-merchants | Commerces, catalogue, click and collect | réseau interne |
| merchants-db | Base PostgreSQL de service-merchants | réseau interne |

## Arrêter la plateforme

```bash
docker compose down
```

## Documentation

Les schémas d'architecture sont dans `docs/diagrams/`, écrits en Mermaid.