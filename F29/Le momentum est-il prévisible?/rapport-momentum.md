<table width="100%">
<tr>
<td align="left"><img src="images/logo-encg.png" alt="ENCG Settat" width="130"></td>
<td align="right"><img src="images/logo-uh1.png" alt="Université Hassan 1er" width="130"></td>
</tr>
</table>

<br><br>

<p align="center"><b>RAPPORT : BASE DE DONNÉES ET DATA SCIENCE</b></p>

<h1 align="center"><u>LE MOMENTUM EST-IL PRÉVISIBLE ?</u></h1>

<hr>

<br><br>

<table width="100%">
<tr>
<td><b><u>Réalisé par :</u></b><br><br><b>KANINE YASMINE</b><br><br><b>EL-BOUKILI MARIA</b></td>
<td><b><u>Encadré par :</u></b><br><br><b>Mr. ABDERRAHIM LARHLIMI</b></td>
</tr>
</table>

<div style="page-break-after: always;"></div>

---

# RÉSUMÉ :

Ce rapport étudie la capacité des rendements passés du facteur Momentum à prévoir le signe de son rendement mensuel suivant. À partir des données de Kenneth R. French, quatre variables historiques sont construites, puis trois approches sont comparées sur un découpage chronologique : le signe du dernier mois, la régression logistique et la Random Forest. Sur l'échantillon de test couvrant janvier 2007 à septembre 2026, la régression logistique obtient la meilleure accuracy (57,8 %) et le meilleur F1-score (66,7 %). Elle est également la seule stratégie simulée à présenter un rendement cumulé net positif (+9,78 %). Toutefois, son drawdown maximal atteint −56,52 %. Les résultats suggèrent donc un signal prédictif limité, dont l'exploitation reste exposée à des pertes importantes.

---

# <u>Plan du rapport :</u>

**Introduction générale**

**Chapitre 1 :** Cadre théorique et conceptuel

**Chapitre 2 :** Problématique et démarche méthodologique

**Chapitre 3 :** Mise en œuvre empirique et traitement des données

**Conclusion générale**

---

# INTRODUCTION

> Les marchés financiers évoluent sous l'influence de facteurs économiques, financiers et comportementaux. Dans ce contexte, l'anticipation des rendements constitue un enjeu important pour les investisseurs. Parmi les phénomènes étudiés en finance figure le momentum, qui repose sur l'idée que les actifs ayant récemment enregistré de bonnes performances peuvent continuer à bien performer à court terme.
>
> Cette observation conduit à s'interroger sur la persistance des rendements : les performances passées contiennent-elles une information utile pour anticiper les performances futures ? Le présent projet examine cette question à partir du facteur Momentum de la base de données Kenneth R. French.
>
> L'étude transforme cette question en un problème de classification binaire. Il ne s'agit pas de prévoir précisément le montant du rendement futur, mais de déterminer si celui-ci sera positif ou négatif au mois suivant. Trois approches sont ensuite comparées et une simulation financière permet d'évaluer l'intérêt économique des signaux produits.
>
> Problématique : dans quelle mesure les performances passées du facteur Momentum permettent-elles de prévoir le signe de son rendement au cours du mois suivant, et cette information peut-elle être exploitée dans une stratégie après coûts ?

---

## <u>Chapitre 1 : Cadre théorique et conceptuel</u>

**1.1. Le concept de momentum en finance**

Le momentum désigne une régularité observée sur les marchés financiers : les actifs qui ont relativement bien performé sur une période passée tendent parfois à poursuivre leur dynamique à court terme. À l'inverse, les actifs ayant enregistré de faibles performances peuvent continuer à sous-performer. Cette logique est à la base de stratégies dites de suivi de tendance.

Dans le cadre de ce projet, l'analyse porte sur le facteur Momentum agrégé et sur ses rendements mensuels. L'objectif est de vérifier si la dynamique passée de ce facteur contient une information permettant d'anticiper le signe de son rendement futur.

**1.2. Momentum et prévisibilité des marchés**

La prévisibilité des rendements pose la question de la rapidité avec laquelle les marchés intègrent l'information. Si les prix réagissent progressivement à certaines informations, une tendance peut se prolonger pendant plusieurs périodes. Les comportements des investisseurs et les ajustements graduels des anticipations peuvent également contribuer à cette persistance.

Ces mécanismes constituent des explications possibles du momentum, mais ils ne sont pas directement testés dans le notebook. L'étude se concentre sur la relation statistique entre les rendements historiques et le signe du rendement du mois suivant.

**1.3. Les limites du momentum et le risque de crash**

Le momentum n'est pas une régularité permanente. Lorsque les tendances de marché s'inversent brutalement, les stratégies qui reposent sur leur prolongement peuvent subir des pertes importantes. Ces épisodes sont souvent désignés sous le terme de momentum crashes.

Une évaluation pertinente doit donc aller au-delà de la performance moyenne ou de l'exactitude prédictive. Elle doit également considérer les pertes extrêmes, les baisses cumulées du capital et les coûts liés aux changements de position.

---

## <u>Chapitre 2 : Problématique et démarche méthodologique</u>

**2.1. Objectifs de l'étude**

Étudier l'évolution historique du facteur Momentum.<br>
Construire des variables explicatives à partir de plusieurs horizons passés.<br>
Prédire le signe du rendement du mois suivant.<br>
Comparer une règle de référence, une régression logistique et une Random Forest.<br>
Évaluer les résultats statistiques et la performance d'une stratégie après coûts.<br>
Examiner les risques de pertes extrêmes et les limites de l'approche.

**2.2. Hypothèses de recherche**

H1 : les rendements historiques du facteur Momentum contiennent une information utile pour prévoir le signe de son rendement au mois suivant.<br>
H2 : l'utilisation de plusieurs horizons historiques peut améliorer la prévision par rapport à une règle fondée uniquement sur le dernier mois.<br>
H3 : les modèles de classification peuvent apporter une amélioration par rapport à la méthode de référence.

Ces hypothèses sont examinées empiriquement à partir des résultats obtenus sur l'échantillon de test. Elles ne sont pas considérées comme validées avant l'analyse des résultats.

**2.3. Présentation des données**

Les données proviennent de la Kenneth R. French Data Library, dans le fichier consacré au facteur Momentum. Le notebook télécharge l'archive ZIP, lit la série mensuelle et conserve deux colonnes : la date et le rendement du facteur Mom. Les rendements exprimés en pourcentage dans le fichier source sont convertis en valeurs décimales pour les calculs.

**2.4. Variables explicatives et variable cible**

Les variables explicatives sont décalées d'un mois : elles utilisent uniquement les rendements connus avant le mois cible. La cible vaut 1 lorsque le rendement du mois courant est strictement positif et 0 lorsqu'il est négatif ou nul. Cette construction évite d'introduire directement le rendement cible dans les variables d'entrée.

**2.5. Découpage chronologique**

Les 80 % premières observations sont utilisées pour l'entraînement et les 20 % les plus récentes pour le test. Le découpage respecte l'ordre temporel : l'échantillon d'entraînement couvre janvier 1928 à décembre 2006 (948 observations), et l'échantillon de test couvre janvier 2007 à septembre 2026 (237 observations). Cette démarche évite le mélange aléatoire du passé et du futur.

**2.6. Modèles et indicateurs d'évaluation**

Trois modèles sont comparés. Le modèle de référence reprend le signe du dernier mois connu. La régression logistique estime la probabilité d'un rendement positif à partir des variables historiques, après imputation et standardisation. La Random Forest combine des arbres de décision afin de représenter des relations potentiellement non linéaires ; le notebook utilise 300 arbres et une profondeur maximale de 4.

Les performances sont mesurées par l'accuracy, la précision, le rappel et le F1-score. Ces indicateurs sont complémentaires : l'accuracy mesure la proportion totale de prédictions correctes, tandis que la précision et le rappel se concentrent sur la classe positive. Le F1-score synthétise ces deux dernières mesures.

---

## <u>Chapitre 3 : Mise en œuvre empirique et traitement des données</u>

**3.1. Environnement de travail et outils utilisés**

L'analyse a été réalisée dans Google Colab à l'aide du langage Python. Le notebook utilise notamment pandas pour la manipulation des données, NumPy pour les calculs numériques, Matplotlib pour les visualisations et scikit-learn pour les modèles de classification et les métriques d'évaluation. Le notebook fourni met effectivement en œuvre le traitement et la modélisation en Python.

**3.2. Importation et présentation du jeu de données**

Après importation, le jeu de données contient 1 197 observations et 2 colonnes. Il couvre la période de janvier 1927 à septembre 2026. Cette profondeur historique permet d'observer des configurations de marché variées.

<p align="center"><img src="images/stats-descriptives.png" alt="Statistiques descriptives du facteur Mom" width="380"></p>

La moyenne positive indique une performance moyenne favorable sur l'ensemble de la série, mais l'écart-type et les valeurs extrêmes montrent une forte variabilité. La valeur minimale rappelle que des pertes mensuelles très importantes peuvent survenir.

**3.3. Représentation graphique de l'évolution des rendements**

<p align="center"><img src="images/evolution-momentum.png" alt="Évolution mensuelle du facteur momentum" width="720"></p>

Le graphique met en évidence l'alternance de rendements positifs et négatifs ainsi que plusieurs épisodes de forte variation. La série ne suit donc pas une trajectoire régulière. Cette instabilité justifie l'étude de la prévisibilité et des risques associés.

**3.4. Construction des variables et répartition de la cible**

Après la construction des quatre variables historiques et la suppression des observations incomplètes, le jeu de données exploitable comporte 1 185 observations. La cible est composée de 62,4 % de rendements positifs et de 37,6 % de rendements négatifs ou nuls. Cette répartition est déséquilibrée : un modèle peut obtenir une accuracy apparemment élevée en privilégiant la classe majoritaire.

**3.5. Résultats de classification et comparaison des modèles**

<p align="center"><img src="images/resultats-classification.png" alt="Résultats de classification des trois modèles" width="600"></p>

La régression logistique obtient les meilleurs résultats sur les quatre indicateurs. Son rappel de 73,5 % signifie qu'elle identifie une proportion importante des mois positifs. La Random Forest arrive en deuxième position, tandis que la règle du dernier mois présente l'accuracy la plus faible.

<p align="center"><img src="images/comparaison-accuracy.png" alt="Comparaison de l'exactitude des modèles" width="600"></p>

Une nuance importante concerne l'accuracy : les mois positifs représentent 57,38 % de l'échantillon de test. Un modèle prédisant systématiquement la classe positive obtiendrait donc 57,38 % d'accuracy. La régression logistique atteint 57,8 %, soit seulement 0,42 point de plus. Son avantage statistique doit ainsi être considéré comme modeste.

**3.6. Analyse des matrices de confusion**

Les matrices de confusion permettent de distinguer les vrais positifs, les vrais négatifs, les faux positifs et les faux négatifs. Elles complètent les scores globaux en montrant la nature des erreurs réalisées par chaque modèle.

<p align="center"><img src="images/matrices-confusion.png" alt="Matrices de confusion des trois modèles" width="760"></p>

**3.7. Simulation d'une stratégie d'investissement**

La simulation applique une règle simple : la stratégie est investie sur le facteur Momentum lorsque le modèle prédit un rendement positif et reste en liquidités lorsqu'il prédit un rendement négatif. Un coût de 0,10 % est appliqué à chaque changement de position. Les résultats correspondent à cette hypothèse simplifiée de coûts.

<p align="center"><img src="images/simulation-strategies.png" alt="Résultats de la simulation des stratégies" width="720"></p>

La régression logistique est la seule stratégie dont le rendement cumulé net est positif (+9,78 %). Elle réalise également le plus petit nombre de changements de position. Toutefois, son drawdown maximal atteint −56,52 %, le plus élevé des trois stratégies. Le rendement final positif ne signifie donc pas que le parcours de la stratégie a été peu risqué.

<p align="center"><img src="images/evolution-capital.png" alt="Évolution du capital des stratégies simulées, net de coûts" width="720"></p>

**3.8. Analyse du risque de momentum crash**

Les cinq rendements mensuels les plus défavorables de la période de test sont présentés ci-dessous. Ils illustrent le risque de retournement brutal du facteur Momentum.

<p align="center"><img src="images/pires-rendements.png" alt="Les cinq pires rendements mensuels de la période de test" width="480"></p>

La perte d'avril 2009 (−34,36 %) est particulièrement importante. Elle montre qu'une tendance qui semble persistante peut être suivie d'un retournement marqué. Ce type d'épisode peut affecter fortement le capital, même lorsque la stratégie présente une performance positive à la fin de la période.

**3.9. Synthèse et limites de l'étude**

- L'analyse porte sur un seul facteur financier ; les conclusions ne peuvent pas être généralisées automatiquement à tous les actifs.
- La cible binaire ne tient pas compte de l'amplitude des rendements : une faible hausse et une forte hausse sont classées de la même manière.
- Un seul découpage chronologique est utilisé ; une validation glissante permettrait de tester la stabilité des résultats.
- Les coûts de transaction sont simplifiés et ne représentent pas nécessairement toutes les frictions réelles.
- Les métriques de classification ne mesurent pas directement le risque de pertes extrêmes.

---

# CONCLUSION

> En définitive, ce projet avait pour objectif d'étudier si les performances passées du facteur Momentum permettent de prévoir le signe de son rendement au mois suivant. La démarche a suivi plusieurs étapes : importation des données, exploration descriptive, construction de variables historiques, définition d'une cible binaire, découpage chronologique, entraînement de modèles et évaluation d'une stratégie simulée.
>
> Parmi les trois approches comparées, la régression logistique obtient les meilleurs scores de classification et le seul rendement cumulé net positif (+9,78 %). Ces résultats suggèrent que les rendements passés contiennent une information prédictive, mais l'amélioration de l'accuracy reste faible par rapport à une prédiction systématique de la classe majoritaire.
>
> En outre, le drawdown maximal de −56,52 % observé pour la stratégie logistique met en évidence une exposition importante aux pertes. La qualité d'un modèle ne peut donc pas être appréciée uniquement à partir de son taux de bonnes prédictions ou de son rendement final.
>
> Somme toute, le facteur Momentum présente une capacité prédictive partielle dans les résultats observés, mais cette capacité demeure modeste et ne suffit pas à établir une stratégie robuste sans mécanisme complémentaire de gestion du risque.
