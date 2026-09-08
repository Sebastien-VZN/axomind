# Security

Axomind is designed with a "security by default" philosophy. The architecture prioritizes data confidentiality, integrity, and availability for both the desktop and mobile applications. As a self-hosted solution protected by Cloudflare Tunnels, Axomind eliminates traditional network exposure while enforcing strict application-level controls.

## Network Architecture & Perimeter Defense

Axomind operates on a zero-trust network model where no internal services are directly exposed to the public internet.

| Control Measure | Implementation Detail |
| :--- | :--- |
| **Network Exposure** | Zero open inbound ports. All external traffic flows exclusively through outbound-only Cloudflare Tunnels. |
| **Traffic Filtering** | Strict firewall rules (default deny) allow only authorized tunnel traffic. Database, cache, and administrative interfaces are bound strictly to localhost. |
| **DDoS & Bot Mitigation** | Enterprise-grade protection managed by Cloudflare, including automatic TLS termination and malicious bot filtering before traffic reaches your infrastructure. |
| **Remote Access** | Administrative access (SSH) is restricted to the local network only, requiring key-based authentication and protected by automated intrusion prevention systems. |

## Application Security Controls

The software includes built-in mechanisms to prevent unauthorized access and data leakage without requiring complex configuration.

*   **Silent Threat Neutralization**: The system employs an active anti-brute-force engine. Invalid requests receive generic, non-informative responses. IP addresses exhibiting malicious behavior are automatically and permanently banned after a minimal number of failed attempts.
*   **Cryptographic Data Protection**: All authentication tokens and sensitive session data are encrypted at rest using industry-standard AES-256-GCM encryption before being stored.
*   **Session Management**: Strict time-to-live (TTL) policies are enforced for all sessions, WebSocket connections, and password reset links to minimize the window of opportunity for misuse.
*   **Upload Safety**: File uploads undergo rigorous validation, including file type verification, magic byte analysis, and automated antivirus scanning prior to processing.
*   **Credential Hygiene**: Sensitive credentials are automatically sanitized and stripped from memory before any response is transmitted to the client.

## Real-Time Communication (WebSocket)

Real-time features are secured through a multi-layered authentication handshake.

*   Connections are only established after successful authentication and session verification.
*   Internal APIs used by WebSocket services are accessible solely via localhost, preventing external interception.
*   An automated heartbeat mechanism detects and immediately terminates inactive or "zombie" connections to preserve resources and security.

## Data Integrity & Storage

*   **Database Security**: PostgreSQL is used with column-level encryption for all sensitive data fields.
*   **Cache Isolation**: The Redis caching layer is strictly isolated and accessible only to local processes, ensuring no data leakage via cache channels.
*   **Dependency Hygiene**: All third-party libraries and dependencies are continuously monitored and verified to ensure no known vulnerabilities (CVEs) are present in the build chain.

## Why Not Web?

Axomind is deliberately engineered as a **native desktop and mobile application**, not a web solution. This architectural decision is driven by two non-negotiable pillars: **security integrity** and **real-time performance**. While web applications offer accessibility, they introduce inherent risks and latency bottlenecks that are incompatible with Axomind's requirements for secure, collaborative intelligence.

### 1. Security: Eliminating the Client-Side Attack Surface

Web applications inherently expose their logic to the end-user. Axomind rejects this model to ensure robust security.

| Feature | Traditional Web App | Axomind (Native) | Security Impact |
| :--- | :--- | :--- | :--- |
| **Code Visibility** | JavaScript source is fully visible, auditable, and reversible by anyone. | Compiled to native code (Dart AOT). Harder to reverse-engineer than JavaScript. | Significantly raises the bar for reverse engineering of business logic and client-side tampering. |
| **Logic Location** | Significant business logic often resides or is mirrored on the client. | 100% of business logic remains on the secured server; client is a pure renderer. | Removes the risk of client-side logic manipulation or injection. |
| **Supply Chain** | Heavy reliance on npm packages creates a vast attack surface (cascade vulnerabilities). | Minimalist dependency tree. Critical components use standard ES6 with zero external bloat. | Drastically reduces the risk of supply chain attacks via compromised packages. |

**The Node.js Strategy:**

While the real-time WebSocket server utilizes Node.js for its efficiency, its dependency tree is strictly limited. Unlike typical web stacks relying on hundreds of external npm packages, the WebSocket layer is built on **pure standard JavaScript (ES6)** with only three essential, audited dependencies (`ws`, `dotenv`, `axios`). The entire communication layer is small enough to be fully audited by a single person in hours, ensuring total control over the code executing on the infrastructure.

### 2. Performance: The Limits of the Browser

Axomind handles complex, collaborative data structures (dynamic mind maps, Gantt charts, real-time messaging) requiring sub-16ms latency for fluid interaction. Web browsers cannot meet these demands due to architectural overhead.

*   **Rendering Engine Overhead:** Browsers must parse HTML/CSS, manage the DOM tree, and handle JavaScript garbage collection. These layers introduce unpredictable latency spikes ("jank") during heavy updates.
*   **Direct Hardware Access:** As a native application (Flutter/Dart), Axomind bypasses the DOM entirely. It renders directly to the hardware canvas using the GPU.
*   **Scalability:** Rendering hundreds of interconnected nodes with live data streams causes significant degradation in a browser environment. The native engine maintains consistent 60 FPS performance regardless of map complexity.

**Conclusion:** Choosing a web interface would compromise the confidentiality of your data through exposed code and degrade the user experience through unavoidable browser latency. Axomind's native architecture ensures that your intellectual property remains protected and your workflow remains fluid.

---

## Compliance & Auditing

Axomind undergoes a rigorous cycle of recurring security audits to validate its defensive posture.

*   **Regular Audits**: Comprehensive in-house audits are conducted on a recurring basis using industry-standard penetration testing tools and methodologies.
*   **Latest Assessment**: The most recent full-scope audit was completed in **August 2026**.
*   **Findings**: These audits confirmed the absence of critical vulnerabilities within the application logic and infrastructure configuration.
*   **Performance Validation**: Load testing and stress tests are regularly performed to ensure system stability and resilience under high demand.

---

<details>
<summary><strong>🇫🇷 Version Française (French Version)</strong></summary>

# Sécurité

Axomind est conçu selon une philosophie de « sécurité par défaut ». L'architecture priorise la confidentialité, l'intégrité et la disponibilité des données pour les applications bureau et mobiles. En tant que solution auto-hébergée protégée par des tunnels Cloudflare, Axomind élimine l'exposition réseau traditionnelle tout en appliquant des contrôles stricts au niveau de l'application.

## Architecture Réseau et Défense Périmétrique

Axomind fonctionne sur un modèle de confiance zéro où aucun service interne n'est directement exposé à l'internet public.

| Mesure de Contrôle | Détail d'Implémentation |
| :--- | :--- |
| **Exposition Réseau** | Zéro port entrant ouvert. Tout le trafic externe transite exclusivement via des tunnels Cloudflare en sortie seule (outbound-only). |
| **Filtrage du Trafic** | Règles de pare-feu strictes (rejet par défaut) n'autorisant que le trafic de tunnel autorisé. Les bases de données, caches et interfaces d'administration sont liées strictement au localhost. |
| **Atténuation DDoS & Bots** | Protection de niveau entreprise gérée par Cloudflare, incluant la terminaison TLS automatique et le filtrage des bots malveillants avant que le trafic n'atteigne votre infrastructure. |
| **Accès à Distance** | L'accès administratif (SSH) est restreint au réseau local uniquement, nécessitant une authentification par clé et protégé par des systèmes automatisés de prévention d'intrusion. |

## Contrôles de Sécurité Applicative

Le logiciel intègre des mécanismes natifs pour prévenir les accès non autorisés et les fuites de données sans configuration complexe.

*   **Neutralisation Silencieuse des Menaces**: Le système emploie un moteur anti-brute-force actif. Les requêtes invalides reçoivent des réponses génériques et non informatives. Les adresses IP présentant un comportement malveillant sont automatiquement et définitivement bannies après un nombre minimal de tentatives échouées.
*   **Protection Cryptographique des Données**: Tous les jetons d'authentification et les données de session sensibles sont chiffrés au repos utilisant le standard industriel AES-256-GCM avant stockage.
*   **Gestion de Session**: Des politiques strictes de durée de vie (TTL) sont appliquées à toutes les sessions, connexions WebSocket et liens de réinitialisation de mot de passe pour minimiser la fenêtre d'opportunité d'utilisation abusive.
*   **Sécurité des Téléchargements**: Les fichiers uploadés subissent une validation rigoureuse, incluant la vérification du type de fichier, l'analyse des « magic bytes » et un scan antivirus automatisé avant traitement.
*   **Hygiène des Identifiants**: Les identifiants sensibles sont automatiquement assainis et supprimés de la mémoire avant qu'une réponse ne soit transmise au client.

## Communication en Temps Réel (WebSocket)

Les fonctionnalités en temps réel sont sécurisées via une poignée de main d'authentification multi-couches.

*   Les connexions ne sont établies qu'après une authentification réussie et une vérification de session.
*   Les API internes utilisées par les services WebSocket sont accessibles uniquement via localhost, empêchant toute interception externe.
*   Un mécanisme de « heartbeat » (battement de cœur) automatisé détecte et termine immédiatement les connexions inactives ou « zombies » pour préserver les ressources et la sécurité.

## Intégrité et Stockage des Données

*   **Sécurité de la Base de Données**: PostgreSQL est utilisé avec un chiffrement au niveau des colonnes pour tous les champs de données sensibles.
*   **Isolement du Cache**: La couche de mise en cache Redis est strictement isolée et accessible uniquement aux processus locaux, garantissant aucune fuite de données via les canaux de cache.
*   **Hygiène des Dépendances**: Toutes les bibliothèques tierces et dépendances sont continuellement surveillées et vérifiées pour garantir qu'aucune vulnérabilité connue (CVE) n'est présente dans la chaîne de construction.

## Pourquoi Pas Web ?

Axomind est délibérément conçue comme une **application native pour bureau et mobile**, et non comme une solution web. Cette décision architecturale repose sur deux piliers non négociables : **l'intégrité de la sécurité** et **la performance temps réel**. Bien que les applications web offrent une certaine accessibilité, elles introduisent des risques inhérents et des goulots d'étranglement incompatibles avec les exigences d'Axomind en matière de sécurité et de collaboration fluide.

### 1. Sécurité : Élimination de la surface d'attaque côté client

Les applications web exposent intrinsèquement leur logique à l'utilisateur final. Axomind rejette ce modèle pour garantir une sécurité robuste.

| Fonctionnalité | Application Web Classique | Axomind (Natif) | Impact Sécuritaire |
| :--- | :--- | :--- | :--- |
| **Visibilité du Code** | Le code JavaScript est entièrement visible, auditable et réversible par quiconque. | Compilé en code natif (Dart AOT). Plus difficile à rétro-ingénierier que le JavaScript. | Augmente significativement la difficulté de rétro-ingénierie de la logique métier et de la falsification côté client. |
| **Localisation de la Logique** | Une partie significative de la logique métier réside ou est dupliquée chez le client. | 100 % de la logique métier reste sur le serveur sécurisé ; le client est un pur moteur de rendu. | Supprime le risque de manipulation de la logique ou d'injection côté client. |
| **Chaîne d'Approvisionnement** | Forte dépendance aux paquets npm créant une vaste surface d'attaque (vulnérabilités en cascade). | Arbre de dépendances minimaliste. Les composants critiques utilisent du JS standard sans surcharge externe. | Réduit drastiquement le risque d'attaques via des paquets compromis (supply chain attacks). |

**La Stratégie Node.js :**

Bien que le serveur WebSocket temps réel utilise Node.js pour son efficacité, son arbre de dépendances est strictement limité. Contrairement aux stacks web typiques s'appuyant sur des centaines de paquets npm externes, la couche WebSocket est bâtie sur du **JavaScript standard pur (ES6)** avec seulement trois dépendances essentielles et auditées (`ws`, `dotenv`, `axios`). L'intégralité de la couche de communication est suffisamment compacte pour être auditée par une seule personne en quelques heures, assurant un contrôle total du code exécuté sur l'infrastructure.

### 2. Performance : Les limites du navigateur

Axomind gère des structures de données collaboratives complexes (cartes mentales dynamiques, diagrammes de Gantt, messagerie instantanée) nécessitant une latence inférieure à 16 ms pour une interaction fluide. Les navigateurs web ne peuvent répondre à ces exigences en raison de leur surcharge architecturale.

*   **Surcharge du Moteur de Rendu :** Les navigateurs doivent parser le HTML/CSS, gérer l'arbre DOM et effectuer le garbage collection JavaScript. Ces couches introduisent des pics de latence imprévisibles lors de mises à jour intensives.
*   **Accès Direct au Matériel :** En tant qu'application native (Flutter/Dart), Axomind contourne entièrement le DOM. Elle rend directement sur le canvas matériel via le GPU.
*   **Évolutivité :** Le rendu de centaines de nœuds interconnectés avec des flux de données vives provoque une dégradation significative dans un environnement navigateur. Le moteur natif maintient des performances constantes à 60 FPS quelle que soit la complexité de la carte.

**Conclusion :** Choisir une interface web compromettrait la confidentialité de vos données via un code exposé et dégraderait l'expérience utilisateur par une latence navigateur inévitable. L'architecture native d'Axomind garantit que votre propriété intellectuelle reste protégée et que votre flux de travail demeure fluide.

## Conformité et Audit

Axomind suit un cycle rigoureux d'audits de sécurité récurrents pour valider sa posture défensive.

*   **Audits Réguliers**: Des audits complets sont menés en interne de manière récurrente à l'aide d'outils et de méthodologies standard de l'industrie en matière de test d'intrusion.
*   **Dernière Évaluation**: Le dernier audit complet a été achevé en **août 2026**.
*   **Constats**: Ces audits ont confirmé l'absence de vulnérabilités critiques au sein de la logique applicative et de la configuration de l'infrastructure.
*   **Validation de Performance**: Des tests de charge et de résistance sont régulièrement effectués pour assurer la stabilité du système et sa résilience sous forte demande.

</details>