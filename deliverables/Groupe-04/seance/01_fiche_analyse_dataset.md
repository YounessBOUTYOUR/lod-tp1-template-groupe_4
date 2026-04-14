# Fiche d'analyse du jeu de données

## Identification

- **Groupe** :04
- **Date** :09/04/2026
- **Nom du jeu de données** :higher_education_sample.csv
- **Source / producteur** : data.gov.ma
- **URL de la source** :(./higher_education_sample.csv)
- **Domaine métier** :Education / Enseignement supérieur

## Description générale

- **Objectif du jeu de données** :structurer les informations relatives aux établissements d’enseignement supérieur (universités, écoles, instituts)
- **Format(s) disponible(s)** :CSV
- **Langue(s)** :Francais
- **Licence** :  Open Data
- **Fréquence de mise à jour** :liée aux cycle universitaires ( voir Annuelle)
- **Unité d'observation** :institution académique
- **Granularité des enregistrements** :établissement ( niveau organisationnel), chaque ligne représente une institution

## Structure du jeu de données

| Champ / colonne | Signification | Exemple de valeur | Observation |
| --- | --- | --- | --- |
|record_id |identifiantde chaue enregistrement | 1,2.. | unique et de nature discret |
|institution_name|nom d'établissement| Ecole Mohammadia d'Ingenieurs, Ecole Nationale Superieure d'Informatique et d'Analyse des Systemes |de type chaine de caractere et unique  |
|short_name |abréviation du nom d'établissement | EMI,ENSIAS | certaines abréviations peuvent être partagées par plusieurs institutions ou changer dans le temps. Pas fiable comme identifiant unique.  |
|city |nom de la ville ou réside l'établissement | Rabat,Kenitra | Pas de normalisation + Risque d’hétérogénéité si orthographe varie (Marrakesh et Marrakech). |
|country |nom du pays ou réside l'établissement | Morocco(unique) | Valeur unique, donc peu discriminante.|
|website |site web de l'établissement | https://www.emi.ac.ma,https://www.uiz.ac.ma,university| Très utile pour vérification, mais fragile : les sites peuvent changer ou disparaître.|
|institution_type |type de l'établissement | public school,faculty,university| Donnée informative mais hétérogène, mélange de français et anglais, pas de nomenclature officielle. Risque de confusion. | |
|main_field |domaine de formation d'établissement | multidisciplinary,engineering| Valeurs libres, non normalisées. Risque d’ambiguïté   |
| year_created | Année de création | 1992 | Numérique, Donnée fiable mais parfois approximative |
| portal_hint | Source du portail | data.gov.ma | Redondant,donc il pourrait être supprimé ou remplacé par un identifiant du jeu sur le portail. | |




## Évaluation "Open Data"

| Critère | Observation | Score / 5 |
| --- | --- | --- |
| Accessibilité | Disponible en CSV via data.gov.ma | 5 |
| Réutilisabilité | Champs clairs mais pas toujours normalisés | 4 |
| Documentation | Métadonnées limitées, pas de dictionnaire de données | 3 |
| Format exploitable | CSV lisible et structuré | 5 |
| Présence d'une licence | Implicite via portail, pas explicitée | 3 |

## Évaluation "Linked Data readiness"

| Point observé | Oui / Non / Partiel | Commentaire |
| --- | --- | --- |
| Identifiants stables pour les enregistrements | Partiel | record_id interne, pas d’URI |
| Entités clairement distinguables | Oui | Chaque établissement est unique |
| Valeurs normalisées | Partiel | institution_type et main_field hétérogènes |
| Informations géographiques exploitables | Oui | Ville + pays disponibles |
| Possibilité de lien vers des référentiels externes | Oui | Sites web, villes, domaines |
| Présence de clés candidates plausibles | Oui | short_name + city |
| Présence de codes ou nomenclatures réutilisables | Non | Pas de code officiel (ex. MEN) |

## Maturité "5 étoiles"

| Niveau | Atteint ? | Justification |
| --- | --- | --- |
| 1 étoile | Oui | Données publiées sur le Web |
| 2 étoiles | Oui | Structurées en tableau |
| 3 étoiles | Oui | Format CSV non propriétaire |
| 4 étoiles | Non | Pas d’URI unique par entité |
| 5 étoiles | Non | Pas de liens vers référentiels externes |
## Clés candidates et identifiants

| Objet ou entité cible | Champ(s) candidat(s) | Fiabilité | Limites | Recommandation |
| --- | --- | --- | --- | --- |
| Institution | short_name | Bonne | Risque d’homonymie | Ajouter URI stable |
| Institution | website | Bonne | Peut changer | Normaliser via URI persistante |
| Institution | record_id | Moyenne | Purement interne | Remplacer par identifiant pérenne |

## Normalisation préalable nécessaire

| Champ | Problème observé | Impact pour le LOD | Action recommandée |
| --- | --- | --- | --- |
| institution_type | Catégories non homogènes | Difficulté d’alignement | Utiliser nomenclature officielle |
| main_field | Valeurs libres | Ambiguïté | Normaliser via référentiel disciplinaire |
| portal_hint | Redondant | Peu utile | Supprimer ou clarifier |

## Qualité des données

- **Forces observées** : Structure claire, URLs exploitables, cohérence des villes et pays.
- **Faiblesses observées** : Absence de dictionnaire de données, manque de normalisation des champs.
- **Ambiguïtés ou doublons potentiels** : institution_type et main_field.
- **Champs manquants pour une meilleure interconnexion** : Codes institutionnels officiels, coordonnées géographiques.
- **Risque principal pour une future mise en relation** : Absence d’URI pérennes et valeurs non normalisées.
## Conclusion courte

Ce jeu de données constitue une base solide pour une publication en données ouvertes liées, car il est structuré, accessible et couvre un périmètre national pertinent. Toutefois, pour atteindre une maturité LOD, il est indispensable de créer des identifiants pérennes (URI par institution), de normaliser les champs institution_type et main_field selon des référentiels officiels, et d’ajouter des métadonnées plus détaillées (licence explicite, dictionnaire de données). L’intégration de coordonnées géographiques et de codes institutionnels renforcerait l’interopérabilité. En résumé, avec quelques actions techniques ciblées (normalisation, enrichissement, URI), ce jeu de données peut évoluer vers un véritable jeu de données liées interconnecté au niveau international. ces tableau