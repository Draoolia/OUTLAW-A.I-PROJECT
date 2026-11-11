# 🎮 Gaming Outlaws Bot - Guide complet

Bot Discord multifonction pour serveurs gaming. Panneau de configuration graphique via `/settings`.

---

## 📋 Fonctionnalités

### 🔧 Panneau de configuration (`/settings`)
Accessible uniquement aux administrateurs. Contient :

- **Admin** : langues, rôles équipe, protection anti-raid, couleur des fenêtres,...
- **Welcome** : messages d'arrivée/départ, carte, mention, image de fond perso
- **Rules** : salon du règlement, bouton d'acceptation, traduction
- **FreeGames** : annonces de jeux gratuits (Steam, Epic, GOG, etc.)
- **Voice Channels** : salons vocaux temporaires illimité
- **Levels** : XP, rôles de niveau, counting
- **Tickets** : catégorie et salon d'archives
- **Autres** :
  - **Birthday** : salon des annonces, activation
  - **YouTube** : ajout/suppression de chaînes, dans des salons défini et le rôle annoncé
  - **Giveaway** : création, participants, terminer, reroll

### 🔒 Commandes Admin / Modérateur
| Commande | Description |
|----------|-------------|
| `/settings` | Ouvre le panneau de configuration |
| `/reactionrole` | Rôles par réactions (emoji) |
| `/reactionrolebutton` | Rôles par boutons |
| `/youtube` | Gestion des chaînes YouTube |
| `/giveaway` | Création et gestion des giveaways |
| `/mod` | Modération (mute, warn, ban, etc.) |
| `/clearmessages` | Suppression de messages en masse |
| `/annonce` | Annonces via IA |
| `/reglement` | Gestion du règlement |
| `/rules` | Affichage un rappel des règles |
| `/welcome` | Configuration avancée du welcome |
| `/ticket` | Gestion des tickets |
| `/freegames` | Jeux gratuits |
| `/voice-setup` | Salons vocaux temporaires illimité et customisable |
| `/redprotection` | Protection anti-raid |
| `/xp` | Rôles de niveau |
| `/setup-levels` | Configuration XP |
| `/setup-counting` | Jeu de comptage |

### 👥 Commandes Membres
| Commande | Description |
|----------|-------------|
| `/embed create` | Créer un embed (fenêtre de composition) |
| `/embed edit` | Modifier un embed existant par son identifiant |
| `/embed delete` | Supprimer son dernier embed |
| `/level` | Voir sa carte de niveau |
| `/rank` | Classement du serveur |
| `/birthday` | Gérer son anniversaire |
| `/meme` | Mèmes |
| `/joke` | Blagues |
| `/translate` | Traduire un message |
| `/outlawchat` | Discuter avec l'IA en DM |
| `/youtube watch` | Regarder des vidéos YouTube à plusieurs dans un salon vocal |
| `/help` | Aide (adaptée au rôle et à la langue) |

### 🎮 Jeux
Solo
- `/snake` `/slots` `/findemoji` `/akinator`

Multi joueurs
- `/rps` Pierre papier ciseaux, `/connect4` puissance 4, `/tictactoe` morpion

### 🤖 Intelligence artificielle
- Mentionne le bot `@Outlaw Bot` pour poser une question dans le salon où tu te trouves.
- Répond dans la langue du membre (selon son rôle)
- Utilise Groq par défaut (`openai/gpt-oss-20b`), fournisseur et modèle configurables
- Gemini reste disponible en second fournisseur via `GEMINI_API_KEY`

### 🌍 Multilingue (8 langues)
FR, EN, PT (BR), ES, RU, EL (grec), IT, DE

Le bot s'adapte à la langue du membre selon son rôle. Les embeds ont un bouton **Translate** ou pas selon le vouloir de l'admin

---

## 📜 Logs du serveur et logs de protection

Moteur de logs calqué sur RaidProtect, avec **39 types répartis en 5 familles**. Chaque type
peut avoir son propre salon, ou suivre celui de sa famille, ou être coupé (il ne part nulle part).

**Familles et types :**

| Famille | Types journalisés |
|---------|-------------------|
| 📁 **Salons, rôles et invitations** | salon créé / modifié / supprimé, fil créé / supprimé, rôle créé / modifié / supprimé, invitation créée / supprimée, bot ajouté / retiré |
| 👥 **Membres** | arrivée (avec l'âge du compte), départ (date d'arrivée et rôles), rôles ajoutés ou retirés, pseudo modifié, avatar de serveur modifié, boost ajouté ou retiré, exclusion temporaire, membre inactif retiré |
| 💬 **Messages** | message supprimé (avec son contenu), message modifié (avant / après), suppression en masse, message épinglé / désépinglé |
| 🔊 **Vocal** | arrivée, départ avec la durée de la session, changement de salon, micro coupé ou rétabli, son coupé ou rétabli, caméra activée ou coupée, action du staff : rendre muet, action du staff : déconnecter ou déplacer |
| 😀 **Emojis** | emoji créé / modifié / supprimé, sticker créé / modifié / supprimé |

**Détails utiles :**

- **Qui a fait quoi** : un événement Discord dit ce qui a changé, jamais qui l'a fait. L'auteur
  est lu dans le journal d'audit du serveur (suppression de salon, de rôle, kick, action vocale).
- **Trois formats de rendu** : Défaut, Encadré, Compact.
- **Trois salons de protection** : Général, Modération, Captcha. Ils retombent sur le salon
  `raid_logs` déjà configuré si rien n'est défini.
- **Fiches de sanction** dans le style RaidProtect : en-tête avec pseudo et `ID`, section avec
  l'icône, le type et le motif, la ligne « Sanction silencieuse, le membre n'a pas été prévenu. »,
  puis `ID s_XXXXX · Mod :`. 13 types couverts : warn, mute, unmute, kick, ban, softban, unban,
  anti-raid, anti-arnaque, HoneyPot, AutoMod, captcha, réinitialisation.
- Chaque sanction reçoit un **numéro de dossier** court (`s_XXXXX`) conservé en base : il permet
  de retrouver une sanction et de compter ce qu'un membre a pris.

---

## 🛡️ Protections

### 🚪 Captcha d'arrivée (`/settings` → Captcha)
Modèle RaidProtect `/captcha`, en mode **Challenge** : à l'arrivée, le membre reçoit un rôle
*Non vérifié* et un code affiché dans une image. Tant qu'il ne l'a pas recopié dans le salon de
vérification, il ne peut rien écrire ailleurs. Au bout de N essais ratés ou du délai dépassé,
la sanction configurée s'applique.

Deux réglages impossibles depuis un simple bot, et pourquoi :
- le captcha « Panneau » (bouton qui ouvre une Activity Discord) demande une application
  enregistrée chez Discord et un hébergement HTTPS public ;
- la « Vérification Discord » officielle exige que le serveur coche lui-même chaque membre : le
  bot peut seulement élever le niveau de vérification du serveur, ce que fait le module DISCON.

### 🚨 DISCON (`/settings` → Admin → DISCON)
Le niveau de danger du serveur, sur le modèle du DEFCON : cinq niveaux, de 5 (calme) à 1
(catastrophe). Un clic applique d'un coup toutes les protections du niveau choisi, un second
clic défait ce que le DISCON avait lui-même activé. **Règle de sécurité** : on ne défait que ce
qu'on a soi-même posé. Un mode anti-raid, une fermeture des MP ou un verrouillage que l'équipe
avait mis à la main restent en place, parce que l'état d'origine est capturé une fois, à la
première montée, et remis tel quel. À partir du niveau 2, le DISCON impose le captcha aux
arrivants en plus du mode anti-raid.

### 🍯 HoneyPot
Un salon piège : quiconque y écrit est sanctionné automatiquement. Le propriétaire et l'équipe
en sont exemptés (ils voient le salon, donc y écrire n'est pas forcément un signe de bot, et un
bannissement automatique d'un admin est irréversible).

### 🛡️ Anti-raid (`/redprotection`)
Détection des vagues d'arrivées, avec action configurable : expulser les nouveaux membres ou
faire fermer les invitations par Discord.

---

## 🎫 Autres modules

- **Tickets** : système complet avec catégorie résolue dynamiquement (une catégorie est créée si
  besoin), transcript HTML à l'archive.
- **Salons vocaux temporaires** : salons illimités et personnalisables.
- **Rôles-réaction et rôles-boutons** : les boutons sont enregistrés en base, donc modifiables et
  supprimables après un redémarrage (25 boutons max, 5 lignes de 5).
- **Giveaways** : tirage automatique des gagnants à la fin du timer, avec reroll.
- **Niveaux et XP** : XP par message, embed, mème, blague et counting, cooldown configurable,
  rôles de niveau, carte de niveau avec image de fond personnalisable en DM.
- **Anniversaires** : salon d'annonce et activation.
- **Jeux gratuits** : annonces Steam, Epic, GOG et autres via GamerPower.
- **YouTube** : notifications de nouvelles vidéos et lecture à plusieurs dans un salon vocal.
- **Règlement** : salon dédié, bouton d'acceptation, seule exception aux règles de langue.
- **Traduction** : `/translate`, plus un bouton *Translate* ajoutable à un message via
  `/addtranslate`.

---

## 🚀 Installation

### 1. Créer le bot sur Discord
1. Va sur [Discord Developer Portal](https://discord.com/developers/applications)
2. **New Application** → nomme-la
3. Onglet **Bot** → **Reset Token** → copie le token
4. Active les intents : Presence, Server Members, **Message Content**
   (sans Message Content, les logs de messages ne peuvent donner ni l'avant ni l'après)
5. Onglet **OAuth2 → URL Generator** :
   - Coche `bot` + `applications.commands`
   - Permissions : Manage Roles, Moderate Members, Ban Members, Manage Messages, Send Messages,
     Embed Links, Attach Files, Read Message History, Add Reactions, **View Audit Log**
     (nécessaire pour attribuer une action à son auteur dans les logs), Manage Channels,
     Manage Guild, Move Members
6. Invite le bot sur ton serveur avec le lien généré

### 2. Configuration
1. Crée un fichier `.env` à la racine :
   ```
   DISCORD_TOKEN=ton_token
   GROQ_API_KEY=ta_cle_groq
   GEMINI_API_KEY=ta_cle_gemini
   CLIENT_ID=id_de_ton_application
   GUILD_ID=id_de_ton_serveur
   ```
2. Installe les dépendances :
   ```bash
   npm install
   ```
3. Lance le bot :
   ```bash
   npm start
   ```
4. Enregistre les commandes :
   ```bash
   npm run deploy
   ```

### 3. Première configuration
1. Sur Discord, utilise `/settings`
2. Configure la langue, les rôles équipe, le welcome, etc.
3. Active les modules dont tu as besoin, puis configure les salons de logs

---

## 📁 Structure
```
├── commands/
│   ├── admin/          # Commandes réservées au staff
│   └── public/         # Commandes pour tous
├── events/             # Écouteurs Discord
│   ├── logsSalons.js   #   logs salons, rôles, invitations, bots
│   ├── logsMembres.js  #   logs arrivées, départs, rôles, pseudos
│   ├── logsMessages.js #   logs messages, purges, épingles
│   ├── logsVocal.js    #   logs vocal, avec durée des sessions
│   ├── logsEmojis.js   #   logs emojis et stickers
│   └── ...
├── utils/
│   ├── database.js     # Base SQLite et requêtes
│   ├── setup.js        # Panneau /settings
│   ├── logs.js         # Moteur de logs et fiches de sanction
│   ├── logEvenements.js # Lecture du journal d'audit
│   ├── captcha.js      # Captcha d'arrivée
│   ├── discon.js       # Niveaux de danger
│   ├── i18n.js         # Traductions
│   ├── gemini.js       # IA (Groq et Gemini)
│   └── ...
├── locales/            # Fichiers de langue
├── tests/              # Suites de tests
├── data/               # Base SQLite et médias (bot.db, cards, welcome, embed-media)
├── index.js            # Point d'entrée
└── .env                # Configuration (à créer)
```

---

## 🔧 Variables d'environnement
| Variable | Description |
|----------|-------------|
| `DISCORD_TOKEN` | Token du bot (obligatoire) |
| `GROQ_API_KEY` | Clé API Groq (IA, fournisseur par défaut) |
| `GEMINI_API_KEY` | Clé API Gemini (second fournisseur, optionnel) |
| `CLIENT_ID` | Identifiant de l'application, pour déployer les commandes |
| `GUILD_ID` | Serveur de test, pour déployer les commandes localement |

---

## 📝 Notes
- Le panneau `/settings` est réservé aux administrateurs
- Les commandes sont enregistrées en global (pas de doublons)
- La base de données est SQLite, dans `data/bot.db`
- Les giveaways tirent automatiquement les gagnants à la fin du timer
- Les messages transférés peuvent recevoir un bouton de traduction via `/addtranslate`
- Les logs de messages supprimés ne peuvent afficher le contenu que si le message est encore en
  cache ou si l'intent Message Content est activé : sinon le log le dit explicitement

---

## 🆘 Support
En cas de problème, vérifie :
1. Les intents sont activés (dont Message Content et Server Members)
2. Le bot a les permissions nécessaires, dont **View Audit Log** pour les logs
3. Les commandes sont déployées (`npm run deploy`)
4. Les logs de la console pour les erreurs
