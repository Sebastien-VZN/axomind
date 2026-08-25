<div align="center">
  <img src="image/logo_axomind.png" alt="Axomind Logo" width="100"/>
  <h1>Axomind Bot API</h1>
  <p><strong>Integrate automated bots into your Axomind workflows</strong></p>

  <p>
    <img src="https://img.shields.io/badge/API-v2.0-green?style=for-the-badge" alt="API Version"/>
    <img src="https://img.shields.io/badge/Method-POST-blue?style=for-the-badge" alt="HTTP Method"/>
  </p>
</div>

---

## Overview

The Axomind Bot API allows third-party applications to interact with Axomind resources via automated bots. Bots can send and manage messages, read and modify mindmaps, and manage planning assignments.

### What bots can do

| Module | Capabilities |
|---|---|
| **Messenger** | Send, read, update, delete messages in conversations where the bot is assigned |
| **Mindmap** | Read mindmaps, create/update/delete nodes, manage styles, inject directory trees |
| **Planning** | List activities, create/update/delete assignments, read time slots |

### Use cases

- **Monitoring alerts** — Send system notifications to your team
- **CI/CD pipelines** — Notify about build statuses and deployments
- **AI agents** — Connect an AI (via MCP) to read and modify mindmaps, manage planning
- **Scheduled reports** — Automate daily/weekly updates
- **n8n/Zapier workflows** — Connect Axomind to hundreds of services
- **Custom applications** — Build your own integrations

---

## Table of Contents

- [Quick Start](#-quick-start)
- [Authentication](#-authentication)
- [Routes](#-routes)
- [Messenger API (`api_messenger`)](#-messenger-api-api_messenger)
- [Mindmap API (`api_mindmap`)](#-mindmap-api-api_mindmap)
- [Activity / Planning API (`api_activity`)](#-activity--planning-api-api_activity)
- [MCP Server](#-mcp-server)
- [Security](#-security)
- [Rate Limiting](#-rate-limiting)
- [Error Handling](#-error-handling)
- [Real-Time Delivery](#-real-time-delivery)
- [Version Française](#-version-française)

---

## Quick Start

### Prerequisites

1. An Axomind account
2. A bot created in the app (Settings → Bots Management)
3. Your bot's `key_access` token (shown once at creation — store it securely)
4. Your bot's `id_bot` identifier (visible in bot settings)
5. The bot must be assigned to the resources you want it to access (conversations, mindmaps, activities)

### Send Your First Message

```bash
curl -X POST "https://quantive-studio.fr/app/bot_api.php?route=api_messenger" \
  -d "key_access=YOUR_BOT_TOKEN" \
  -d "id_bot=YOUR_BOT_ID" \
  -d "type_action=add_message" \
  -d "content_message=Hello from my bot!"
```

**Success Response (HTTP 200):**
```json
{"return_info": "message added successfully", "message_id": 12345}
```

---

## Authentication

All Bot API requests use the same authentication scheme:

| Parameter | Description |
|---|---|
| `key_access` | Your bot's authentication token (encrypted, shown once at creation) |
| `id_bot` | Your bot's unique identifier |

These two parameters must be included in **every** request body. The server validates them via `BotManager::validateBotToken()` and returns HTTP 404 (empty) if invalid — no information leak.

The bot operates as the **bot owner's user_id**. This means the bot can only access resources where its ID is listed in the `bots` JSONB column — enforced server-side.

---

## Routes

The Bot API has a single endpoint with 3 routes and 13 actions:

```
POST https://quantive-studio.fr/app/bot_api.php?route=<route_name>
```

| Route | Module | Actions |
|---|---|---|
| `api_messenger` | Messenger | `add_message`, `get_messages`, `update_message`, `delete_message` |
| `api_mindmap` | Mindmap | `get_mindmaps`, `get_mindmap`, `sync_nodes` |
| `api_activity` | Activity / Planning | `get_activities`, `get_activity`, `add_assignment`, `update_assignment`, `delete_assignment`, `read_planning` |

### Request Format

- **Method**: POST
- **Route**: Query string (`?route=api_messenger`)
- **Body**: Form-encoded (`application/x-www-form-urlencoded`)
- **Auth params** (in body): `id_bot` + `key_access` + `type_action`

### Response Format

| Scenario | HTTP Code | Response Body |
|---|---|---|
| Success | 200 | JSON with `return_info` (text) + data |
| Bot not assigned to resource | 200 | `{"return_info": "access denied"}` |
| Rate limit exceeded | 200 | `{"return_info": "Please respect the imposed usage limit..."}` |
| Invalid credentials / missing params / unknown route | 404 | *(empty — zero info leak)* |

⚠️ Bot API responses use `return_info` (text), never `return_code` (numeric). The `return_code` field is reserved for the client Flutter API.

---

## Messenger API (`api_messenger`)

### `add_message` — Send a message

| Parameter | Required | Description |
|---|---|---|
| `content_message` | Yes | Message content (text) |
| `id_conversation` | No | Target conversation ID. If omitted, broadcasts to all conversations where the bot is assigned |

**Response:**
```json
{"return_info": "message added successfully", "message_id": 12345}
```

### `get_messages` — Read bot messages

| Parameter | Required | Description |
|---|---|---|
| `id_conversation` | Yes | Conversation ID to read messages from |

**Response:**
```json
{"return_info": "messages retrieved successfully", "messages": [...]}
```

### `update_message` — Update a bot message

| Parameter | Required | Description |
|---|---|---|
| `update_message_id` | Yes | ID of the message to update (must be sent by this bot) |
| `content_message` | Yes | New message content |

**Response:**
```json
{"return_info": "message updated successfully"}
```

### `delete_message` — Delete a bot message

| Parameter | Required | Description |
|---|---|---|
| `delete_message_id` | Yes | ID of the message to delete (must be sent by this bot) |

**Response:**
```json
{"return_info": "message deleted successfully"}
```

---

## Mindmap API (`api_mindmap`)

### `get_mindmaps` — List mindmaps

Returns metadata for all mindmaps where the bot is assigned (no nodes).

**Parameters:** None (only auth)

**Response:**
```json
{"return_info": "mindmaps retrieved successfully", "mindmaps": [...]}
```

### `get_mindmap` — Read a mindmap

Returns full mindmap metadata + all nodes.

| Parameter | Required | Description |
|---|---|---|
| `id_mindmap` | Yes | Mindmap ID |

**Response:**
```json
{"return_info": "mindmap retrieved successfully", "mindmap": {...}, "nodes": [...]}
```

### `sync_nodes` — Replace all nodes

⚠️ **Destructive**: replaces ALL nodes. Example payload: `[{"id": 1, "label": "Node 1"}, ...]`.

| Parameter | Required | Description |
|---|---|---|
| `id_mindmap` | Yes | Mindmap ID |
| `nodes` | Yes | JSON array of full node objects (~25 fields per node) |

**Response:**
```json
{"return_info": "nodes synced successfully"}
```

---

## Activity / Planning API (`api_activity`)

### `get_activities` — List activities

Returns metadata for all activities where the bot is assigned.

**Parameters:** None (only auth)

**Response:**
```json
{"return_info": "activities retrieved successfully", "activities": [...]}
```

### `get_activity` — Read an activity

| Parameter | Required | Description |
|---|---|---|
| `id_activity` | Yes | Activity ID |

**Response:**
```json
{"return_info": "activity retrieved successfully", "activity": {...}}
```

### `add_assignment` — Create time slots

| Parameter | Required | Description |
|---|---|---|
| `id_activity` | Yes | Activity ID |
| `planning_list` | Yes | JSON array of day slots (slot_year, index_position_jour, rel_id_user, taches) |
| `recursive_group` | Yes | JSON object defining the recurrence group (titre, start_date, end_date, active_days, ...) |

**Response:**
```json
{"return_info": "assignment created successfully", "data": [...]}
```

### `update_assignment` — Update an assignment group

| Parameter | Required | Description |
|---|---|---|
| `id_activity` | Yes | Activity ID |
| `update_assignement_id` | Yes | Group ID to update |
| `planning_list` | Yes | JSON array of new day slots |
| `recursive_group` | Yes | JSON object with updated group definition |

**Response:**
```json
{"return_info": "assignment updated successfully", "data": [...]}
```

### `delete_assignment` — Delete an assignment group

| Parameter | Required | Description |
|---|---|---|
| `id_activity` | Yes | Activity ID |
| `delete_recursive_group_slot` | Yes | Group ID to delete |

**Response:**
```json
{"return_info": "assignment deleted successfully"}
```

### `read_planning` — Read all planning slots

Returns all time slot data for a given year (start/end times, day of year, user assignments) and group controls.

| Parameter | Required | Description |
|---|---|---|
| `year` | Yes | Year to read (e.g. 2026) |

**Response:**
```json
{"return_info": "planning retrieved successfully", "slots": [...], "groups": [...]}
```

---

## MCP Server

The [axomind-mcp](https://github.com/Sebastien-VZN/axomind-mcp) repository provides a **MCP server** (Model Context Protocol) that wraps the Bot API into 26 AI-callable tools. This allows any MCP-compatible AI client (Hermes, opencode, Claude Desktop, Cursor, etc.) to interact with Axomind resources naturally.

### What the MCP does

- **Mindmap (10 tools)** — list, read, summary, node description, sync, add, replace, update node, delete node, update style
- **Messenger (4 tools)** — send, get, update, delete messages
- **Planning (9 tools)** — list activities, get activity, add/update/delete assignment, create/modify/verify assignment, read planning
- **Tree (3 tools)** — scan local directories and inject them as mindmap structures

The MCP is a **harmless proxy** — it contains no business logic, makes HTTP POST requests to `bot_api.php`, and returns the JSON. All security stays on the PHP side.

### Installation

```bash
git clone https://github.com/Sebastien-VZN/axomind-mcp.git
cd axomind-mcp
uv pip install -e .
```

### Configuration

Copy `.env.example` to `.env` and fill in your bot credentials:

```bash
cp .env.example .env
```

| Variable | Description |
|---|---|
| `AXOMIND_BASE_URL` | `https://quantive-studio.fr/app/bot_api.php` |
| `AXOMIND_BOT_ID` | Your bot ID (from Axomind UI) |
| `AXOMIND_BOT_KEY` | Your bot access key |

### Hermes integration

In `~/.hermes/config.yaml`:

```yaml
mcp_servers:
  axomind:
    command: "python3"
    args: ["-m", "axomind_mcp.serveur.server"]
    env:
      AXOMIND_BASE_URL: "https://quantive-studio.fr/app/bot_api.php"
      AXOMIND_BOT_ID: "<your_bot_id>"
      AXOMIND_BOT_KEY: "<your_key_access>"
      PYTHONPATH: "/path/to/axomind-mcp/src"
    workdir: "/path/to/axomind-mcp"
```

After editing, restart Hermes or run `/reload-mcp` — the 26 tools are discovered automatically with the `mcp_axomind_` prefix.

### Other MCP clients

Use the same env vars and command. The server uses standard stdio transport — compatible with any MCP client.

---

## Security

### Token Security

- **Encrypted in transit**: Tokens are encrypted with AES-256-GCM when transmitted
- **HTTPS required**: All API calls must use HTTPS (HTTP requests are rejected)
- **One-time visibility**: Tokens are shown only once at creation — store them securely
- **No expiration**: Tokens don't expire, but can be regenerated if compromised

### Regenerating a Compromised Token

1. Open Axomind app
2. Go to **Settings** → **Bots Management**
3. Select your bot
4. Click **"Regenerate Token"**

The old token is immediately invalidated.

### Authorization Model

The bot can only access resources where its ID is listed in the `bots` JSONB column:
- **Conversations** — `coms_conversations.bots @> [id_bot]`
- **Mindmaps** — `mindmap.bots @> [id_bot]`
- **Activities** — `activity.bots @> [id_bot]`

This is enforced server-side. The user manages bot assignments via the Axomind UI — the bot cannot self-assign.

For update/delete messages, an additional check ensures `sender_rel_bot_id == id_bot` — a bot can only modify its own messages.

---

## Rate Limiting

| Limit | Value |
|---|---|
| Requests per token | **20 requests per 60 seconds** (production) |
| Ban threshold | **15 failed auth attempts** |
| Ban duration | **Permanent** (IP added to blocklist, requires manual unban) |

When rate limit is exceeded:
```json
{"return_info": "Please respect the imposed usage limit of 20 requests per minute"}
```

---

## Error Handling

| Scenario | HTTP Code | Response Body | Action |
|---|---|---|---|
| Success | 200 | `{"return_info": "...", ...}` | Operation completed |
| Bot not assigned | 200 | `{"return_info": "access denied"}` | Assign bot to the resource |
| Rate limit | 200 | `{"return_info": "Please respect..."}` | Wait 60 seconds |
| Unknown action | 200 | `{"return_info": "unknown action"}` | Check `type_action` value |
| Invalid token / missing params / unknown route | 404 | *(empty)* | Check credentials and URL |

⚠️ HTTP 404 with empty body = authentication failure or invalid request. The server intentionally returns zero information to prevent reconnaissance.

---

## Real-Time Delivery

Messages and data changes are delivered in real-time to all connected clients via WebSocket:

```
Bot API ──▸ PHP Backend ──▸ Node.js WebSocket Server ──▸ Connected Clients
                                      │
                                      ├──▸ Mobile (Android)
                                      ├──▸ Desktop (Windows/Linux)
                                      └──│ Other sessions
```

### WebSocket Services

| Service | Trigger |
|---|---|
| `new_message` | Bot sends a message |
| `update_message` | Bot updates a message |
| `delete_message` | Bot deletes a message |
| `update_bots` | Bot info updated |
| `sync_mindmap_nodes` | Bot syncs mindmap nodes |
| `add_assignement` | Bot creates an assignement |
| `update_assignement` | Bot updates an assignement |
| `delete_assignement` | Bot deletes an assignement |

---

## Version Française

<details>
<summary>Cliquez pour lire en français</summary>

<div align="center">
  <img src="image/logo_axomind.png" alt="Logo Axomind" width="100"/>
  <h1>API Bot Axomind</h1>
  <p><strong>Intégrez des bots automatisés dans vos workflows Axomind</strong></p>
</div>

---

## Aperçu

L'API Bot Axomind permet aux applications tierces d'interagir avec les ressources Axomind via des bots automatisés. Les bots peuvent envoyer et gérer des messages, lire et modifier des mindmaps, et gérer les affectations de planning.

### Ce que les bots peuvent faire

| Module | Capacités |
|---|---|
| **Messenger** | Envoyer, lire, modifier, supprimer des messages dans les conversations où le bot est affecté |
| **Mindmap** | Lire des mindmaps, créer/modifier/supprimer des nodes, gérer les styles, injecter des arbres de répertoires |
| **Planning** | Lister les activités, créer/modifier/supprimer des affectations, lire les créneaux |

---

## Démarrage Rapide

### Prérequis

1. Un compte Axomind
2. Un bot créé dans l'app (Paramètres → Gestion des Bots)
3. Le token `key_access` de votre bot (affiché une seule fois à la création)
4. L'identifiant `id_bot` de votre bot (visible dans les paramètres du bot)
5. Le bot doit être affecté aux ressources auxquelles on veut qu'il accède

### Envoyer un premier message

```bash
curl -X POST "https://quantive-studio.fr/app/bot_api.php?route=api_messenger" \
  -d "key_access=VOTRE_TOKEN_BOT" \
  -d "id_bot=VOTRE_ID_BOT" \
  -d "type_action=add_message" \
  -d "content_message=Bonjour depuis mon bot !"
```

---

## Authentification

Toutes les requêtes Bot API utilisent le même schéma :

| Paramètre | Description |
|---|---|
| `key_access` | Token d'authentification du bot (chiffré, affiché une seule fois) |
| `id_bot` | Identifiant unique du bot |

Le bot opère avec le **user_id du propriétaire** du bot. Il ne peut accéder qu'aux ressources où son ID est **assigné** dans la colonne `bots` JSONB — vérifié côté serveur.

---

## Routes

L'API Bot a un seul endpoint avec 3 routes et 13 actions :

```
POST https://quantive-studio.fr/app/bot_api.php?route=<nom_route>
```

| Route | Module | Actions |
|---|---|---|
| `api_messenger` | Messenger | `add_message`, `get_messages`, `update_message`, `delete_message` |
| `api_mindmap` | Mindmap | `get_mindmaps`, `get_mindmap`, `sync_nodes` |
| `api_activity` | Activity / Planning | `get_activities`, `get_activity`, `add_assignment`, `update_assignment`, `delete_assignment`, `read_planning` |

### Format de requête

- **Méthode** : POST
- **Route** : Query string (`?route=api_messenger`)
- **Body** : Form-encoded
- **Auth** (dans le body) : `id_bot` + `key_access` + `type_action`

### Format de réponse

| Scénario | Code HTTP | Body |
|---|---|---|
| Succès | 200 | JSON avec `return_info` (texte) + données |
| Bot non affecté | 200 | `{"return_info": "access denied"}` |
| Rate limit dépassé | 200 | `{"return_info": "Please respect..."}` |
| Credentials invalides / params manquants / route inconnue | 404 | *(vide — zéro fuite d'info)* |

⚠️ Les réponses Bot API utilisent `return_info` (texte), jamais `return_code` (numérique).

---

## API Messenger (`api_messenger`)

### `add_message` — Envoyer un message

| Paramètre | Requis | Description |
|---|---|---|
| `content_message` | Oui | Contenu du message |
| `id_conversation` | Non | ID de la conversation ciblée. Si omis, broadcast à toutes les conversations du bot |

### `get_messages` — Lire les messages du bot

| Paramètre | Requis | Description |
|---|---|---|
| `id_conversation` | Oui | ID de la conversation |

### `update_message` — Modifier un message du bot

| Paramètre | Requis | Description |
|---|---|---|
| `update_message_id` | Oui | ID du message à modifier (doit être envoyé par ce bot) |
| `content_message` | Oui | Nouveau contenu |

### `delete_message` — Supprimer un message du bot

| Paramètre | Requis | Description |
|---|---|---|
| `delete_message_id` | Oui | ID du message à supprimer (doit être envoyé par ce bot) |

---

## API Mindmap (`api_mindmap`)

### `get_mindmaps` — Lister les mindmaps

Retourne les métadonnées des mindmaps où le bot est affecté (pas de nodes).

**Paramètres :** Aucun (auth uniquement)

### `get_mindmap` — Lire une mindmap

| Paramètre | Requis | Description |
|---|---|---|
| `id_mindmap` | Oui | ID de la mindmap |

Retourne les métadonnées + tous les nodes.

### `sync_nodes` — Remplacer tous les nodes

⚠️ **Destructif** — remplace TOUS les nodes. Toujours envoyer la liste complète.

| Paramètre | Requis | Description |
|---|---|---|
| `id_mindmap` | Oui | ID de la mindmap |
| `nodes` | Oui | JSON array des nodes complets (~25 champs par node) |

---

## API Activity / Planning (`api_activity`)

### `get_activities` — Lister les activités

**Paramètres :** Aucun (auth uniquement)

### `get_activity` — Lire une activité

| Paramètre | Requis | Description |
|---|---|---|
| `id_activity` | Oui | ID de l'activité |

### `add_assignment` — Créer des créneaux

| Paramètre | Requis | Description |
|---|---|---|
| `id_activity` | Oui | ID de l'activité |
| `planning_list` | Oui | JSON array des jours + plages horaires |
| `recursive_group` | Oui | JSON object du groupe de récurrence |

### `update_assignment` — Modifier un groupe de créneaux

| Paramètre | Requis | Description |
|---|---|---|
| `id_activity` | Oui | ID de l'activité |
| `update_assignement_id` | Oui | ID du groupe à modifier |
| `planning_list` | Oui | JSON array des nouveaux jours + créneaux |
| `recursive_group` | Oui | JSON object du groupe mis à jour |

### `delete_assignment` — Supprimer un groupe de créneaux

| Paramètre | Requis | Description |
|---|---|---|
| `id_activity` | Oui | ID de l'activité |
| `delete_recursive_group_slot` | Oui | ID du groupe à supprimer |

### `read_planning` — Lire tous les créneaux

| Paramètre | Requis | Description |
|---|---|---|
| `year` | Oui | Année à lire (ex: 2026) |

Retourne les données réelles des plages horaires (heures début/fin, jour de l'année, affectations) et les groupes de contrôle.

---

## Serveur MCP

Le repo [axomind-mcp](https://github.com/Sebastien-VZN/axomind-mcp) fournit un **serveur MCP** (Model Context Protocol) qui encapsule l'API Bot en 26 tools utilisables par une IA. Permet à n'importe quel client MCP (Hermes, opencode, Claude Desktop, Cursor, etc.) d'interagir avec Axomind.

### Installation

```bash
git clone https://github.com/Sebastien-VZN/axomind-mcp.git
cd axomind-mcp
uv pip install -e .
```

### Configuration

| Variable | Description |
|---|---|
| `AXOMIND_BASE_URL` | `https://quantive-studio.fr/app/bot_api.php` |
| `AXOMIND_BOT_ID` | ID du bot (depuis l'UI Axomind) |
| `AXOMIND_BOT_KEY` | Clé d'accès du bot |

### Intégration Hermes

Dans `~/.hermes/config.yaml` :

```yaml
mcp_servers:
  axomind:
    command: "python3"
    args: ["-m", "axomind_mcp.serveur.server"]
    env:
      AXOMIND_BASE_URL: "https://quantive-studio.fr/app/bot_api.php"
      AXOMIND_BOT_ID: "<your_bot_id>"
      AXOMIND_BOT_KEY: "<your_key_access>"
      PYTHONPATH: "/path/to/axomind-mcp/src"
    workdir: "/path/to/axomind-mcp"
```

Redémarrer Hermes ou `/reload-mcp` → les 26 tools sont découverts avec le préfixe `mcp_axomind_`.

---

## Sécurité

### Tokens

- **Chiffrés en transit** : AES-256-GCM
- **HTTPS obligatoire**
- **Visibilité unique** : affichés une seule fois à la création
- **Pas d'expiration** : régénérables si compromis

### Modèle d'autorisation

Le bot n'accède qu'aux ressources où son ID est dans la colonne `bots` JSONB :
- **Conversations** — `coms_conversations.bots @> [id_bot]`
- **Mindmaps** — `mindmap.bots @> [id_bot]`
- **Activities** — `activity.bots @> [id_bot]`

L'utilisateur gère les affectations via l'UI Axomind — le bot ne peut pas s'affecter lui-même.

Pour update/delete messages : vérification supplémentaire `sender_rel_bot_id == id_bot`.

---

## Limitation de Débit

| Limite | Valeur |
|---|---|
| Requêtes par token | **20 requêtes par 60 secondes** (production) |
| Seuil de ban | **15 tentatives d'auth échouées** |
| Durée du ban | **Permanent** (IP ajoutée à la blocklist, déblocage manuel requis) |

---

## Gestion des Erreurs

| Scénario | Code HTTP | Body | Action |
|---|---|---|---|
| Succès | 200 | `{"return_info": "...", ...}` | Opération réussie |
| Bot non affecté | 200 | `{"return_info": "access denied"}` | Affecter le bot |
| Limite de débit | 200 | `{"return_info": "Please respect..."}` | Attendre 60 sec |
| Action inconnue | 200 | `{"return_info": "unknown action"}` | Vérifier `type_action` |
| Token invalide / params manquants / route inconnue | 404 | *(vide)* | Vérifier credentials et URL |

---

## Livraison en Temps Réel

Les messages et changements sont livrés en temps réel via WebSocket :

```
API Bot ──▸ Backend PHP ──▸ Serveur WebSocket Node.js ──▸ Clients Connectés
```

| Service WS | Déclencheur |
|---|---|
| `new_message` | Bot envoie un message |
| `update_message` | Bot modifie un message |
| `delete_message` | Bot supprime un message |
| `sync_mindmap_nodes` | Bot sync des nodes |
| `add_assignement` | Bot crée une affectation |
| `update_assignement` | Bot modifie une affectation |
| `delete_assignement` | Bot supprime une affectation |

</details>

---

<div align="center">
  <p><sub><strong>Guide version:</strong> 2026-08-22 | <strong>API version:</strong> 2.0</sub></p>
  <p><a href="README.md">← Back to main documentation</a></p>
</div>