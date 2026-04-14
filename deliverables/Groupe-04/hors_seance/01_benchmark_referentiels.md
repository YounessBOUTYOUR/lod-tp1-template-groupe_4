# Benchmark des référentiels externes
## Identification

- **Groupe** : Groupe-04
- **Jeu de données étudié** : Établissements de l'enseignement supérieur universitaire public
  ouverts au titre de l'année 2024-2025 — MESRSI / data.gov.ma (XLSX, licence ODbL)
- **Types d'entités retenus** : Établissement, Université (tutelle), Ville

---

## 1. Référentiels comparés

| Référentiel | URL | Types d'entités couverts | Intérêt potentiel | Limites observées |
|---|---|---|---|---|
| **Wikidata** | https://www.wikidata.org | Établissement, Université, Ville, Pays, Discipline | Identifiants stables (Qxxx), multilingue, liens vers DBpedia et Wikipedia, propriétés riches (date de fondation, site officiel, coordonnées) | Couverture incomplète pour les petits établissements marocains ; données parfois non vérifiées ou obsolètes |
| **GeoNames** | https://www.geonames.org | Ville, Région, Pays | Identifiants géographiques stables, coordonnées précises, hiérarchie administrative (ville → région → pays) | Ne couvre pas les établissements eux-mêmes ; uniquement utile pour les entités géographiques |
| **GRID / ROR** | https://ror.org | Établissement de recherche et d'enseignement supérieur | Identifiants institutionnels stables (ROR ID), conçus spécifiquement pour les établissements académiques, alignés avec Wikidata | Couverture partielle pour le Maroc ; certains établissements absents ou mal renseignés |


---

## 2. Critères de comparaison
Les référentiels ont été évalués selon les critères suivants :

**Couverture** : Wikidata couvre la majorité des universités publiques marocaines (UM5, UCA, UIZ…)
mais reste lacunaire pour les établissements composantes (facultés, écoles rattachées).
GeoNames couvre toutes les villes du dataset sans exception. ROR couvre bien les universités
mais peu les grandes écoles et instituts.

**Qualité des identifiants** : Les trois référentiels proposent des identifiants stables et
pérennes. Les QID Wikidata (Q1360237 pour UM5) et les GeoNames ID (2536539 pour Rabat)
sont non ambigus et réutilisables dans des triplets RDF. Les ROR ID (ror.org/019hn4076
pour UM5) sont spécifiquement conçus pour les institutions académiques.

**Précision sémantique** : Wikidata offre la modélisation la plus riche grâce à ses propriétés
typées (P571 = date de fondation, P856 = site officiel, P131 = localisation administrative).
GeoNames est plus limité sémantiquement mais très fiable géographiquement.

**Facilité d'appariement** : L'appariement avec Wikidata se fait par croisement du nom officiel,
du site web et de la date de fondation. GeoNames s'apparie facilement par le nom de ville.
ROR propose une API de recherche par nom qui facilite la vérification.

**Richesse des informations disponibles** : Wikidata est le référentiel le plus riche : il
fournit des libellés multilingues (arabe, français, anglais), des images, des liens vers
Wikipedia et d'autres bases. GeoNames ajoute les coordonnées géographiques et la hiérarchie
administrative. ROR ajoute les affiliations et identifiants croisés (GRID, ISNI).

---

## 3. Cas d'appariement manuel

| Entité locale | Type | Référentiel cible | Correspondance candidate | Indices utilisés | Niveau de confiance | Décision |
|---|---|---|---|---|---|---|
| Université Mohammed V (Rabat) | Université | Wikidata | Q1360237 — "université Mohammed-V" | Nom, ville (Rabat), date de fondation (1957), site officiel (um5.ac.ma) | Élevé | Appariement confirmé |
| Université Ibn Zohr (Agadir) | Université | Wikidata | Q3551418 — "université Ibn Zohr" | Nom, ville (Agadir), date de fondation (1989) | Élevé | Appariement confirmé |
| Université Chouaib Doukkali (El Jadida) | Université | Wikidata | Q12204499 | Nom, ville (El Jadida), date de fondation (1985) | Élevé | Appariement confirmé |
| Université Mohammed Premier (Oujda) | Université | Wikidata | Q3551445 — "université Mohammed-Ier" | Nom (variante orthographique : "Premier" vs "Ier"), ville (Oujda), date (1978) | Moyen | Appariement probabler |
| Rabat | Ville | GeoNames | GeoNames ID 2536539 | Nom exact, pays (Maroc), capitale administrative | Élevé | Appariement confirmé |
| Casablanca | Ville | GeoNames | GeoNames ID 2553604 | Nom exact, pays (Maroc), plus grande ville | Élevé | Appariement confirmé |
| Fès | Ville | GeoNames | GeoNames ID 2295412 | Nom (attention : orthographe "Fes" dans le dataset vs "Fès" dans GeoNames) | Moyen | Appariement probable (normalisation orthographique requise) |
| Université Mohammed V (Rabat) | Université | ROR | ror.org/019hn4076 | Nom, pays, site officiel | Élevé | Appariement confirmé (cohérent avec Wikidata) |

## 4. Synthèse

-Quel référentiel paraît le plus utile pour chaque type d'entité ?

-Pour les établissements et universités Wikidata est le référentiel le plus adapté : il
offre des identifiants stables, une couverture raisonnable des universités marocaines publiques,
et une richesse sémantique. ROR constitue un bon complément, notamment pour les établissements à vocation de recherche, et permet de croiser les identifiants.
-Pour les villes, GeoNames est le choix naturel : couverture totale, identifiants stables,
coordonnées précises et hiérarchie administrative, ce qui permettra d'enrichir le graphe avec des données géographiques structurées.

-Quels cas restent ambigus ?

Deux types d'ambiguïtés ont été relevés. D'abord, les variantes orthographiques : le dataset
utilise des noms sans accents ("Fes", "Mohammed Premier") alors que Wikidata et GeoNames
utilisent les formes normalisées ("Fès", "Mohammed-Ier"). Ces écarts rendent l'appariement
automatique risqué sans pré-normalisation. Ensuite, les établissements composantes sont souvent absents ou mal distingués dans Wikidata, ce qui rend leur appariement incertain : une faculté peut être confondue avec son université de tutelle si le référentiel ne les distingue pas clairement.

-Quels champs du dataset aident le plus à l'appariement ?

Le nom complet de l'établissement est le point d'entrée principal, mais il doit être normalisé
(accents, abréviations). La date de fondation constitue un critère de confirmation très fiable
car elle est stable dans le temps et disponible dans Wikidata. La ville permet de lever des
ambiguïtés entre établissements homonymes dans différentes régions. Le site web officiel est
le critère le plus discriminant lorsqu'il est disponible dans le référentiel cible, car il est
unique et vérifiable directement.
```





