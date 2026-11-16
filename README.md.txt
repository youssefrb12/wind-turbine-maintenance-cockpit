# Cockpit de maintenance éolien : analyse SCADA, disponibilité et causes d'arrêt

Analyse d'une année (2023) de données SCADA et du journal d'états de l'éolienne **Kelmarsh 1**
(Senvion MM92, 2 050 kW), avec un tableau de bord Power BI de 5 pages.

> **Limites** : turbine **terrestre, à multiplicateur** (pas offshore à entraînement direct).
> Une turbine, une année. Projet personnel de formation ; la méthode est transposable.

## Données

Kelmarsh wind farm data, Cubico Sustainable Investments (Plumley & Takeuchi), Zenodo,
licence CC-BY-4.0.
- SCADA 10 min : 312 colonnes, 2,7 Go (année 2023), fuseau UTC.
- Journal d'états Greenbyte (Status) : 7 997 lignes.

Les fichiers bruts ne sont **pas inclus** : à télécharger sur Zenodo
(« Kelmarsh wind farm data ») et à placer dans `data/raw/`.

## Structure

| Dossier | Contenu |
|---|---|
| `notebooks/` | 00 archive du nettoyage, 02 courbe de puissance, 03 états et disponibilité |
| `data/` | `kelmarsh1_2023_propre.csv` (SCADA nettoyé, 16 colonnes utiles) |
| `results/` | Tableaux de résultats (CSV) |
| `figures/` | Graphiques (PNG) |
| `powerbi/` | `kelmarsh1_dashboard.pbix` |

## Méthode et résultats

**1. Nettoyage**
- 2 174 760 lignes pour 52 560 dates : le surplus était constitué de lignes entièrement vides,
  supprimées après vérification (52 560 lignes distinctes).
- 83 valeurs manquantes (0,16 %), en 9 blocs. 8 blocs sur 9 correspondent à des coupures de
  communication confirmées par le journal d'états ; le 9e (15/08, 10 min) reste inexpliqué.

**2. Performance**
- Courbe de puissance conforme à la courbe constructeur, plateau à 2 050 kW.
- Pitch proche de 0° jusqu'à environ 10 m/s, puis croissant pour plafonner la puissance.
- Cp médian estimé à 0,46. Environ 4 % des valeurs dépassent la limite de Betz (0,593) :
  cause probable, mesure du vent sur la nacelle (non confirmé).
- Épisode du 10/08 à 06:00 (rotor 3,6 RPM, pitch 53°) : test de batterie (code 710) suivi d'un
  redémarrage automatique, confirmé par le journal d'états.

**3. États, disponibilité, causes d'arrêt**
- États reconstitués sur 8 760 h (total 99,99 %). Début d'année (74 h avant le premier événement)
  supposé en Full Performance : hypothèse. 18 lignes sans catégorie classées « Non classé ».
- Disponibilité temporelle recalculée : **99,61 %** (hors givre), **99,02 %** (givre inclus),
  contre 98,93 % publié par Greenbyte. Compter le givre est un choix de règle, pas un fait.
- Pareto : givre ≈ 52 h (≈ 60 % des heures d'indisponibilité), arrêts manuels ≈ 26 h,
  défauts techniques < 1 h.
- MTTR 3,6 min, MTBF 876 h, sur 10 arrêts forcés seulement (échantillon réduit, indicatif).

**4. Tableau de bord Power BI** : états, Pareto, indicateurs clés, courbe de puissance, pitch.

## Reproduire

1. Télécharger les fichiers Zenodo (SCADA 2023 et Status de la turbine 1) dans `data/raw/`.
2. `pip install pandas matplotlib`
3. Lancer les notebooks dans l'ordre (le notebook 00 relit 2,7 Go : plusieurs minutes).

## Source et licence

Données : Cubico Sustainable Investments, CC-BY-4.0.

## Auteur

Youssef Rebah, élève ingénieur en génie électrique, ENSIT.
[LinkedIn](https://www.linkedin.com/in/youssef-rebah-895b47223/)