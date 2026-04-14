### Ce document contientdra les réponses aux questions du TP1

## Cadrage et Déouverte du TP1
# Quelle différence faites-vous entre une donnée ouverte et une donnée liée ?
Open Data : une donnée librement accessible, téléchargeable et réutilisable (souvent en CSV, JSON, Excel). Elle est ouverte au sens juridique et technique, mais elle reste isolée : elle ne sait pas qu'une autre source parle du même établissement, de la même ville, du même domaine.
Linked Data : une donnée structurée autour d'URIs et de triplets RDF (sujet – prédicat – objet), ce qui permet de la connecter mécaniquement à d'autres sources. Elle n'a pas besoin d'être ouverte pour être liée.
Linked Open Data (LOD) : l'intersection des deux : ouverte et liée.
# Pourquoi un simple fichier CSV ne suffit-il pas pour faciliter l'interconnexion de données ?
Un fichier CSV présente plusieurs limites structurelles :
  - Pas d'identifiant universel : le champ record_id n'est local et n'a aucune signification hors du fichier.
  - Pas de sémantique formelle : institution_type = "public school" est une chaîne de texte libre, non liée à un référentiel partagé. Une autre source écrira peut-être "école publique" ou "Grandes Écoles".
  - Pas de liens entre fichiers : si un autre dataset contient des données sur les étudiants ou les cursus, aucun mécanisme automatique ne permet de les relier à ce CSV.
  - Fragilité à la mise à jour : si l'établissement change de nom, les jointures cassent.
# Quels types d'entités réelles apparaissent déjà dans le jeu de données ?
En analysant les colonnes du CSV, on distingue trois niveaux d'entités. L'entité centrale est l'établissement d'enseignement supérieur, représenté par "institution_name", "short_name", "website", "institution_type" et "year_created". On trouve ensuite des entités géographiques — la ville "city" et le pays ("country") — qui apparaissent comme de simples textes mais renvoient à des entités réelles bien référencées dans GeoNames ou Wikidata. Enfin, "main_field" et "institution_type" sont des valeurs contrôlées qui représentent des concepts réels — disciplines académiques et types d'institutions — mais qui restent non formalisées dans le fichier et nécessiteraient un alignement sur un référentiel partagé pour être exploitables en LOD.
# Quelle est l'unité d'observation du dataset et quel est son niveau de granularité ?
-Unité d'observation : chaque ligne représente un établissement d'enseignement supérieur marocain.Ce qui fait de l'établissement l'unité d'observation centrale.
La granularité est donc macro : on dispose d'une seule entrée par institution, sans descendre au niveau des composantes internes comme les facultés, les départements, les filières ou les programmes. 
# Quels champs pourraient jouer le rôle d'identifiant ou de clé candidate ?
Aucun champ du dataset ne constitue à lui seul un identifiant stable au sens du Linked Data. Le record_id est purement local et arbitraire, le short_name est lisible mais fragile — "ENSA" désigne par exemple plusieurs établissements dans différentes villes — et le website, bien qu'unique et vérifiable, peut changer au fil du temps. Le institution_name souffre quant à lui de variantes orthographiques, notamment l'absence d'accents. La recommandation est donc de construire des URIs intentionnels du type https://data.gov.ma/education/institution/ensias, ou de s'appuyer sur les identifiants Wikidata existants comme ancre externe universelle.\

#### Questions auxquelles vous devez répondre

1. Quelle est la différence entre Open Data et Linked Open Data ?

L'Open Data désigne des données librement accessibles, téléchargeables et réutilisables, généralement publiées sous forme de fichiers tabulaires (CSV, XLSX). Elles sont ouvertes au sens juridique et technique, mais restent isolées : aucun mécanisme ne permet de les connecter automatiquement à d'autres sources. 
Le Linked Open Data (LOD) combine l'ouverture de l'Open Data avec les principes du Web sémantique. Chaque ressource est identifiée par une URI stable, les données sont structurées en triplets RDF (sujet, prédicat, objet) et des liens explicites pointent vers d'autres référentiels. 


2. Quels éléments peuvent être considérés comme des entités centrales ?**

L'entité centrale du dataset est l'établissement d'enseignement supérieur, autour duquel s'organisent tous les autres champs. On identifie également l'université de tutelle comme entité secondaire importante, car 165 établissements sont affiliés à 12 université. La ville et le pays sont des entités géographiques réelles, déjà référencées dans GeoNames et Wikidata. Enfin, le domaine académique(main_field) et le type d'institution (institution_type) sont des entités conceptuelles qui renvoient à des classifications disciplinaires et institutionnelles partageables.


3. Le jeu de données contient-il des identifiants stables et exploitables ?

Non. Le "record_id" est une clé locale arbitraire, sans signification hors du fichier. Le "short_name" est lisible mais non unique à l'échelle nationale. Le "website" est unique et vérifiable mais les URLs ne sont pas conçues comme des identifiants pérennes et peuvent changer. Le nom complet souffre de variantes orthographiques. En l'état, aucun champ ne remplit les critères d'un identifiant LOD : il faudra construire des URIs intentionnels ou s'ancrer sur des identifiants Wikidata ou ROR existants.


4. Quelles ressources externes pourraient enrichir ce jeu de données ?

Plusieurs référentiels sont pertinents. Wikidata peut fournir des identifiants stables pour les établissements et universités, ainsi que des informations complémentaires comme les coordonnées, les logos et les liens Wikipedia multilingues. GeoNames permet de remplacer les valeurs textuelles de "city" par des identifiants géographiques stables avec coordonnées et hiérarchie administrative. ROR (Research Organization Registry) offre des identifiants institutionnels spécifiquement conçus pour les établissements académiques. Le portail data.gov.ma lui-même, déjà cité dans le champ "portal_hint", constitue une source de métadonnées officielles sur les données produites par l'État marocain. 


5. Quels sont les principaux obstacles à une publication en données liées ?

On distingue des obstacles techniques et sémantiques. Sur le plan technique, l'absence d'identifiants stables : il faut concevoir et maintenir un schéma d'URI cohérent. Le format XLSX n'est pas directement transformable en RDF sans étape de conversion et de nettoyage. Sur le plan sémantique, les valeurs de "institution_type' et "main_field" sont des chaînes de texte libres, non alignées sur une ontologie partagée, ce qui empêche l'interopérabilité. Les noms d'établissements sans accents créent des risques de non-correspondance avec les référentiels externes. Enfin, les relations hiérarchiques entre établissements et universités de tutelle ne sont pas représentées dans le dataset, ce qui appauvrirait considérablement le graphe produit sans un travail de reconstruction préalable.


6. Quel serait le bénéfice concret d'une mise en relation avec Wikidata, GeoNames ou un autre référentiel ?

La mise en relation avec Wikidata permettrait à n'importe quelle application ou moteur de recherche de retrouver automatiquement l'université Mohammed V de Rabat lorsqu'elle est mentionnée dans une autre base, sans ambiguïté de nom ou de langue. Le lien avec GeoNames remplacerait la valeur textuelle `"Rabat"` par un identifiant géographique universel, rendant possible des requêtes spatiales : "quels établissements sont situés dans la région Rabat-Salé-Kénitra ?". Un alignement avec ROR faciliterait l'intégration du dataset dans des systèmes internationaux de suivi de la recherche et des publications scientifiques. De façon générale, ces liens transforment un fichier descriptif en un nœud du Web de données, interopérable sans transformation manuelle.


7. Quelle est la granularité exacte du dataset et quel impact sur la modélisation ?

Le dataset est au niveau de l'établissement individuel : chaque ligne représente un établissement unique. Pour le jeu de données réel (MESRSI 2024-2025), cela représente 165 établissements affiliés à 12 universités. Cette granularité macro est suffisante pour une cartographie institutionnelle, mais elle limite la richesse du graphe futur : on ne pourra pas relier directement un chercheur, un diplôme ou un cursus à un établissement sans élargir le périmètre du dataset. Elle impose aussi de modéliser explicitement la relation de tutelle université → établissement composante, qui est absente du fichier mais indispensable à une représentation fidèle du système.


8. Quelles valeurs devraient être normalisées avant toute transformation en données liées ?**

Plusieurs champs nécessitent une normalisation prioritaire. Le champ "institution_name" doit être réécrit avec les accents corrects . Le champ "institution_type" doit être aligné sur une taxonomie contrôlée : les valeurs actuelles sont en anglais et non formalisées, alors que les établissements sont marocains et francophones. Le champ "city" doit être remplacé ou complété par un identifiant GeoNames. Enfin, le champ "year_created" devrait être converti en date ISO 8601 complète si possible, pour être utilisable comme littéral typé en RDF.


9. Quels champs pourraient servir de clé candidate locale, et avec quelles limites ?**

Le "website" est la clé candidate la plus robuste : il est en principe unique pour chaque établissement et vérifiable en ligne. Sa limite principale est que une URL peut changer lors d'une refonte ou d'un changement de domaine. Le 'short_name" est lisible et mémorisable, mais ambigu : "ENSA" désigne plusieurs établissements dans différentes villes. La combinaison "short_name" + "city" améliore la robustesse mais ne garantit pas l'unicité à l'échelle nationale si deux établissements du même type existent dans la même ville. Le "institution_name' complet est trop sujet aux variantes orthographiques pour être fiable sans normalisation préalable. Aucune de ces clés n'est donc utilisable directement comme identifiant LOD sans traitement complémentaire.


10. Quel risque de faux appariement existe pour les liens externes proposés ?**

Plusieurs risques concrets ont été identifiés.
- Le premier est le risque de **confusion entre un établissement et son université de tutelle** : dans Wikidata, ENSIAS peut ne pas avoir d'entrée propre et être confondue avec UM5, son université de rattachement.
 - Le deuxième est le risque lié aux **variantes de noms** : "Université Mohammed Premier" dans le dataset correspond à "Université Mohammed-Ier" dans Wikidata — un appariement automatique par nom exact échouerait. 
 - Le troisième concerne les **homonymes géographiques** : la valeur "Kenitra" dans le champ "city" pourrait, sans identifiant GeoNames, être confondue avec une localité homonyme dans un autre pays. 