# Analyse des ventes en ligne d'une librairie — Lapage

Lapage est une librairie historiquement physique qui a ouvert son site de vente en ligne il y a deux ans. La direction commerciale veut un bilan complet de cette activité pour décider de la marche à suivre : offres, prix, ciblage. Deux notebooks répondent à deux demandes distinctes, celle d'Annabelle (responsable marketing) sur les indicateurs de vente, et celle de Julie (BI analyst) sur cinq corrélations précises concernant le comportement client.

Étude de cas complète, avec démarche et recommandations : [voir sur mon portfolio](https://ahmedelghrairi.github.io/projets/lapage.html)

## Contenu du dépôt

- `01_analyse_ventes_lapage.ipynb` : indicateurs de vente, catalogue, profils clients
- `02_tests_statistiques_lapage.ipynb` : les cinq tests statistiques demandés par Julie
- `donnees_lapage.zip` : les trois fichiers sources (customers.csv, products.csv, Transactions.csv), à décompresser dans le même dossier que les notebooks avant de les exécuter
- `requirements.txt` : bibliothèques utilisées

## Les données

Trois fichiers fournis par OpenClassrooms pour cet exercice : `customers.csv` (8 621 clients, genre et année de naissance), `products.csv` (3 286 références, prix et catégorie), et `transactions.csv` (l'historique des ventes en ligne sur près de deux ans). Le fichier de transactions brut contenait 1 048 575 lignes, dont 361 041 entièrement vides, un artefact d'export Excel, supprimées pour obtenir 687 534 transactions valides. Lapage est une entreprise fictive utilisée pour cet exercice pédagogique : ces données ne concernent aucune personne réelle.

## La démarche

**Nettoyage et fusion.** Les trois tables sont jointes sur les identifiants produit et client, avec vérification qu'aucune transaction ne pointe vers une référence ou un client absent du catalogue. Création des variables dérivées : âge du client, mois de la transaction pour les séries temporelles.

**Analyse commerciale.** Chiffre d'affaires, moyenne mobile, tops et flops par référence, répartition par catégorie, courbe de Lorenz et indice de Gini pour mesurer la concentration du CA, avec un traitement séparé pour les quatre clients BtoB identifiés (des volumes d'achat dix fois supérieurs au reste de la base, probablement des bibliothèques ou des revendeurs).

**Tests statistiques.** Les cinq corrélations demandées par Julie sont testées au niveau client, pas au niveau transaction ni par tranche d'âge agrégée, pour respecter l'indépendance des observations. Les quatre clients BtoB sont exclus de cette partie : leurs volumes auraient faussé les corrélations. Un test a nécessité une correction en cours de route : la taille du panier était d'abord mesurée en nombre d'articles, ce qui donnait un effet faible, alors que la mesurer en euros révèle un effet fort. La leçon retenue est que l'unité de mesure peut complètement changer la conclusion d'un test.

## Quelques résultats

- 12,0 M€ de chiffre d'affaires sur deux ans, mais une croissance de seulement +0,3 % entre la 1re et la 2e année, qui masque un recul de 16 % des transactions, 20 % des produits vendus et 10 % des clients uniques depuis un pic de janvier 2022. La stabilité du CA cache une érosion réelle de l'activité.
- Catalogue très concentré (indice de Gini de 0,74) : une catégorie qui ne représente que 22,5 % des références porte à elle seule la moitié du chiffre d'affaires.
- Le genre n'a aucun effet mesurable sur les catégories achetées (V de Cramer = 0,006) : segmenter le marketing par genre n'a pas de justification statistique.
- La fréquence d'achat semble augmenter avec l'âge en apparence, mais ce lien global disparaît complètement une fois les clients regroupés par tranche d'âge : c'est un paradoxe de Simpson, la vraie rupture se situe autour de 30-32 ans, avec trois profils distincts (jeunes occasionnels, adultes réguliers, seniors fidèles mais au panier réduit).
- Le panier moyen baisse fortement avec l'âge (r = -0,62), un effet bien plus puissant que celui de la fréquence : c'est le vrai levier à activer pour la croissance du CA.

## Outils

Python, pandas, numpy pour l'analyse, matplotlib et seaborn pour les graphiques, scipy et statsmodels pour les tests statistiques.

Ahmed El Ghrairi, 2026.
