# Carte des entités et des liens potentiels

## 1. Inventaire des entités

| Entité ou type d'entité | Exemple local | Pourquoi s'agit-il d'une entité ? | Attributs associés | Identifiant local potentiel |
| --- | --- | --- | --- | --- |
| Établissement d’enseignement supérieur | Ecole Nationale Superieure d'Informatique et d'Analyse des Systemes | C’est l’entité centrale du dataset, chaque ligne décrit un établissement | institution_name, short_name, website, institution_type, main_field, year_created, city, country | record_id |
| Ville | Rabat | La ville localise l’établissement et peut être liée à un référentiel géographique externe | city, country | nom de la ville |
| Pays | Morocco | Le pays permet de situer les établissements dans un cadre national | country | nom du pays |
| Type d’établissement | public school | Permet de catégoriser les établissements selon leur nature | institution_type | valeur textuelle normalisée |
| Domaine principal | engineering | Représente le champ d’étude principal de l’établissement | main_field | valeur textuelle normalisée |
| Site web | https://www.ensias.um5.ac.ma | Peut servir de ressource descriptive ou d’identifiant secondaire | website | URL |
| Portail source | data.gov.ma | Représente la source de publication du dataset | portal_hint | nom du portail |
| Année de création | 1992 | Fournit une caractéristique temporelle importante de l’établissement | year_created | combinaison avec établissement |
| Nom court | ENSIAS | Représente une forme abrégée utile pour l’identification locale | short_name | short_name |
| Université de rattachement (implicite) | Universite Mohammed V in Rabat | Certaines institutions peuvent être rattachées à une université plus large | institution_name, short_name, website, city | nom officiel |

## 2. Relations conceptuelles observées

| Source | Relation conceptuelle | Cible | Cardinalité | Commentaire |
| --- | --- | --- | --- | --- |
| Établissement | possède un identifiant local | record_id | 1:1 | Chaque établissement a un record_id unique |
| Établissement | a pour nom officiel | institution_name | 1:1 | Le nom officiel identifie l’établissement |
| Établissement | a pour nom court | short_name | 1:1 | Le nom court facilite la lecture |
| Établissement | est situé dans | Ville | N:1 | Plusieurs établissements peuvent être dans la même ville |
| Ville | appartient à | Pays | N:1 | Toutes les villes du dataset appartiennent au Maroc |
| Établissement | possède un site web | Site web | 1:1 | Chaque établissement possède une URL dans cet échantillon |
| Établissement | a pour type | Type d’établissement | N:1 | Plusieurs établissements peuvent partager le même type |
| Établissement | est associé à | Domaine principal | N:1 | Plusieurs établissements peuvent appartenir au même domaine |
| Établissement | possède une année de création | Année de création | N:1 | Plusieurs établissements peuvent avoir la même année |
| Établissement | est publié via | Portail source | N:1 | Toutes les lignes proviennent du même portail |
| Université | localise / regroupe potentiellement | Établissement | 1:N | Relation conceptuelle possible pour les écoles/facultés rattachées |
| Nom court | désigne | Établissement | 1:1 ou N:1 | Utile localement mais pas toujours globalement stable |

## 3. Liens externes proposés

| Entité locale | Ressource externe candidate | Type de lien envisagé | Critères d'appariement | Justification | Bénéfice attendu | Niveau de confiance | Risque |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Ville | GeoNames | correspondance exacte ou proche | nom de la ville + pays | GeoNames est adapté aux entités géographiques | ajout de coordonnées et enrichissement géographique | Moyen | variantes d’écriture : Fes / Fez, Marrakesh / Marrakech |
| Ville | Wikidata | sameAs / closeMatch | nom de la ville + pays | Wikidata contient des identifiants stables pour les villes | enrichissement sémantique | Moyen | homonymie possible |
| Pays (Morocco) | Wikidata | sameAs | nom exact du pays | Le pays est unique et facile à apparier | identifiant international stable | Élevé | faible |
| Pays (Morocco) | DBpedia | sameAs | nom exact du pays | Ressource LOD classique pour les pays | enrichissement avec description et liens | Élevé | faible |
| Établissement | Wikidata | exactMatch ou closeMatch | nom officiel + ville + site web | Certains établissements marocains existent déjà dans Wikidata | récupération d’un URI global | Moyen | homonymie ou absence de ressource |
| Établissement | site officiel comme référence externe | seeAlso | URL du site web | Le site officiel confirme l’identité de l’établissement | validation des correspondances | Élevé | URL pouvant changer |
| Domaine principal | Wikidata | closeMatch | libellé du domaine | Permet de normaliser les champs thématiques | harmonisation sémantique | Moyen | granularité différente |
| Type d’établissement | schema.org / vocabulaire éducatif | closeMatch | catégorie textuelle | Permet d’aligner les types sur un vocabulaire plus standard | meilleure interopérabilité | Faible à moyen | catégories locales pas toujours équivalentes |
| Université de rattachement | Wikidata | exactMatch / closeMatch | nom officiel + ville + site web | Les universités publiques sont souvent référencées | enrichissement institutionnel | Moyen | ambiguïté de nom |
| Portail source | data.gov.ma | seeAlso | nom du portail | Permet de relier le dataset à sa source d’origine | traçabilité | Élevé | faible |

## 4. Schéma conceptuel

```mermaid
graph LR
  Dataset["Jeu de données local"]
  Etab["Établissement"]
  ID["record_id"]
  Short["Nom court"]
  City["Ville"]
  Country["Pays"]
  Type["Type d'établissement"]
  Field["Domaine principal"]
  Year["Année de création"]
  Web["Site web"]
  Portal["Portail source"]
  WD["Ressource externe : Wikidata"]
  GN["Ressource externe : GeoNames"]
  DBP["Ressource externe : DBpedia"]

  Dataset --> Etab
  Etab --> ID
  Etab --> Short
  Etab --> City
  City --> Country
  Etab --> Type
  Etab --> Field
  Etab --> Year
  Etab --> Web
  Etab --> Portal

  Etab -. lien potentiel .-> WD
  City -. lien potentiel .-> GN
  Country -. lien potentiel .-> WD
  Country -. lien potentiel .-> DBP
  Field -. lien potentiel .-> WD