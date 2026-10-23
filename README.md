# 0xGPT0

Je construis un média crypto qui lit le marché par les sessions et les liquidations, et tous les outils qui vont avec : un indicateur TradingView, des backtests, un site d'analyse, un bot Telegram, une chaîne de publication automatique et des Shorts générés en code.

Chaque jour de semaine, BTC et Hyperliquid sont découpés en trois sessions (Asie, Londres, New York, heure de Paris). Les hauts et les bas de chaque session sont des zones de stops. Je mesure lesquels sont pris, par qui et en combien de temps, sur un an de données, et je publie les résultats avec leurs limites.

| Où | |
|---|---|
| X | [@0xGPT0](https://x.com/0xGPT0) |
| Telegram | [Crypto Review Daily](https://t.me/CryptoReviewDaily) |
| Outils en ligne | [brief-crypto.vercel.app](https://brief-crypto.vercel.app) |
| TradingView | OxGPT0 : indicateur public et un « Backtest du soir » chaque soir de semaine |
| YouTube, TikTok | Shorts de la série « J'ai vérifié » |

---

## Indicateur TradingView : Sessions, Liquidity & Backtest Day Mask

Script public en Pine v5, conçu pour lire la liquidité de session sans rien redessiner à la main.

- **Sessions** Asie, Londres, chevauchement Londres et New York, New York, en fond léger ou en cadres, du lundi au vendredi.
- **Liquidité de session** : à la fin de chaque session, son haut et son bas deviennent des niveaux. Trait plein tant que les stops sont là, pointillés qui s'arrêtent à la bougie exacte où ils sont pris.
- **Liquidité daily et hebdo** : haut et bas de chaque jour reconstruits sous le daily (15 min, 1 h, 4 h), cadre haut et bas de chaque semaine.
- **Zones communes** : plusieurs niveaux non pris au même prix sont fusionnés, avec un compteur ×2, ×3 et une épaisseur qui augmente.
- **Événements passés** : stats macro US de 8h30, ouverture et clôture de Wall Street, ouverture de Londres, clôture daily, gap CME. Un triangle marque chaque événement et la réaction mesurée sur N minutes s'affiche dans la légende.
- **Masque de backtest** : on décoche des jours de la semaine et leurs bougies deviennent invisibles (ou en noir et blanc), pour s'entraîner sans voir la suite. Fuseau UTC ou Paris.
- **Tableau de statistiques** en option.

## Backtests

Node.js sur les bougies Binance, lancés par GitHub Actions, résultats publiés sur une branche dédiée puis recopiés sur le site.

**Sessions** (bougies 15 min, BTCUSDT, 21/10/2025 au 19/10/2026, 260 jours de semaine)
- Pour chaque jour : quelle session prend en premier le haut et le bas de la précédente, et combien de ces prises se referment de l'autre côté.
- Rangé par jour de la semaine et par contexte de marché.
- Qui fait le haut ou le bas du jour : Asie 77 %, Londres 59 %, New York 52 %. L'idée reçue des 70 % pour Londres ne tient pas.
- Le haut de l'Asie est repris le jour même 56 % du temps, son bas 63 %.

**Timing** (bougies 1 min, un an)
- 876 hauts et bas de session pris, 813 repris de l'autre côté en moins de 4 h. Délai médian : 1 minute ; 90 % en moins de 32 minutes.
- Trade simulé à chaque reprise (entrée à la clôture revenue, stop à l'extrême de la mèche) : environ 0 R avant frais, -0,6 R après. Les stops médians font 0,125 % du prix.
- Analyse des gros RR (3R, 5R, 10R), de la règle « couper si rien après N minutes » et des impulsions de volume qui suivent un balayage.
- Limites écrites partout : un an, un actif, pas de glissement, pas de financement.

## 0xbaw Lab : le site

HTML, CSS et JavaScript sans framework, fonctions serverless sur Vercel. Identité « terminal » : JetBrains Mono, Instrument Serif, teal sur fond nuit.

- **/graphique** : bougies BTC et HYPE sans limite d'historique via Binance, sessions, niveaux, flèches sur chaque haut ou bas pris (couleur de la session qui le prend), trades du backtest avec entrée, stop, objectif et sortie, masque des jours.
- **/backtest** : qui prend quoi, par jour et par contexte, haut et bas du jour, cartes sur téléphone, export d'une image pour X.
- **/timing** : simulateur complet (objectif, coupure, session, stop, sens, frais, impulsion), courbe du compte, liste des trades avec lien vers le graphique.
- Filtres synchronisés avec l'adresse : chaque résultat se partage par lien. Bandeau BTC en direct avec la session en cours.

## Bot Telegram et publication automatique

- Bot privé, point central du média : positions (Backpack, adresses EVM suivies), marché, watchlist, funding, niveaux, réglages stockés dans le message épinglé.
- Chaque post proposé arrive avec ✅ Publier, 🕐 Programmer ou ❌ Ignorer. Publication par l'API officielle de X, client écrit à la main. Mode automatique activable depuis Telegram.
- Les posts sont rangés en séries (Asie, New York, soir, éducation) et chaque nouveau post cite le précédent de sa série.
- File de posts programmés dans Vercel Blob, publiée par GitHub Actions aux heures prévues.
- Les workflows s'authentifient auprès de Vercel avec le jeton OIDC de GitHub : aucun secret partagé.
- Rapport X chaque soir : posts classés par série, heure, longueur et clics sur le profil.

## Données de marché

- GitHub Actions chaque nuit : Hyperliquid, Fear & Greed, marchés financiers (indices US, Europe, Asie, futures, VIX, taux à 10 ans, dollar, EUR/USD, USD/JPY, pétrole, or).
- Graphiques 15 min générés en Python (matplotlib) dans le style de l'indicateur, avec une croix sur chaque stop pris.
- Service local 24 h/24 (Node.js, WebSocket Binance) : alerte Telegram quand un haut ou un bas de session BTC est pris, suivi sur 15 minutes, enregistrement des liquidations Binance en CSV, tâche planifiée Windows.

## Shorts « J'ai vérifié »

- Vidéos verticales 1080×1920 écrites en React avec Remotion : idée reçue, backtest, barres animées, chiffre barré, preuve sur les vraies bougies du jour.
- Voix de synthèse française hors ligne (Piper), musique générée en Python (numpy), mixage et normalisation avec ffmpeg.
- Validation dans Telegram puis envoi sur YouTube (API Data v3) et en brouillon sur TikTok (Content Posting API).
- Charte graphique et modèles HTML (Short, miniature YouTube, image de post) rendus en PNG avec Playwright.

---

## Autres projets

- **[ConFluent](https://github.com/GPT012/Engineering-Portfolio/tree/main/ConFluent_Mac)** : application macOS (Electron) avec écoute globale du clavier, dictée vocale vers le presse-papiers et traduction en temps réel. Extensions [Chrome](https://github.com/GPT012/Engineering-Portfolio/tree/main/ConFluent_Chrome) et [Safari](https://github.com/GPT012/Engineering-Portfolio/tree/main/ConFluent_Safari) en Manifest V3 pour traduire les champs de saisie sur n'importe quel site.
- **[CoefRessources](https://github.com/GPT012/Engineering-Portfolio/tree/main/coef-modern)** : CMS et CRM en Next.js 15 et Supabase, tableau de bord des contacts en temps réel, politiques Row Level Security sur PostgreSQL. [Démo](https://coef-modern.vercel.app).
- **[Screenshot to Video](https://github.com/GPT012/Engineering-Portfolio/tree/main/screenshot-to-video)** : génération automatique de vidéos courtes à partir de captures (Python, MoviePy, OCR Tesseract).
- **[Chess Analysis](https://github.com/GPT012/Engineering-Portfolio/tree/main/Chess_Animations_Chrome)** : extension d'analyse de coups avec Stockfish NNUE en local et API cloud.
- **[aisec-dataset](https://github.com/GPT012/aisec-dataset)** : jeu de données pour entraîner des modèles à la sécurité des smart contracts. 20 projets Solidity audités entre juin et novembre 2024 (DeFi, stablecoins, staking, cross-chain), 8 104 fichiers de code et 214 constats classés par sévérité.

## Stack

| | |
|---|---|
| Langages | TypeScript, JavaScript, Python, Pine Script, Solidity, SQL |
| Front | React, Next.js, Remotion, HTML et CSS sans framework, Tailwind, Framer Motion |
| Back et données | Node.js, fonctions Vercel, Vercel Blob, Supabase, PostgreSQL, WebSocket |
| Automatisation | GitHub Actions, OIDC, API X, Telegram, YouTube Data, TikTok, Binance, Hyperliquid |
| Média | ffmpeg, Piper TTS, matplotlib, Playwright, MoviePy, Tesseract |
| Desktop et navigateur | Electron, Manifest V3, Service Workers |

## Contact

Un message sur [X](https://x.com/0xGPT0) ou sur le [canal Telegram](https://t.me/CryptoReviewDaily).

Pas un conseil financier. Les résultats passés d'un backtest ne disent rien de la suite.
