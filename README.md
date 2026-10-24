# 0xGPT0 · IA & marchés

Développeur IA et automatisation. J'ai construit un média sur les marchés financiers qui tourne presque seul : des agents IA programmés lisent les données, écrivent, produisent les visuels, publient et mesurent les résultats. Je montre tout : le système, le code, les coûts et le ROI.

Le terrain de jeu, ce sont les marchés. Chaque jour de semaine, BTC et Hyperliquid sont découpés en trois sessions (Asie, Londres, New York, heure de Paris). Les hauts et les bas de chaque session sont des zones de stops. Je mesure lesquels sont pris, par qui et en combien de temps, sur un an de données, et je publie les résultats avec leurs limites.

**Ce que je sais faire pour un projet :** automatiser un processus de bout en bout (collecte de données, agent IA qui rédige selon des règles, génération d'images et de vidéos, publication multi-plateforme, tableau de mesure), avec des secrets jamais exposés et des coûts suivis.

| Où | |
|---|---|
| X | [@0xGPT0](https://x.com/0xGPT0) |
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

## Création de contenu automatisée

Le média tourne par « rendez-vous » : des tâches programmées réveillent un agent IA (Claude) aux heures des sessions. Il lit les données fraîches, rédige, produit les visuels et prépare la publication. Je valide depuis mon téléphone, ou je laisse partir en automatique.

| Quand (Paris) | Contenu produit |
|---|---|
| Lun à ven ~1h45 | État des lieux avant l'Asie : marchés financiers, niveaux de liquidité, stats du backtest, post X et 5 réponses prêtes à poster sous les gros comptes |
| Lun à ven, ouvertures et clôtures | Asie, Londres, New York : niveaux pris, liquidité restante, graphique de la session |
| Lun à ven ~19h43 | Backtest du soir : idée Éducation TradingView (titre, texte, image), post long sur X et lien de l'idée en réponse |
| Lun à jeu ~23h12 | Review du soir : ce que chaque session a pris, marchés financiers et crypto |
| Vendredi ~23h12 | Rapport complet de la semaine |
| Samedi 11h43 | Post long pédagogique |
| Dimanche 20h47 | Le week-end et la semaine qui arrive |

Ce qui est automatisé à chaque rendez-vous :
- **Données** : récupération des prix, des niveaux de session et des marchés financiers, aucun chiffre écrit sans source ni date.
- **Rédaction** : l'agent écrit selon une charte éditoriale stricte (faits, niveaux et scénarios conditionnels, aucun avis sur la hausse ou la baisse), puis repasse chaque texte dans un filtre anti-style IA avant envoi.
- **Visuels** : trois vues de graphique par actif générées en Python, une par usage, et des images de post aux formats de la charte.
- **Publication** : chaque post arrive avec ✅ Publier, 🕐 Programmer ou ❌ Ignorer, ou part seul en mode automatique. Client de l'API X écrit à la main, séries de posts qui citent le précédent de leur série, file programmée dans Vercel Blob publiée par GitHub Actions.
- **Recyclage** : quand l'idée TradingView est publiée, son lien suffit pour générer et publier le post X qui la présente.
- **Mesure** : chaque post publié est enregistré ; un rapport nocturne classe les séries par heure, longueur, présence d'image et clics sur le profil. Un test par semaine, bilan le vendredi.
- **Sécurité** : les workflows s'authentifient auprès de Vercel avec le jeton OIDC de GitHub, aucun secret partagé.

## Données de marché

- GitHub Actions chaque nuit : Hyperliquid, Fear & Greed, marchés financiers (indices US, Europe, Asie, futures, VIX, taux à 10 ans, dollar, EUR/USD, USD/JPY, pétrole, or).
- Graphiques 15 min générés en Python (matplotlib) dans le style de l'indicateur, avec une croix sur chaque stop pris.
- Service local 24 h/24 (Node.js, WebSocket Binance) : alerte sur mobile quand un haut ou un bas de session BTC est pris, suivi sur 15 minutes, enregistrement des liquidations Binance en CSV, tâche planifiée Windows.

## Shorts « J'ai vérifié »

- Vidéos verticales 1080×1920 écrites en React avec Remotion : idée reçue, backtest, barres animées, chiffre barré, preuve sur les vraies bougies du jour.
- Voix de synthèse française hors ligne (Piper), musique générée en Python (numpy), mixage et normalisation avec ffmpeg.
- Données injectées directement depuis les backtests et les bougies du jour : un nouveau Short se génère à partir d'un fichier de props.
- Validation sur mobile puis envoi sur YouTube (API Data v3) et en brouillon sur TikTok (Content Posting API).
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
| Automatisation | Agents IA programmés (Claude), GitHub Actions, OIDC, API X, YouTube Data, TikTok, Binance, Hyperliquid |
| Média | ffmpeg, Piper TTS, matplotlib, Playwright, MoviePy, Tesseract |
| Desktop et navigateur | Electron, Manifest V3, Service Workers |

## Contact

Un message sur [X](https://x.com/0xGPT0).

Pas un conseil financier. Les résultats passés d'un backtest ne disent rien de la suite.
