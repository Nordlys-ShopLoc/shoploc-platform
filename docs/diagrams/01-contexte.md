````mermaid
%% Schéma de contexte — ShopLoc
%% Notation : boîtes et flèches, inspirée des diagrammes de composants UML
%% Montre les acteurs du système, le système et les services partenaires simulés

flowchart TB
    subgraph ACTEURS["Utilisateurs"]
        direction LR
        A1["Client<br/><i>Pierre, Julie, Arthur</i><br/>achète, cumule des points"]
        A2["Commerçant partenaire<br/><i>Suzanne</i><br/>gère son catalogue"]
        A3["Administrateur collectivité<br/><i>Marius</i><br/>suit les indicateurs"]
        A4["Policier municipal<br/>contrôle le stationnement"]
        A5["Administrateur ShopLoc<br/>exploite la plateforme"]
    end

    SYS["<b>ShopLoc</b><br/>Plateforme de commerce local<br/>click and collect et fidélité<br/><i>architecture microservices</i>"]

    subgraph PARTENAIRES["Services partenaires (simulés)"]
        direction LR
        P1["Établissement bancaire<br/>paiement, rechargement"]
        P2["Réseau de transport<br/>titres offerts"]
        P3["Stationnement municipal<br/>minutes offertes"]
        P4["Envoi d'e-mails<br/>notifications et relances"]
    end

    A1 -->|"HTTPS"| SYS
    A2 -->|"HTTPS"| SYS
    A3 -->|"HTTPS"| SYS
    A4 -->|"HTTPS"| SYS
    A5 -->|"HTTPS"| SYS

    SYS -.->|"Interface simulée"| P1
    SYS -.->|"Interface simulée"| P2
    SYS -.->|"Interface simulée"| P3
    SYS -.->|"Interface simulée"| P4

    classDef acteur fill:#43E0A6,stroke:#15203B,stroke-width:1.5px,color:#15203B
    classDef systeme fill:#2FD4EE,stroke:#15203B,stroke-width:2.5px,color:#15203B
    classDef partenaire fill:#E8ECF4,stroke:#5B6788,stroke-width:1.5px,stroke-dasharray:5 3,color:#15203B
    class A1,A2,A3,A4,A5 acteur
    class SYS systeme
    class P1,P2,P3,P4 partenaire
````
