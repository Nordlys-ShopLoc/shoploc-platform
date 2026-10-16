````mermaid
%% Schéma des services et de leurs liens — ShopLoc
%% Notation : boîtes et flèches, inspirée des diagrammes de composants UML
%% Montre le découpage interne, les bases et les interfaces externes

flowchart TB
    FRONT["<b>frontend</b><br/>React<br/><i>application web responsive</i>"]
    GW["<b>api-gateway</b><br/>Spring Cloud Gateway<br/><i>point d'entrée unique, routage</i>"]

    subgraph SERVICES["Microservices — Spring Boot"]
        direction LR
        ACC["<b>service-accounts</b><br/>inscription, authentification<br/>carte, porte-monnaie"]
        MER["<b>service-merchants</b><br/>commerces, catalogue<br/>click and collect"]
        LOY["<b>service-loyalty</b><br/>points, cadeaux<br/>statut VFP, avantages"]
        ANA["<b>service-analytics</b><br/>indicateurs, notifications<br/>administration multi-villes"]
    end

    DBA[("PostgreSQL<br/>accounts")]
    DBM[("PostgreSQL<br/>merchants")]
    DBL[("PostgreSQL<br/>loyalty")]
    DBN[("PostgreSQL<br/>analytics")]

    subgraph EXT["Services partenaires (simulés)"]
        direction LR
        E1["Établissement<br/>bancaire"]
        E2["Transport et<br/>stationnement"]
        E3["Envoi<br/>d'e-mails"]
    end

    FRONT -->|"REST / JSON"| GW
    GW -->|"REST"| ACC
    GW -->|"REST"| MER
    GW -->|"REST"| LOY
    GW -->|"REST"| ANA

    ACC --- DBA
    MER --- DBM
    LOY --- DBL
    ANA --- DBN

    MER ==>|"vérifie la carte,<br/>débite le porte-monnaie"| ACC
    MER ==>|"signale un achat payé"| LOY
    ANA -.->|"lecture périodique"| ACC
    ANA -.->|"lecture périodique"| MER
    ANA -.->|"lecture périodique"| LOY

    ACC -.-> E1
    LOY -.-> E2
    ANA -.-> E3

    classDef front fill:#FFFFFF,stroke:#15203B,stroke-width:1.5px,color:#15203B
    classDef gateway fill:#2FD4EE,stroke:#15203B,stroke-width:2px,color:#15203B
    classDef svc fill:#43E0A6,stroke:#15203B,stroke-width:1.5px,color:#15203B
    classDef db fill:#FFF3D6,stroke:#B8860B,stroke-width:1.5px,color:#15203B
    classDef ext fill:#E8ECF4,stroke:#5B6788,stroke-width:1.5px,stroke-dasharray:5 3,color:#15203B
    class FRONT front
    class GW gateway
    class ACC,MER,LOY,ANA svc
    class DBA,DBM,DBL,DBN db
    class E1,E2,E3 ext
````
