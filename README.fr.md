[English](README.md) · **Français**

# Agent Viewer

Un tableau kanban pour gérer plusieurs agents Claude Code tournant dans des sessions tmux. Lancez, surveillez et interagissez avec des agents depuis une seule interface web.

<img src="monde.jpg" alt="agent-viewer" width="100%">

<img width="1466" height="725" alt="Screenshot 2026-02-09 at 14 54 21" src="https://github.com/user-attachments/assets/cd31b988-f649-4e92-9844-7a1ece9aa634" />

Gérez vos agents depuis votre téléphone avec Tailscale

![IMG_7782](https://github.com/user-attachments/assets/c7298d61-dd37-4d0f-8b0a-d9d1f0231782)


## Prérequis

- [Node.js](https://nodejs.org/) (v18+)
- [tmux](https://github.com/tmux/tmux)
- [Claude CLI](https://docs.anthropic.com/en/docs/claude-code) (commande `claude` disponible dans le PATH)

### Installer les prérequis (macOS)

```bash
brew install node tmux
npm install -g @anthropic-ai/claude-code
```

## Installation

```bash
git clone <repo-url> && cd agent-viewer
npm install
```

## Utilisation

```bash
npm start
```

Ouvrez http://localhost:4200 dans votre navigateur.

### Configuration

| Variable | Défaut      | Description              |
|----------|-------------|--------------------------|
| `PORT`   | `4200`      | Port du serveur          |
| `HOST`   | `localhost`  | Adresse d'écoute (`0.0.0.0` pour l'accès réseau) |

Exemple :

```bash
HOST=0.0.0.0 PORT=3000 npm start
```

## Accès distant via Tailscale

Vous pouvez accéder à Agent Viewer depuis votre téléphone (ou tout autre appareil) en utilisant [Tailscale](https://tailscale.com/).

### 1. Installer Tailscale sur votre Mac

```bash
brew install tailscale
```

Ou téléchargez depuis [tailscale.com/download](https://tailscale.com/download).

### 2. Installer Tailscale sur votre téléphone

Téléchargez l'application Tailscale depuis l'[App Store](https://apps.apple.com/app/tailscale/id1470499037) ou [Google Play](https://play.google.com/store/apps/details?id=com.tailscale.ipn). Connectez-vous avec le même compte.

### 3. Démarrer le serveur

```bash
npm start
```

Le serveur écoute sur `0.0.0.0` par défaut, il est donc déjà accessible sur toutes les interfaces réseau, y compris Tailscale.

### 4. Ouvrir depuis votre téléphone

Trouvez l'IP Tailscale de votre Mac (affichée dans l'application Tailscale ou via `tailscale ip`), puis visitez :

```
http://<tailscale-ip>:4200
```

Si vous avez activé [MagicDNS](https://tailscale.com/kb/1081/magicdns), vous pouvez utiliser le nom de votre machine à la place :

```
http://<machine-name>:4200
```

## Fonctionnalités

- **Lancer des agents** — Cliquez sur `[+ SPAWN]` ou appuyez sur `N`, saisissez un chemin de projet et un prompt. Chaque agent est lancé dans sa propre session tmux exécutant `claude`.
- **Colonnes kanban** — Les agents sont triés dans les colonnes Running, Idle et Completed selon leur état.
- **Auto-découverte** — Les sessions tmux existantes exécutant Claude sont automatiquement détectées et ajoutées au tableau.
- **Sortie en direct** — Cliquez sur `VIEW OUTPUT` pour voir la sortie terminal complète avec rendu des couleurs ANSI.
- **Envoi de messages** — Tapez dans le champ prompt de n'importe quelle carte et appuyez sur `Ctrl+Enter` pour envoyer des messages de suivi à un agent.
- **Envoi de fichiers** — Glissez-déposez des fichiers sur une carte ou cliquez sur `FILE` pour envoyer des fichiers à un agent.
- **Relance** — Les agents terminés (Completed) peuvent être relancés avec un nouveau prompt depuis le même répertoire de projet.
- **Attach** — Cliquez sur `ATTACH` pour copier la commande `tmux attach` et accéder directement au terminal.
- **Détection de branche git** — Chaque carte affiche automatiquement la branche git courante du répertoire de travail de l'agent.
- **Notification à la fin** — Cochez « NOTIFY ON COMPLETE » lors du lancement pour déclencher une notification système quand l'agent termine (nécessite `~/.claude/scripts/notify.sh`).
- **Invites interactives** — Quand un agent s'arrête sur une approbation de plan ou un menu numéroté, la carte le détecte, affiche un badge `PLAN APPROVAL` ou `SELECT` et fait apparaître les contrôles `↑ ↓ ENTER ESC` pour naviguer et répondre sans avoir à s'attacher au terminal.
- **Retour sur le plan** — Sur une invite d'approbation de plan, saisissez directement dans le champ de message pour router votre texte vers l'option « dire à Claude quoi changer » au lieu d'approuver tel quel.
- **Stop** — Cliquez sur `STOP` pour interrompre un agent en cours d'exécution.

### Invites interactives

Quand un agent s'arrête pour poser une question, la carte reproduit l'invite et vous donne les touches pour y répondre.

Une invite d'approbation de plan sur une carte, avec les contrôles de navigation `PLAN` :

![Approbation de plan sur une carte](docs/screenshots/plan-card.png)

Un menu numéroté affiché sur une carte, avec les contrôles `SELECT` :

![Invite de sélection sur une carte](docs/screenshots/select-card.png)

Les mêmes invites, vues en entier dans la visionneuse de sortie :

![Approbation de plan dans la visionneuse de sortie](docs/screenshots/plan-output-viewer.png)

![Invite de sélection dans la visionneuse de sortie](docs/screenshots/select-output-viewer.png)

## Licence

Sous licence Creative Commons Attribution-NonCommercial-NoDerivatives 4.0 International (CC BY-NC-ND 4.0). Voir [LICENSE.md](LICENSE.md). Pour citer ce travail, voir [CITATION.cff](CITATION.cff) ou utiliser le bouton « Cite this repository » de GitHub.

## Développement

L'architecture, les systèmes internes et le fonctionnement sont documentés dans [CLAUDE.md](CLAUDE.md), qui guide aussi Claude Code lorsqu'il travaille dans ce dépôt.

Par [Ismaël Joffroy Chandoutis](https://ismaeljoffroychandoutis.com).
