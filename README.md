# Tableau de Bord des Performances Commerciales — TelecomPlus

Projet réalisé dans le cadre d'un examen de Visualisation de Données (L3 SEA).
Conception d'un tableau de bord analytique complet sous Power BI pour un opérateur de télécommunications fictif.

---

## Contexte

TelecomPlus est un opérateur de télécommunications souhaitant disposer d'une vision claire de ses performances commerciales et opérationnelles. L'objectif est de concevoir un tableau de bord dynamique permettant à la direction de piloter les activités en temps réel à partir de données mensuelles multi-dimensionnelles.

---

## Structure des données

Le fichier `ventes_telecom.xlsx` contient 4 tables :

| Table | Lignes | Description |
|---|---|---|
| Ventes | 180 | CA, coûts, marges par produit et région |
| Clients | 120 | Acquisition, résiliations, satisfaction |
| Support_Technique | 240 | Tickets, temps de résolution, taux |
| KPIs_Mensuels | 12 | ARPU, Churn, CAC, LTV, satisfaction |

---

## Modèle de données

Schéma en étoile avec une table Calendrier centrale reliée aux 4 tables de faits via la colonne Date (relation 1 vers plusieurs, filtre à sens unique).

```
Calendrier (1)
    ├── Ventes (*)
    ├── Clients (*)
    ├── KPIs_Mensuels (*)
    └── Support_Technique (*)
```

---

## Indicateurs calculés (DAX)

```dax
ARPU Moyen         = AVERAGE(KPIs_Mensuels[ARPU])
Churn Rate         = AVERAGE(KPIs_Mensuels[Taux_Churn])
Taux Satisfaction  = AVERAGE(KPIs_Mensuels[Taux_Satisfaction])
CA Total           = SUM(Ventes[Chiffre_Affaires])
Marge Totale       = SUM(Ventes[Marge])
Taux Marge         = DIVIDE([Marge Totale], [CA Total])
Taux Acquisition   = DIVIDE(SUM(Clients[Nouveaux_Clients]),
                       SUM(Clients[Nouveaux_Clients]) + SUM(Clients[Resiliations]))
MTTR               = AVERAGE(Support_Technique[Temps_Resolution])
Taux Resolution    = AVERAGE(Support_Technique[Taux_Resolution])
```

---

## Structure du tableau de bord

### Page 1 — Ventes et Finance
- Courbe d'évolution du CA mensuel par produit (Mobile, Internet, TV)
- Graphique à barres du CA par région
- Anneau de répartition du CA par produit
- 5 cartes KPI : CA Total, Marge Totale, Taux de Marge, Nombre de Ventes, Coût moyen
- Filtres dynamiques : Produit, Région

### Page 2 — Clients et Churn
- Courbe du Churn Rate mensuel avec ligne de tendance
- Histogramme Nouveaux clients vs Résiliations
- Anneau des motifs de résiliation
- 5 cartes KPI : ARPU, Taux Satisfaction, Churn Rate, Taux Acquisition, Nouveaux Clients
- Filtres dynamiques : Segment, Région

### Page 3 — Support Technique
- Courbe du MTTR mensuel avec ligne d'objectif
- Jauge du taux de résolution global
- Barres horizontales par type d'incident
- 5 cartes KPI : MTTR, Taux Résolution, Tickets Total, Non résolus, Score SAV
- Filtres dynamiques : Type d'incident, Région

### Page 4 — Analyses Avancées
- Nuage de points : corrélation Satisfaction / Churn (r = -0,72)
- Prévisions CA sur 6 mois avec intervalle de confiance à 95%
- Tableau de synthèse mensuel de tous les KPIs

---

## Résultats obtenus

Le tableau de bord développé sous Power BI regroupe 9 indicateurs clés calculés en DAX (ARPU, Churn Rate, MTTR, etc.) répartis sur 3 pages interactives avec filtres dynamiques par produit, région et période. L'analyse avancée a permis de mettre en évidence une corrélation négative significative (r = -0,72) entre la satisfaction client et le taux de désabonnement, ainsi qu'une tendance de croissance du CA confirmée par des prévisions sur 6 mois avec un intervalle de confiance à 95%.

---

## Compétences démontrées

- Power BI Desktop
- Power Query (nettoyage et transformation de données)
- DAX (mesures et colonnes calculées)
- Modélisation de données (schéma en étoile)
- Visualisation analytique
- Storytelling par les données

---

## Fichiers du projet

```
├── ventes_telecom.xlsx       # Données source
├── TelecomPlus.pbix          # Fichier Power BI
├── telecomplus_dashboard.html # Aperçu visuel du tableau de bord
└── README.md
```

---

## Aperçu

> Un aperçu HTML interactif du tableau de bord est disponible dans le fichier `telecomplus_dashboard.html`.
> Il peut être ouvert directement dans un navigateur sans Power BI.

---

## Auteur

Projet réalisé dans le cadre du cours de Visualisation de Données — L3 SEA.

