🍜 Ontologie des Nouilles
==========================

Présentation du projet
---------------------
Ce projet est une ontologie OWL/RDF dédiée aux nouilles asiatiques, couvrant des spécialités représentatives de Chine, Japon et Corée.
Elle modélise de manière systématique :
- les ingrédients
- les techniques de fabrication
- les valeurs nutritionnelles
- les méthodes de cuisson
- les restrictions alimentaires

Ce projet constitue le travail final du cours Web Sémantique et Big-Data.

Auteurs : Yunhan Xue, Ruixing Zheng, Jiatian Liu
Cours   : Web Sémantique et Big-Data
Outils  : Protégé · Python (rdflib) · SPARQL

-----------------------------------------------------

Structure du projet
------------------
.
├── Ontologie_de_nouilles.rdf           # Fichier OWL/RDF (export Protégé)
├── Requetes_Sparql_en_python.ipynb     # Requêtes SPARQL (Jupyter Notebook)
├── Exemple_d_utilisation.pdf           # Documentation des requêtes
├── Rapport_final.pdf                   # Rapport complet du projet
└── README.txt

-----------------------------------------------------

Aperçu de l'ontologie
--------------------
- Classes : 25
- Instances : 42
- Object Properties : 4
- Data Properties : 9

Structure des classes principales :
- Food (classe racine)
  - Nouilles
    - Par façon de cuisine : Soupe_de_nouilles / Nouilles_sautées / Nouilles_sèches
    - Par façon de fabrication : Nouilles_faites_maison / Nouilles_faites_à_la_machine
  - Accompagnements : Fruits de mer / Légumes / Viande
  - Ingrédients :
      - Farine : Farine_gluten (Blé) / Farine_sans_gluten (Sarrasin)
      - Fécule : Patate_douce / Riz
  - Restrictions_de_diète : Aliments_sans_gluten / Aliments_végétariens
  - Valeurs_nutritives : Allergie / Nutritions

-----------------------------------------------------

Propriétés principales
---------------------
Propriété                 | Type                  | Description
---------------------------|---------------------|-------------
est_servi_avec            | Object Property       | Accompagnements associés
a_ingredient_de           | Object Property       | Composition en ingrédients
a_allergie_de             | Object Property       | Allergènes potentiels
a_nutrition_de            | Object Property       | Composants nutritifs
contient_des_calories      | Data Property (int)   | Teneur en calories
est_vegetarien             | Data Property (bool)  | Convient aux végétariens
est_sans_gluten            | Data Property (bool)  | Sans gluten
a_origine_de               | Data Property (str)   | Pays d'origine
a_methode_de_cuisson       | Data Property (str)   | Méthode de cuisson
a_facon_de_fabrication     | Data Property (str)   | Méthode de fabrication
a_saveur_de                | Data Property (str)   | Profil de saveur

-----------------------------------------------------

Prise en main
-------------
1. Installer les dépendances :
   pip install rdflib jupyter

2. Charger l'ontologie et exécuter une requête SPARQL :
   from rdflib import Graph

   g = Graph()
   g.parse("Ontologie_de_nouilles.rdf")

   NS  = "http://www.semanticweb.org/zhengruixing/ontologies/2025/2/untitled-ontology-7#"
   NOU = "http://www.semanticweb.org/zhengruixing/ontologies/2025/2/untitled-ontology-7/"

   query = f"""
   PREFIX ns:  <{NS}>
   PREFIX nou: <{NOU}>

   SELECT ?nouille ?calories
   WHERE {{
       ?nouille nou:contient_des_calories ?calories .
   }}
   ORDER BY DESC(?calories)
   """

   for row in g.query(query):
       name = row.nouille.split("#")[-1] if "#" in row.nouille else row.nouille.split("/")[-1]
       print(f"{name} : {row.calories} kcal")

3. Lancer le Notebook :
   jupyter notebook Requetes_Sparql_en_python.ipynb

-----------------------------------------------------

Point d'attention : problème de double Namespace
-----------------------------------------------
Certaines propriétés et instances utilisent "#" comme séparateur final d'IRI, d'autres "/".
Il est donc nécessaire de déclarer les deux préfixes dans chaque requête :

   PREFIX ns:  <...ontology-7#>
   PREFIX nou: <...ontology-7/>

L'utilisation d'un préfixe incorrect retourne zéro résultat.
Vérifiez l'IRI exact dans Protégé ou le fichier RDF avant d'écrire vos requêtes.

-----------------------------------------------------

Questions de compétence — 14 requêtes SPARQL
-------------------------------------------
1. Quels ingrédients composent un certain type de nouilles ?
2. Combien de calories contient un certain type de nouilles ?
3. Est-ce qu'un certain type de nouilles convient aux végétariens ?
4. Quels types de nouilles sont riches en calories (> 501 kcal) ?
5. Comment cuisiner les nouilles biangbiang ?
6. Quels types de nouilles sont sans gluten ?
7. Quels allergènes les nouilles au sarrasin peuvent-elles contenir ?
8. Quel plat de nouilles est riche en protéines ?
9. Nouilles instantanées vs nouilles au sarrasin : lesquelles sont plus caloriques ?
10. Naengmyeon est souvent servi avec quels accompagnements ?
11. Pour les personnes allergiques aux carottes, quel plat conseiller ?
12. Pour les amateurs de repas lourds (salé, huileux ou pimenté), quelles nouilles choisir ?
13. Dîner avec un ami japonais en étant Chinoise : quels plats choisir ?
14. Il fait très froid aujourd'hui, quel plat pour se réchauffer ?

> Les codes et résultats sont disponibles dans `Requetes_Sparql_en_python.ipynb` et `Exemple_d_utilisation.pdf`.

-----------------------------------------------------

Bibliographie
-------------
[1] Hou, G. G. (2010). Asian Noodles: Science, Technology, and Processing. John Wiley & Sons. doi:10.1002/9780470634370
[2] Staab, S., & Harth, A. (2004). Pizza Ontology. Stanford University.
[3] FoodOntology. FoodOn: The Food Ontology. https://github.com/FoodOntology/foodon
[4] Shao, W. (2014). Chinese Dim Sum Culture. Southeast University Press. ISBN 9787564121280
