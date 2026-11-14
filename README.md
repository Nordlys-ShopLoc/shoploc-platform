# shoploc-platform

Point d'entrée du projet ShopLoc : orchestration Docker de la plateforme, architecture et schémas.

## Démarrer la plateforme

Prérequis : Docker et Docker Compose.

Les dépôts doivent être clonés côte à côte :

```
ShopLoc/
├── shoploc-platform/
├── service-accounts/
├── service-merchants/
├── api-gateway/
└── frontend/
```

Créer le fichier des secrets, une seule fois :

```bash
cp .env.example .env
```

puis y renseigner les mots de passe des bases. Ce fichier n'est jamais commité.

Lancer la plateforme :

```bash
docker compose up -d --build
```

L'application est accessible sur http://localhost, et l'API sur http://localhost:8080.

## Composants

| Service | Rôle | Accès |
|---|---|---|
| frontend | Application web React | port 80 |
| api-gateway | Point d'entrée unique de l'API | port 8080 |
| service-accounts | Comptes, authentification, cartes de fidélité | réseau interne |
| accounts-db | Base PostgreSQL de service-accounts | réseau interne |
| service-merchants | Commerces, catalogue, click and collect | réseau interne |
| merchants-db | Base PostgreSQL de service-merchants | réseau interne |

Chaque service possède sa propre base. Seuls le frontend et la gateway sont exposés à l'extérieur.

## Vérifier que tout fonctionne

```bash
docker compose ps
curl http://localhost:8080/api/v1/shops
```

La première commande liste les conteneurs ; la seconde doit renvoyer la liste des commerces en JSON. Les services Java mettent une dizaine de secondes à démarrer.

## Arrêter la plateforme

```bash
docker compose down       # arrête les conteneurs, conserve les données
docker compose down -v    # supprime aussi les bases
```

## Configuration

| Variable | Rôle |
|---|---|
| `MERCHANTS_DB_PASSWORD` | mot de passe de merchants-db |
| `ACCOUNTS_DB_PASSWORD` | mot de passe de accounts-db |
| `JWT_SECRET` | clé de signature des jetons de connexion, 32 caractères minimum |

PostgreSQL ne lit le mot de passe qu'à la création d'une base. S'il change ensuite, il faut recréer les bases avec `docker compose down -v`.

## Documentation

Les schémas d'architecture sont dans `docs/diagrams/`, écrits en Mermaid :

- `01-contexte.md` — le système et ses acteurs
- `02-services.md` — le découpage en microservices
- `03-chaine-outils.md` — la chaîne qui mène du commit au déploiement
