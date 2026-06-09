# esg-performance-analysis
ESG Power Bi project

Présentation du projet

Dans un monde où la transition énergétique est devenue une priorité stratégique, les entreprises des secteurs industriels et énergétiques sont sous pression croissante pour améliorer leurs performances environnementales, sociales et de gouvernance. Le suivi sur le long terme des scores ESG et de l'intensité carbone est essentiel pour évaluer si les engagements de durabilité des entreprises se traduisent par des résultats mesurables.

Ce projet présente un **dashboard Power BI interactif** construit sur des données ESG de 30 entreprises mondiales réparties sur 3 secteurs, couvrant 14 années de données (2010–2024) pour analyser :

* L'évolution des scores ESG dans le temps
* Les tendances de l'intensité carbone par secteur et par entreprise
* La distribution de la maturité ESG entre les entreprises
* Les meilleurs et moins bons performeurs ESG

Le dashboard permet une prise de décision basée sur les données pour soutenir l'investissement durable, la conformité réglementaire et la stratégie ESG des entreprises.

---

Objectifs

* Suivre l'évolution du score ESG et de l'intensité carbone de 2010 à 2024
* Comparer les performances ESG entre les secteurs (Services aux collectivités, Industrie, Énergie)
* Identifier les leaders et retardataires ESG parmi 30 entreprises mondiales
* Analyser la relation entre intensité carbone et score ESG
* Classer les entreprises par niveau de maturité ESG

---

Nettoyage & Transformation des données 

Les étapes suivantes ont été réalisées pour préparer le jeu de données :

1. Transformation des données (Power Query)

* Ouverture de l'éditeur Power Query pour nettoyer et structurer les données
* Vérification des valeurs manquantes et correction des types de données
* Standardisation des noms de colonnes et des formats
* Corriger les erreurs liées aux nombres 

2. Import des données dans Power BI

Chargement du jeu de données ESG dans Power BI Desktop

3. Création de colonnes calculées

Création d'une colonne de maturité ESG basée sur le score :

```DAX
ESG_Maturite = 
SWITCH(
    TRUE(),
    Fact_ESG[esg_score_0_100] < 40, "Faible",
    Fact_ESG[esg_score_0_100] < 60, "Moyen",
    Fact_ESG[esg_score_0_100] < 80, "Élevé",
    "Leader"
)
```

* Permet de classer chaque entreprise par niveau de maturité ESG
* Utilisée pour le graphique de distribution par maturité

4. Création des mesures DAX**

```DAX
Evolution Intensité Carbone 2010-2024 = 
VAR Score2010 =
    CALCULATE(
        AVERAGE(Fact_ESG[carbon_intensity_tco2e_per_musd]),
        Fact_ESG[year] = 2010
    )
VAR Score2024 =
    CALCULATE(
        AVERAGE(Fact_ESG[carbon_intensity_tco2e_per_musd]),
        Fact_ESG[year] = 2024
    )
RETURN
    DIVIDE(Score2024 - Score2010, Score2010) * 100
Evolution ESG 2010-2024 = 
VAR Score2010 =
    CALCULATE(
        AVERAGE(Fact_ESG[esg_score_0_100]),
        Fact_ESG[year] = 2010
    )
VAR Score2024 =
    CALCULATE(
        AVERAGE(Fact_ESG[esg_score_0_100]),
        Fact_ESG[year] = 2024
    )
RETURN
    DIVIDE(Score2024 - Score2010, Score2010) * 100
Emissions totals = SUM(Fact_ESG[scope1_emissions_mt_co2e])+SUM(Fact_ESG[scope2_emissions_mt_co2e])+SUM(Fact_ESG[scope3_emissions_mt_co2e])
```

---

Fonctionnalités du Dashboard

Cartes KPI**

* Score ESG Moyen %
* Intensité carbone (tCO₂/M$)
* Émissions totales (tCO₂)
* Revenu total (M$)
* Évolution de l'intensité carbone 2010–2024
* Évolution du score ESG 2010–2024


---

### **🎛️ Slicers (Filtres)**

* Filtre par pays
* Filtre par année
* Filtre par secteur
* Filtre par entreprise

Ces filtres permettent une exploration dynamique et interactive des données.

---

## 📈 Visualisations

1. Graphique en barres groupées, Score ESG par secteur

* Affiche le score ESG moyen par secteur
* Permet de comparer les performances entre Utilities, Industriels et Énergie

2. Graphique en courbes, Intensité carbone VS Score ESG

* Montre l'évolution croisée de l'intensité carbone et du score ESG de 2010 à 2024
* Met en évidence la corrélation négative entre les deux indicateurs

3. Nuage de points, Score ESG et Intensité carbone

* Visualise la relation entre score ESG et intensité carbone entreprise par entreprise
* Identifie les profils atypiques et les clusters de performance

4. Graphique en secteurs, Maturité ESG

* Affiche la répartition des entreprises par niveau de maturité (Leader, Élevé, Moyen, Faible)
* Évalue la distribution globale de la performance ESG

5. Graphique en barres horizontales, Top 10 ESG

* Classement des 10 meilleures entreprises par score ESG moyen
* Identifie les leaders et les benchmarks sectoriels

6. Graphique en barres horizontales, Bottom 10 ESG

* Classement des 10 entreprises les moins performantes par score ESG
* Met en lumière les retardataires et les axes d'amélioration prioritaires

---

Principaux enseignements

* La transition ESG est réelle mais incomplète : score moyen de 56,82/100 sur 14 ans
* Décarbonation et performance ESG progressent ensemble (corrélation négative confirmée)
* Le score ESG ne reflète pas uniquement l'empreinte carbone, c'est une mesure multidimensionnelle (E + S + G)
* En 2024, sur les 30 entreprises analysées, aucune catégorie ne domine clairement. 11 entreprises affichent un niveau "Élevé", 10 un niveau "Moyen", 7 ont atteint le statut "Leader" et 2 restent au niveau "Faible".
Autrement dit, 7 entreprises sur 30 seulement sont véritables leaders ESG. Les 21 entreprises restantes ont encore une marge de progression significative. Le secteur avance, mais l'excellence ESG reste l'exception, pas la norme.
* Le clivage ESG est géographique : les leaders sont européens, les retardataires sont majoritairement américains
* Le secteur Énergie est le chantier prioritaire avec un score moyen de 46/100

---

Conclusion

Ce dashboard fournit des insights actionnables pour :

* Accélérer la transition ESG dans le secteur Énergie
* Faire progresser les entreprises du niveau "Moyen" vers "Élevé"
* S'inspirer des modèles européens pour les retardataires
* Réduire l'intensité carbone tout en améliorant la gouvernance et le social

---

Outils & Technologies

* Power BI Desktop
* Power Query (ETL)
* DAX 

---

Comment utiliser le dashboard

1. Télécharger le fichier `.pbix`
2. Ouvrir dans Power BI Desktop
3. Utiliser les slicers pour explorer les données par pays, secteur, année ou entreprise
4. Interagir avec les visuels pour approfondir l'analyse

---

Contact

Pour tout retour ou suggestion, n'hésitez pas à me contacter ! (mel.bourrai2019@gmail.com)
