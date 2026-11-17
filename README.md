# Accidents corporels de la route 2024 – Data Storytelling (option C)

**La route et ses conditions : où les accidents sont-ils les plus graves ?**

Projet réalisé en binôme dans le cadre du TP *Data Storytelling* (M1 Cybersécurité et Sciences des Données, Université Paris 8, 2026/2027).

- **Question directrice :** sur quels types de routes, à quelles vitesses autorisées et dans quelles conditions (météo, état de la chaussée, luminosité) les accidents corporels de 2024 sont-ils les plus graves, et où le gestionnaire de voirie doit-il agir en priorité ?
- **Public cible :** gestionnaire de voirie (département, métropole)
- **Outils :** Python (pandas, Jupyter) pour la préparation, Tableau Desktop Public Edition pour la story

## Structure du dépôt

```
road-safety-data-analysis-2024/
├── data/
│   ├── raw/              # 4 CSV BAAC 2024 d'origine (non modifiés)
│   └── processed/        # usagers_enrichis.csv, accidents.csv (générés par le notebook)
├── notebooks/
│   └── preparation_donnees.ipynb
├── reports/
│   ├── controles_qualite.md      # tableau des contrôles qualité
│   ├── valeurs_non_renseignees.csv
│   ├── story_option_C.pdf        # export PDF de la story Tableau
│   └── figures/                  # captures des tableaux de bord
├── docs/                 # description officielle des bases (ONISR)
└── tableau/
    └── accidents_2024_option_C.twbx
```

## Données

Source : Observatoire national interministériel de la sécurité routière (ONISR), *Bases de données annuelles des accidents corporels de la circulation routière*, année 2024, publiées sur [data.gouv.fr](https://www.data.gouv.fr/fr/datasets/bases-de-donnees-annuelles-des-accidents-corporels-de-la-circulation-routiere-annees-de-2005-a-2024/) sous Licence Ouverte (Etalab).

| Table | Grain | Clés |
|---|---|---|
| caractéristiques | 1 accident | `Num_Acc` |
| lieux | lieu principal de l'accident | `Num_Acc` |
| véhicules | 1 véhicule impliqué | `Num_Acc`, `id_vehicule` |
| usagers | 1 usager impliqué | `Num_Acc`, `id_usager`, `id_vehicule` |

Le notebook produit deux tables :

| Fichier | Grain | Usage dans Tableau |
|---|---|---|
| `accidents.csv` | 1 ligne par accident | compter des accidents, part d'accidents graves, carte |
| `usagers_enrichis.csv` | 1 ligne par usager | compter des personnes (tués, hospitalisés…) |

Un accident est **grave** s'il compte au moins un tué (décès dans les 30 jours) ou un blessé hospitalisé (plus de 24 h).

## Reproduire

```bash
pip install -r requirements.txt
# placer les 4 CSV 2024 dans data/raw/
jupyter notebook notebooks/preparation_donnees.ipynb   # Kernel > Restart & Run All
```

Ouvrez ensuite `tableau/accidents_2024_option_C.twbx` avec Tableau Desktop Public Edition.

## Principaux résultats

En 2024, la base compte **54 402 accidents corporels** (France métropolitaine et outre-mer), **3 432 tués** et **19 126 blessés hospitalisés**. **36,1 %** des accidents sont graves, c'est-à-dire qu'ils comptent au moins un tué ou un hospitalisé.

- **Où ont lieu les accidents ≠ où ils sont graves.** 62,5 % des accidents ont lieu en agglomération, mais **67,7 % des tués** sont hors agglomération. Hors agglomération, un accident sur deux est grave (51,5 %), contre un sur quatre en ville (26,9 %).
- **Les routes départementales hors agglomération** concentrent 19,3 % des accidents mais **46,4 % des tués**, et 67 % de leurs accidents sont graves.
- **Vitesse autorisée de 80 km/h** : 13,2 % des accidents, **34,4 % des tués**, 71,8 % d'accidents graves. C'est la part la plus élevée de toutes les classes de vitesse.
- **Conditions associées à une gravité plus forte** :
  - nuit sans éclairage public : 51,6 % d'accidents graves, contre 35,2 % de jour ;
  - virages : 50,6 %, contre 32,6 % en ligne droite ;
  - collisions frontales : 53,2 %.
- La pluie et la chaussée mouillée ne sont pas associées à des accidents plus graves (30,5 % et 32,9 % d'accidents graves).

Ce sont des associations : elles ne prouvent pas une cause, et les données ne permettent pas de les rapporter au trafic.

## Limites

- Les données ne contiennent **ni trafic, ni kilomètres parcourus, ni population**. On compare donc des parts et des niveaux de gravité, pas un « risque » par kilomètre.
- Les associations observées ne sont **pas des causes**.
- La base est brute, avec des valeurs non renseignées (`-1`) et des erreurs de saisie (vitesses hors norme, années de naissance aberrantes, coordonnées nulles). Elles sont documentées dans `reports/controles_qualite.md`.
- 15 645 accidents sont décrits sur plusieurs voies dans la table LIEUX. On garde le lieu principal (la première ligne).
- La définition de « blessé hospitalisé » a changé en 2018 : aucune comparaison avec les années antérieures.

## Auteurs

- _Prénom Nom_
- _Prénom Nom_
