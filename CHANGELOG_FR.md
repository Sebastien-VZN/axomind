# Changelog — Axomind

Toutes les versions notables d'Axomind sont documentées ici.
Notes de version complètes : [GitHub releases](https://github.com/Sebastien-VZN/axomind/releases)

---

## [Unreleased]

### Écosystème et documentation
- Dépôt public de documentation sur GitHub (README, guides d'installation, `API_BOT.md`, `SECURITY.md`, `PERF.md`)
- `API_BOT.md` v2.0 (EN/FR) — référence complète de l'API Bot : 3 routes, 13 actions
- Serveur MCP publié en open source : [axomind-mcp](https://github.com/Sebastien-VZN/axomind-mcp) — 26 tools IA au-dessus de l'API Bot (messagerie, mindmap, planning, injection d'arborescences)

---

## [beta_0.6.5] — 2026-09-02

### Traductions
- Chaînes UI en dur migrées vers le système de traduction : formulaires Kanban (paramètres du tableau, style et couleur des cartes) et repères d'utilisation de la mindmap (sélection, déplacement, zoom) désormais localisés dans les 33 langues supportées
- Mise à jour du package de langage

### Correctifs & interne
- Mindmap : suppression des logs de debug à l'ouverture du formulaire de node
- Dépendance `native_spell_checker` désormais résolue depuis pub.dev au lieu du fork GitHub
- Préparation de la seconde soumission Microsoft Store (MSIX 0.6.5)

---

## [beta_0.6.4] — 2026-08-31

### Traductions
- Nouvelles chaînes Kanban traduites sur les 33 langues supportées (paramètres du tableau, style et couleur des cartes, interactions canvas : sélection, déplacement et zoom des nœuds)

---

## [beta_0.6.3] — 2026-08-28

### MSIX — packaging Microsoft Store (nouveau)
- Axomind est désormais disponible en package MSIX pour le Microsoft Store
- Premier build vérifié : `axomind_0.6.2.msix` (20,5 Mo)

### Traductions
- Mise à jour des données de traduction sur 33 langues supportées

## [beta_0.6.1] — 2026-08-21

- Kanban : série de correctifs UX — glisser-déposer, images de fond des cartes, décorations et règles de visibilité des cartes
- Correctifs UX sur la timeline
- Correctifs UX Android mobile et panneau de première visite
- Mindmap : correctif de null-safety sur la carte active (mixin)
- Correctifs drapeaux de langue et traductions
- Mise à jour du plugin Gradle Android et de la version Flutter

## [beta_0.6.0] — 2026-08-20

Période : août 2026 — 102 commits, 258 fichiers modifiés.

### Tableau Kanban (nouveau module, en développement actif)
- Nouvelle vue Kanban avec boards, colonnes et cartes
- Glisser-déposer pour réordonner cartes et colonnes
- Mise à jour des cartes en temps réel via WebSocket
- Édition inline des cartes (titre, style, pièces jointes)
- Performance : isolation RepaintBoundary sur chaque carte pour fluidifier le tableau
- Barre d'outils dédiée et écran d'accueil

### Forks de l'éditeur de texte riche
- Migration vers nos propres forks de `flutter_quill` et `native_spell_checker`, publiés sur GitHub
- Amélioration de l'éditeur de zone de texte et du composant de visualisation statique
- Intégration de la correction orthographique native OS

### Système de traduction — 31 nouvelles langues
- Migration complète du pipeline de traduction vers un nouveau format `lang_code` (ex. `fr_FR`)
- Ajout de 31 nouvelles langues supportées, avec drapeau dédié pour chacune

### Améliorations de la mindmap
- Reconstruction de la logique du gestionnaire de mindmap pour fiabilité
- Correction de problèmes de corruption des données de nodes (y compris en test)
- Ajout de l'action « réduire tous les nodes » et amélioration du comportement de fermeture/réinitialisation
- Correction du calcul du nombre de nodes enfants
- Amélioration des interactions canvas et du rendu des lignes de connexion

### Système de quotas (reconstruit)
- Reconstruction complète de la gestion des quotas côté client
- Ajout des paramètres de plan d'abonnement
- Correction de l'affichage et de l'application des quotas
- Auto-cleanup côté serveur préparé

### UX et mobile
- Correction de problèmes de mise en page sur mobile et tablette
- Amélioration de l'UX Android (documentation de débogage ajoutée)
- Correction du comportement de fermeture du menu latéral (drawer)
- Correction des interactions de la vue timeline
- Correction du comportement d'ouverture du module messagerie

### Infrastructure et configuration
- Mise à jour du plugin Gradle Android et de la version du SDK Flutter
- Mise à jour des URLs du projet
- Nettoyage de l'initialisation de l'app et de la gestion des instances
- Mise à jour de la dépendance file picker
- Début de restructuration du code pour le fork de messagerie Axovox

### Ce qu'il reste à faire
- **Kanban** : finaliser l'UX défilement/interaction, corriger l'édition de couleur des cartes, raccorder la suppression de carte, stabiliser les overlays de formulaires
- **Quotas** : déployer et vérifier l'auto-cleanup côté serveur
- **Portage Axovox** : continuer la migration des modules partagés vers le fork de messagerie
- **Éditeur** : continuer à fiabiliser le fork Quill (presse-papier, cas limites de la correction orthographique)

## [beta_0.4.1] — 2026-08-07

- Mindmap : panneau d'info de quota et correctifs de quotas
- Correctif de test : persistance de la position en drag & drop
- Correctifs CI (tokens de dépendances, build Windows)

## [beta_0.4.0] — 2026-08-07

### Mindmap — nouveau moteur de rendu
- Refonte complète du système de rendu de la mindmap pour plus de performance et de confort
- Rendu optimisé : grille supprimée, Level of Detail (LOD) adaptatif
- Drag & drop persisté : positions des nodes sauvegardées en base
- Connexions libres : support des cycles et liaisons personnalisées entre nodes
- Performance améliorée : rebuilds réduits, interactions plus rapides

### Éditeur de texte — flutter_quill
- Nouvel éditeur WYSIWYG intégré dans toute l'application
- Formatage riche : listes, citations, liens, images
- Toolbar contextuelle : mode simple ou avancé selon l'usage
- Utilisé dans : édition des nodes, événements timeline, conversations messenger

### Mobile et tablette — UX responsive
- Formulaires dynamiques : panneaux d'édition adaptant leur position selon la taille de l'écran
- Sidebar unifiée : largeur cohérente sur messenger et mindmap
- Thèmes harmonisés : couleurs clair/sombre corrigées sur tous les widgets

## [beta_0.3.1] — 2026-07-17

- Intégration de la sidebar de la messagerie avec adaptation responsive
- Copie de créneaux sur le planificateur
- Performance mindmap : migration de `setState` vers `ValueNotifier` (hover boxes, changement de page)
- Éditeur de texte avancé déplacé dans une modale dédiée ; éditeurs séparés de la sidebar
- Formulaires dynamiques sur les nodes de mindmap
- Styles de bordure de node (`borderDash`) et widget conteneur avancé
- Formulaires d'accès Bot API et tests d'intégration
- Corrections de build Android (version OpenJDK, configuration Java)

## [beta_0.3.0] — 2026-06-03

- Support calendrier : import de fichiers `.ics` et corrections
- Corrections de la timeline et des dossiers temp ; correctifs planificateur
- Documentation de la stratégie Hermes / MCP et futurs points d'entrée Bot API

## [beta_0.2.1] — 2026-05-31

- Ajustements internes de build et CI

## [beta_0.1.6] — 2026-05-26

- Mise à jour des packs de textes de langue
- Correctifs UX mobile (crash messagerie, affichage basse résolution)
- Changements mineurs de dépendances et améliorations UX

## [beta_0.1.5] — 2026-05-12

- Migration majeure du système d'authentification
- Correctifs de sécurité et optimisations
- Suite de tests de charge serveur
- Correctifs sur le contrôle de fermeture de session

## [beta_0.1.4] — 2026-05-11

- 2FA désactivée pendant la refonte du système d'authentification

## [beta_0.1.3] — 2026-05-10

- Fin de la phase 1 du planning — planificateur fonctionnellement complet
- Gestion des droits et accès sur mindmap, activity et messagerie (correctifs d'escalade de privilèges)
- Système tombstone pour la synchronisation du planning et de la mindmap, avec tests cross-utilisateurs
- Élimination des race conditions entre le client HTTP et les appels WebSocket
- Quotas d'usage et statistiques de la timeline
- Optimisation des I/O disque Sembast (upsert différentiel avec dirty tracking)
- Mindmap : refonte du style récursif (avril), clipping viewport et caches indexés pour le rendu
- Travail de performance du rendu timeline (grille mutualisée, couleurs précalculées, lazy layout builders)
- Refacto du gestionnaire de médias ; suivi des mises à jour par utilisateur
- Vague massive de correctifs UX et couverture de tests étendue
