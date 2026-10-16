````mermaid
%% Schéma de la chaîne d'outils — ShopLoc
%% Notation : schéma de flux, du commit au déploiement en conteneurs
%% Reprend l'organisation décrite en 1.5

flowchart LR
    subgraph DEV["Développement"]
        direction TB
        D1["Branche feature/*<br/><i>une user story</i>"]
        D2["Pull request<br/>vers develop"]
    end

    subgraph CI["Intégration continue — GitHub Actions"]
        direction TB
        C1["Build<br/><i>Maven ou npm</i>"]
        C2["Tests<br/><i>JUnit ou équivalent</i>"]
        C3["Analyse qualité<br/>SonarQube"]
    end

    subgraph CD["Livraison"]
        direction TB
        L1["Fusion sur<br/>develop"]
        L2["Construction de<br/>l'image Docker"]
        L3["Publication<br/>GitHub Container Registry"]
    end

    subgraph RUN["Déploiement — VM OpenStack"]
        direction TB
        R1["docker-compose<br/><i>shoploc-platform</i>"]
        R2["4 services + 4 bases<br/>passerelle + frontend"]
    end

    D1 --> D2
    D2 --> C1
    C1 --> C2
    C2 --> C3
    C3 -->|"relecture validée<br/>et pipeline vert"| L1
    L1 --> L2
    L2 --> L3
    L3 --> R1
    R1 --> R2

    C3 -.->|"échec : la fusion<br/>est bloquée"| D1

    classDef dev fill:#FFFFFF,stroke:#15203B,stroke-width:1.5px,color:#15203B
    classDef ci fill:#2FD4EE,stroke:#15203B,stroke-width:1.5px,color:#15203B
    classDef cd fill:#43E0A6,stroke:#15203B,stroke-width:1.5px,color:#15203B
    classDef run fill:#FFF3D6,stroke:#B8860B,stroke-width:1.5px,color:#15203B
    class D1,D2 dev
    class C1,C2,C3 ci
    class L1,L2,L3 cd
    class R1,R2 run
````
