# GOLD SMC Tool – XAUUSD M15

Indicateur **TradingView (Pine Script v5)** pour le trading de l’or en M15 avec les concepts SMC (Smart Money Concepts), accompagné d’un **journal de trading** web.

> ⚠️ **Avertissement** : cet outil est une aide visuelle et un outil de calcul. Il ne constitue pas un conseil financier. Le trading comporte un risque de perte en capital. Teste toujours en démo avant d’utiliser de l’argent réel.

---

## Contenu du dépôt

| Fichier | Description |
|---|---|
| `GOLD_SMC_Tool.pine` | Indicateur TradingView (liquidités, IDM, OB, S&D, annonces, plan de trade) |
| `journal_trading.html` | Journal de trading (page web autonome) |
| `README.md` | Ce document |

---

## 1. Indicateur `GOLD_SMC_Tool.pine`

### Installation

1. Ouvre TradingView puis le **Pine Editor** (en bas de l’écran).
2. Colle le contenu de `GOLD_SMC_Tool.pine`.
3. Clique sur **Ajouter au graphique**.
4. Utilise-le sur **XAUUSD en M15**.

### Ce que l’indicateur affiche

| Élément | Nom affiché | Couleur par défaut |
|---|---|---|
| Plus haut / plus bas de la session asiatique (00h–06h UTC) | `Asian High`, `Asian Low` | orange, cyan |
| Plus haut / plus bas de la veille | `PDH`, `PDL` | jaune, magenta |
| Plus haut / plus bas de la semaine précédente | `PWH`, `PWL` | saumon, bleu clair |
| Liquidité externe (swings majeurs) | `Liq EXT High / Low` | bleu |
| Equal highs / equal lows | `EQH`, `EQL` | rouge |
| Liquidité interne (petits swings dans le range) | `Liq INT High / Low` | gris *(masquée par défaut)* |
| Inducement protégé par le 0.5 du mouvement | `IDM High / Low · protégé 0.5` | vert lime |
| Order Blocks raffinés | `OB Bull`, `OB Bear` | vert, rouge |
| Supply / Demand | `SUPPLY`, `DEMAND` | violet, teal *(masquées par défaut)* |
| Annonces économiques | `🔴 NOM · date heure` | orange |

Toutes les couleurs sont modifiables dans les paramètres. Un niveau de liquidité pris est retiré du graphique (ou conservé en pointillés avec un ✔ si l’option est activée).

### Définitions utilisées

- **Liquidité externe** : swings majeurs (10 bougies de chaque côté), EQH/EQL, PWH/PWL.
- **Liquidité interne** : petits swings M15 situés à l’intérieur du range majeur.
- **IDM (inducement)** : après un mouvement fort (≥ 4 ATR), petit swing de la correction qui reste du côté protégé du 0.5 du mouvement. Le 0.5 est tracé.
- **Order Block** : dernière bougie baissière avant l’impulsion haussière qui casse la structure (BOS), et inversement. L’impulsion doit démarrer en 6 bougies maximum et faire au moins 2 ATR. L’OB est invalidé si le prix a déjà clôturé au travers.
- **Raffinage de l’OB** : si la bougie est trop grande, on ne garde que le corps ; si le corps est encore trop grand, la zone est plafonnée à 1 ATR ou 6 $ (le plus petit des deux).
- **Supply / Demand** : une base de 1 à 2 petites bougies suivie d’une grosse bougie d’impulsion qui s’en échappe.
- **Zone mitigée** : une zone est supprimée quand le prix clôture au travers.

### Annonces économiques

Pine Script n’a pas accès à un calendrier économique en direct. L’indicateur fonctionne donc ainsi :

- **NFP** : calculé automatiquement (1er vendredi du mois, 08h30 heure de New York, de 2025 à 2030). Les mois où ce vendredi est férié (par exemple janvier 2027) peuvent être décalés par le BLS : vérifie la date officielle.
- **CPI, PPI, FOMC** : liste modifiable dans les paramètres, section « Annonces économiques », une annonce par ligne au format :

```
AAAA-MM-JJ,HH:MM,NOM
```

L’heure est celle de **New York**. Exemple : `2027-01-13,08:30,CPI`. Ne rajoute pas le NFP à la main.

Dates préremplies (jusqu’à fin 2026) : FOMC 28/10 et 09/12, CPI 10/11 et 10/12, PPI 13/11 et 15/12. Le PPI du 13/11 est à vérifier sur le calendrier officiel du BLS.

Affichage : ligne verticale et étiquette sur chaque annonce, fenêtre colorée de 15 min avant à 30 min après, et tableau des prochaines annonces avec compte à rebours (fuseau d’affichage réglable, `Europe/Brussels` par défaut).

Sources officielles : [BLS](https://www.bls.gov/schedule/) (CPI, PPI, NFP) et [Federal Reserve](https://www.federalreserve.gov/monetarypolicy/fomccalendars.htm) (FOMC).

### Plan de trade (biais, entrée, SL, TP, lots)

- **Biais** : suit le dernier BOS. BOS haussier → achats, BOS baissier → ventes.
- **Zone d’entrée** : la zone OB, Supply ou Demand la plus proche du prix, dans le sens du biais. Seule la zone choisie est surlignée en bleu (les Supply/Demand restent masquées par défaut mais sont utilisées par le plan).
- **Entrée** : ordre limite à 50 % de la zone (ou sur le bord, au choix).
- **SL** : placé derrière la liquidité clé la plus proche, au-delà de la zone. Si cette liquidité est trop loin pour respecter le risque maximum au lot minimum, le trade est déclaré **non valide** et la raison est affichée.
- **TP1** : sur la liquidité clé la plus proche entre 1R et 1,5R (pour passer à break even), sinon à 1R.
- **TP2** : sur la liquidité clé la plus proche à 2R minimum (6R maximum), sinon à 2R.
- **Lot** : `risque ÷ (distance du SL × 100)`, arrondi vers le bas au pas de 0,01 et plafonné.
- **Liquidités clés** : PDH, PDL, PWH, PWL, Asian High/Low, liquidités externes et EQH/EQL non encore prises.

Paramètres par défaut : capital 300 $, risque 10 $ par trade, RR minimum 2, contrat 100 oz par lot, lot min 0,01, lot max 0,10. Tout est modifiable dans la section « 8. Plan de trade ».

> 10 $ sur 300 $ représentent 3,3 % du capital par trade, ce qui est élevé. Adapte le risque à ta tolérance.

### Réglages par défaut pour un graphique lisible

3 jours d’historique, 5 liquidités externes et 3 OB actifs au maximum, liquidités internes et Supply/Demand masquées, niveaux pris supprimés.

---

## 2. Journal `journal_trading.html`

Page web autonome, sans dépendance.

**Fonctionnalités**

- Actif (XAUUSD par défaut), unité de temps (M1 à D1), sens, lot, date.
- Bilan **Positif / Négatif / Break even** avec le montant du résultat.
- Points positifs et points négatifs.
- Capture d’écran du trade (fichier ou collage Ctrl+V), compressée automatiquement, agrandissable au clic.
- Historique avec la case **Résultat verte (positif), rouge (négatif) ou grise (break even)**.
- Statistiques : capital actuel, capital de départ (300 $ par défaut), résultat total, nombre de trades, taux de réussite, profit factor.
- Filtre par actif.

**Utilisation** : ouvre `journal_trading.html` dans ton navigateur. Hors de claude.ai, les données sont enregistrées dans le `localStorage` du navigateur : elles restent sur cet appareil et sont perdues si tu vides les données du site. Pense à garder tes captures importantes ailleurs.

---

## Limites connues

- L’indicateur n’a pas de calendrier économique automatique (voir plus haut).
- Les définitions SMC (OB, IDM, liquidités) sont une interprétation : adapte les paramètres à ta façon de trader.
- L’outil ne vérifie pas qu’une liquidité a été prise avant l’entrée.
- Le code n’a pas été testé sur toutes les conditions de marché : vérifie toujours les niveaux visuellement avant de passer un ordre.

## Licence

À définir par l’auteur (par exemple MIT).
