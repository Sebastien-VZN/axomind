# Politique de Confidentialité et d'Utilisation — Axomind & Axovox

**Date d'entrée en vigueur :** 24 octobre 2025

La présente Politique de Confidentialité et d'Utilisation régit la manière dont **Axomind** et **Axovox**, développés par Sébastien VEZZANI, collectent, utilisent et protègent les informations des utilisateurs.

Axomind et Axovox partagent la même infrastructure serveur, la même base de données et les mêmes mécanismes de sécurité. La présente politique s'applique indifféremment à обе applications.

L'utilisation de l'une ou l'autre des Applications vaut acceptation pleine et entière de la présente Politique.

---

## 1. Responsable du Traitement

L'Auteur des Applications, Sébastien VEZZANI, agissant en qualité de responsable du traitement, est joignable à l'adresse **sebastien.vezzani.dev@gmail.com** pour toute question relative à cette Politique.

## 2. Données Collectées

Les Applications collectent un minimum strict de données personnelles, nécessaires exclusivement à la création, à la gestion du compte et au bon fonctionnement du service.

### Données d'identification (Fournies par l'Utilisateur)

- **Adresse e-mail** : Il s'agit de la seule donnée obligatoire servant d'identifiant de connexion unique. L'adresse e-mail est utilisée pour l'authentification, la validation du compte à l'inscription et les communications relatives au compte.
- **Nom et Prénom** : Ces informations sont **facultatives**. Lorsqu'elles sont renseignées, elles servent uniquement à l'identification de l'Utilisateur au sein de ses espaces de travail et canaux de discussion.

### Données de Contenu (Générées par l'Utilisateur)

- **Messages, fichiers et clips audio** : Ces données sont générées par l'Utilisateur. Conformément à l'architecture des Applications, l'intégralité de ces contenus est chiffrée au repos sur les serveurs et durant leur transit. L'Auteur n'a pas accès au contenu en clair de ces communications.

### Données techniques et de sécurité

- **Journaux de connexion** (adresse IP, date/heure, type de client) : utilisés exclusivement à des fins de sécurité (détection d'intrusion, anti brute-force, prévention contre les adresses IP malveillantes).
- **Métadonnées d'utilisation agrégées et anonymisées** (fréquence de connexion, fonctionnalités utilisées, volume de données échangées) : utilisées uniquement pour améliorer l'ergonomie, les performances et la stabilité des Applications.

## 3. Finalité du Traitement et Utilisation des Données

Les données collectées sont utilisées exclusivement pour les finalités suivantes :

- **Fourniture du Service** : Permettre la connexion, l'identification et le fonctionnement des fonctionnalités de messagerie temps réel et contextuelle, de planification visuelle et de collaboration.
- **Maintenance et Sécurité** : Assurer la sécurité des comptes, la stabilité de l'infrastructure et la correction de bugs.
- **Analyse d'Usage (Interne)** : L'Auteur se réserve le droit d'analyser les métadonnées d'utilisation de manière agrégée et anonymisée. Cette analyse a pour unique but d'améliorer l'ergonomie, les performances et les fonctionnalités des Applications.

**Engagement formel** : L'Auteur s'engage à ne jamais revendre, louer, céder ou partager les données personnelles des Utilisateurs à des tiers à des fins commerciales ou autres.

## 4. Sécurité des Données

L'Auteur met en œuvre des mesures techniques et organisationnelles robustes pour garantir la sécurité des données :

- **Chiffrement des contenus** : Toutes les communications, messages, fichiers et clips audio sont chiffrés en transit et au repos. L'algorithme utilisé est AES-256-GCM (Galois/Counter Mode) avec Authenticated Associated Data (AAD), garantissant à la fois la confidentialité et l'intégrité des données.
- **Mots de passe** : Les mots de passe sont hachés via l'algorithme Argon2id. L'Auteur n'a jamais accès au mot de passe en clair.
- **Authentification à double facteur (2FA)** : Un code PIN est envoyé par e-mail lors de la validation de l'inscription et de la connexion. Le 2FA peut être activé par l'Auteur pour renforcer la sécurité des comptes.
- **Gestion des sessions** : Chaque session (desktop ou mobile) dispose d'un token unique chiffré en base de données. La durée de validité d'une session est de 24 heures maximum. Au-delà, l'Utilisateur doit se réauthentifier. Le système limite à une session active par type de client (desktop, mobile) : la connexion sur un nouvel appareil ferme automatiquement la session précédente du même type.
- **Récupération de compte** : En cas de perte d'accès, l'Utilisateur peut récupérer son compte via trois clés secrètes générées à l'inscription, associées à un changement d'adresse e-mail. Ce mécanisme est protégé par un token de récupération à validité de 30 minutes.
- **Protection contre les attaques** : Le système implémente un anti brute-force avec rate limiting et bannissement automatique des adresses IP malveillantes.
- **Plateformes** : Axomind et Axovox sont des applications natives desktop et mobile. Les Applications n'existent pas en version web, par choix technique et de sécurité. Le code compilé natif réduit la surface d'attaque comparé à une application web dont le code serait lisible côté navigateur.

## 5. Rétention des Données

Les Applications appliquent des règles de rétention automatiques par type de contenu et par plan d'abonnement. Les données sont automatiquement supprimées à l'issue de leur durée de rétention, sans intervention manuelle.

| Type de contenu | Plan gratuit / Personnel | Plan Pro |
|---|---|---|
| Messages et tombstones | 1 an | 1 an |
| Messages éphémères | 24 heures | 24 heures |
| Fichiers non épinglés (disque) | 1 mois | 3 ans (RGPD) |
| Médias épinglés (base de données) | 3 mois | 3 ans (RGPD) |
| Clips audio | 1 an | 3 ans (RGPD) |
| Journaux d'erreur et liste d'IP en attente | 3 mois | 3 mois |
| Historique d'usage et statistiques | 3 ans (RGPD) | 3 ans (RGPD) |

Le nettoyage est exécuté automatiquement chaque nuit. La suppression est définitive et irréversible.

## 6. Utilisation Illicite, Sanctions et Coopération Judiciaire

L'Utilisateur est le seul responsable du contenu qu'il publie et des communications qu'il établit via les Applications. Il s'engage à n'utiliser les Applications que dans un cadre légal et conforme aux lois en vigueur dans son pays de résidence et en France.

- **Usage Prohibé** : Tout usage des Applications à des fins illégales, incluant, sans s'y limiter, le harcèlement, la diffusion de contenus haineux, l'apologie du terrorisme, la pédopornographie, la violation de la propriété intellectuelle ou l'organisation d'activités criminelles, est formellement interdit.
- **Sanctions** : L'Auteur se réserve le droit de suspendre ou de fermer définitivement, sans préavis ni indemnité, le compte de tout Utilisateur qui contreviendrait aux présentes règles ou qui utiliserait les Applications à des fins manifestement illicites ou malveillantes.
- **Coopération avec les Autorités** : L'Utilisateur est expressément averti que l'Auteur coopérera pleinement avec les autorités judiciaires compétentes. En cas de suspicion avérée ou de réquisition légale concernant un usage illicite des Applications, l'Auteur fournira aux autorités habilitées les informations en sa possession permettant d'identifier l'auteur des infractions (données d'identification du compte, journaux de connexion, métadonnées), dans le strict respect du cadre légal. Le contenu chiffré des communications n'est pas accessible en clair et ne peut être fourni.

## 7. Droits de l'Utilisateur

Conformément à la réglementation applicable (notamment le RGPD dans l'Union Européenne), l'Utilisateur dispose d'un droit d'accès, de rectification, d'opposition et de suppression (droit à l'oubli) des données personnelles le concernant.

Pour exercer ces droits, l'Utilisateur peut adresser une demande claire et justifiée à l'Auteur à l'adresse **sebastien.vezzani.dev@gmail.com**.

## 8. Modification de la Politique

L'Auteur se réserve le droit de modifier la présente Politique à tout moment pour l'adapter aux évolutions des Applications ou aux contraintes légales. Les Utilisateurs seront informés de toute modification substantielle.

---

<details>
<summary>Privacy Policy and Terms of Use — English version</summary>

# Privacy Policy and Terms of Use — Axomind & Axovox

**Effective date:** October 24, 2025

This Privacy Policy and Terms of Use governs how **Axomind** and **Axovox**, developed by Sébastien VEZZANI, collect, use and protect user information.

Axomind and Axovox share the same server infrastructure, database, and security mechanisms. This policy applies equally to both applications.

By using either Application, you fully accept this Policy.

---

## 1. Data Controller

The Author of the Applications, Sébastien VEZZANI, acting as data controller, can be reached at **sebastien.vezzani.dev@gmail.com** for any questions regarding this Policy.

## 2. Data Collected

The Applications collect a strict minimum of personal data, necessary exclusively for account creation, account management, and proper service operation.

### Identification Data (Provided by the User)

- **Email address**: This is the only mandatory data used as a unique login identifier. The email address is used for authentication, account validation at registration, and account-related communications.
- **First and Last Name**: This information is **optional**. When provided, it is used solely for user identification within workspaces and discussion channels.

### Content Data (Generated by the User)

- **Messages, files, and audio clips**: This data is generated by the User. In accordance with the Applications' architecture, all content is encrypted at rest on servers and during transit. The Author does not have access to the plaintext content of these communications.

### Technical and Security Data

- **Connection logs** (IP address, date/time, client type): used exclusively for security purposes (intrusion detection, anti brute-force, prevention against malicious IP addresses).
- **Aggregated and anonymized usage metadata** (connection frequency, features used, data volume exchanged): used solely to improve the ergonomics, performance, and stability of the Applications.

## 3. Purpose of Processing and Data Use

The collected data is used exclusively for the following purposes:

- **Service Provision**: Enable login, identification, and operation of real-time contextual messaging, visual planning, and collaboration features.
- **Maintenance and Security**: Ensure account security, infrastructure stability, and bug fixes.
- **Internal Usage Analysis**: The Author reserves the right to analyze usage metadata in an aggregated and anonymized manner. This analysis is solely intended to improve the ergonomics, performance, and features of the Applications.

**Formal commitment**: The Author undertakes to never sell, rent, transfer, or share Users' personal data with third parties for commercial or other purposes.

## 4. Data Security

The Author implements robust technical and organizational measures to ensure data security:

- **Content encryption**: All communications, messages, files, and audio clips are encrypted in transit and at rest. The algorithm used is AES-256-GCM (Galois/Counter Mode) with Authenticated Associated Data (AAD), ensuring both confidentiality and data integrity.
- **Passwords**: Passwords are hashed using the Argon2id algorithm. The Author never has access to the plaintext password.
- **Two-Factor Authentication (2FA)**: A PIN code is sent by email during registration validation and login. 2FA can be activated by the Author to strengthen account security.
- **Session management**: Each session (desktop or mobile) has a unique token encrypted in the database. The maximum session validity is 24 hours. Beyond this, the User must re-authenticate. The system limits one active session per client type (desktop, mobile): logging in on a new device automatically closes the previous session of the same type.
- **Account recovery**: In case of lost access, the User can recover their account via three secret keys generated at registration, combined with an email address change. This mechanism is protected by a recovery token with a 30-minute validity.
- **Attack protection**: The system implements anti brute-force with rate limiting and automatic banning of malicious IP addresses.
- **Platforms**: Axomind and Axovox are native desktop and mobile applications. The Applications do not exist as web versions, by technical and security choice. Native compiled code reduces the attack surface compared to a web application whose code would be readable in the browser.

## 5. Data Retention

The Applications apply automatic retention rules by content type and subscription plan. Data is automatically deleted at the end of its retention period, without manual intervention.

| Content type | Free / Personal Plan | Pro Plan |
|---|---|---|
| Messages and tombstones | 1 year | 1 year |
| Ephemeral messages | 24 hours | 24 hours |
| Unpinned files (disk) | 1 month | 3 years (GDPR) |
| Pinned media (database) | 3 months | 3 years (GDPR) |
| Audio clips | 1 year | 3 years (GDPR) |
| Error logs and pending IP list | 3 months | 3 months |
| Usage history and statistics | 3 years (GDPR) | 3 years (GDPR) |

Cleaning runs automatically every night. Deletion is permanent and irreversible.

## 6. Illicit Use, Sanctions, and Judicial Cooperation

The User is solely responsible for the content they publish and the communications they establish via the Applications. They undertake to use the Applications only in a legal framework and in compliance with the laws in force in their country of residence and in France.

- **Prohibited Use**: Any use of the Applications for illegal purposes, including, without limitation, harassment, dissemination of hateful content, apology of terrorism, child pornography, intellectual property violation, or organization of criminal activities, is strictly prohibited.
- **Sanctions**: The Author reserves the right to suspend or permanently close, without notice or compensation, the account of any User who violates these rules or uses the Applications for manifestly illicit or malicious purposes.
- **Cooperation with Authorities**: The User is expressly informed that the Author will fully cooperate with competent judicial authorities. In case of verified suspicion or legal requisition concerning illicit use of the Applications, the Author will provide authorized authorities with information in their possession to identify the author of the offenses (account identification data, connection logs, metadata), in strict compliance with the legal framework. The encrypted content of communications is not accessible in plaintext and cannot be provided.

## 7. User Rights

In accordance with applicable regulations (notably GDPR in the European Union), the User has the right to access, rectify, object to, and delete (right to be forgotten) their personal data.

To exercise these rights, the User may send a clear and justified request to the Author at **sebastien.vezzani.dev@gmail.com**.

## 8. Policy Changes

The Author reserves the right to modify this Policy at any time to adapt it to the evolution of the Applications or legal requirements. Users will be informed of any substantial changes.

</details>