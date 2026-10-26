# Trading Calendrier

Calendrier des **annonces économiques à forte volatilité** pour le trading de l’or (XAUUSD), affiché directement sur le graphique TradingView.

Il fait partie de l’indicateur `GOLD_SMC_Tool.pine` (section **« 7. Annonces économiques »** des paramètres).

> ⚠️ Cet outil est une aide visuelle, pas un conseil financier. Les dates et heures doivent toujours être vérifiées sur les calendriers officiels avant de trader.

---

## Ce que fait le calendrier

- Trace une **ligne verticale** sur chaque annonce, avec une étiquette `🔴 NOM · jj/mm hh:mm`.
- Colore en orange une **fenêtre de volatilité** autour de l’annonce : 15 min avant et 30 min après (réglable).
- Affiche en haut à droite un **tableau des 3 prochaines annonces** avec l’heure et un **compte à rebours**.
- Convertit automatiquement les heures de New York vers ton fuseau (Europe/Brussels par défaut), heure d’été et d’hiver comprises.

## Installation

1. Ouvre TradingView puis le **Pine Editor**.
2. Colle le contenu de `GOLD_SMC_Tool.pine`.
3. Clique sur **Ajouter au graphique** (XAUUSD, M15).
4. Ouvre les paramètres de l’indicateur, section **« 7. Annonces économiques »**.

---

## Annonces incluses

### NFP – automatique

Le NFP est calculé automatiquement : **1er vendredi de chaque mois à 08:30 (heure de New York)**, de 2025 à 2030.

Exception : quand ce vendredi tombe un jour férié (par exemple janvier 2027), la publication peut être décalée. Vérifie la date officielle.

> N’ajoute pas le NFP à la main dans la liste, il serait affiché deux fois.

### CPI, PPI, FOMC – liste préremplie

| Date | Heure (New York) | Annonce |
|---|---|---|
| 28/10/2026 | 14:00 | FOMC |
| 10/11/2026 | 08:30 | CPI |
| 13/11/2026 | 08:30 | PPI |
| 09/12/2026 | 14:00 | FOMC |
| 10/12/2026 | 08:30 | CPI |
| 15/12/2026 | 08:30 | PPI |

Dates vérifiées le 26/10/2026. Le **PPI du 13/11** vient d’un site secondaire : confirme-le sur le calendrier du BLS.

---

## Ajouter ou modifier une annonce

Dans les paramètres, la case de texte « Annonces » contient **une annonce par ligne** :

```
AAAA-MM-JJ,HH:MM,NOM
```

- La date et l’heure sont en **heure de New York** (pas la tienne).
- Pas de virgule dans le nom.

Exemples :

```
2027-01-13,08:30,CPI
2027-01-27,14:00,FOMC
2027-02-12,08:30,PPI
```

### Heures habituelles (heure de New York, à vérifier)

| Annonce | Heure |
|---|---|
| NFP, CPI, PPI | 08:30 |
| Décision FOMC | 14:00 |

### Décalage avec la Belgique

Le décalage entre New York et Bruxelles est de 6 h la plupart de l’année, et de 5 h quelques semaines au printemps et à l’automne (les changements d’heure n’ont pas lieu aux mêmes dates). Le script le gère tout seul : par exemple, le FOMC du 28/10/2026 à 14:00 à New York s’affiche à 19:00 à Bruxelles.

---

## Paramètres

| Paramètre | Défaut | Rôle |
|---|---|---|
| Afficher les annonces | activé | Active ou coupe tout le calendrier |
| NFP automatique | activé | Calcule le NFP (1er vendredi du mois) |
| Fuseau d’affichage | Europe/Brussels | Fuseau des heures affichées |
| News passées | 24 h | Combien d’heures en arrière afficher |
| News à venir | 72 h | Combien d’heures en avant afficher |
| Fenêtre autour de l’annonce | 15 min avant, 30 min après | Zone colorée de volatilité |
| Tableau des prochaines annonces | activé | Tableau avec compte à rebours |
| Couleur des annonces | orange | Couleur des lignes et de la fenêtre |

---

## Mettre à jour le calendrier

Les dates changent chaque année et peuvent être décalées (jour férié, retard de publication). Sources officielles :

- **CPI, PPI, NFP** : [bls.gov/schedule](https://www.bls.gov/schedule/)
- **FOMC** : [federalreserve.gov – calendrier FOMC](https://www.federalreserve.gov/monetarypolicy/fomccalendars.htm)

## Limites

- Pas de calendrier en direct : Pine Script ne peut pas se connecter à un site externe. Les nouvelles dates sont à ajouter à la main.
- Pas de valeurs prévues ni publiées, pas de niveau d’impact : seules les heures des annonces sont affichées.
- Les lignes verticales pour des annonces très éloignées dans le futur peuvent ne pas s’afficher (limite de TradingView) : c’est pour cela que seules les 72 prochaines heures sont dessinées par défaut.

## Licence

À définir par l’auteur (par exemple MIT).
