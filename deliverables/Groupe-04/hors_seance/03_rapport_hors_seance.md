# Rapport hors séance - TP1

## Page de garde

- **Module** : Linked Open Data
- **TP** : TP1 - Introduction aux données ouvertes liées
- **Groupe** : 04
- **Étudiants** : Salma Tammari + Assmaa EL Hidani
- **Date** : 09/04/2026

---

## 1. Introduction

Dans le cadre du module Linked Open Data, ce travail hors séance a pour objectif de préparer la transformation d’un jeu de données ouvert en données liées. Le jeu de données étudié concerne les établissements d’enseignement supérieur marocains, issu du portail data.gov.ma.

L’objectif principal est d’identifier les entités exploitables, d’évaluer leur capacité à être reliées à des référentiels externes, et de proposer une stratégie de normalisation et d’identification en vue d’une modélisation RDF dans le TP2.

Le problème d’ingénierie des données étudié porte sur l’absence d’identifiants globaux, l’hétérogénéité des valeurs textuelles (villes, types, domaines), ainsi que la difficulté d’apparier automatiquement les données locales à des ressources externes fiables.

---

## 2. Types d'entités étudiés

Trois types d’entités ont été retenus :

### 1. Établissement d’enseignement supérieur
- **Raison du choix** : entité centrale du dataset (chaque ligne représente un établissement)
- **Champs disponibles** :
  - institution_name
  - short_name
  - website
  - city
- **Utilité** : constitue le nœud principal du futur graphe RDF

### 2. Ville
- **Raison du choix** : permet la localisation et l’interconnexion avec des référentiels géographiques
- **Champs disponibles** :
  - city
  - country
- **Utilité** : facilite l’enrichissement avec coordonnées géographiques et hiérarchie territoriale

### 3. Domaine principal
- **Raison du choix** : permet une classification thématique des établissements
- **Champs disponibles** :
  - main_field
- **Utilité** : utile pour relier à des vocabulaires disciplinaires externes

---

## 3. Benchmark des référentiels externes

Deux référentiels principaux ont été étudiés :

### 1. Wikidata
- **Couverture** : élevée pour les villes et pays, moyenne pour les établissements marocains
- **Précision** : généralement bonne, mais dépend de la qualité des contributions
- **Identifiants** : stables (URI )
- **Richesse sémantique** : très élevée (relations multiples, propriétés structurées)
- **Facilité d’utilisation** : bonne via API/SPARQL

 Conclusion : référentiel principal recommandé pour l’alignement

---

### 2. GeoNames
- **Couverture** : excellente pour les villes
- **Précision** : bonne pour la géolocalisation
- **Identifiants** : stables (GeoNames ID)
- **Richesse sémantique** : limitée (principalement géographique)
- **Facilité d’utilisation** : simple

 Conclusion : très adapté pour les entités géographiques uniquement

---

## 4. Cas d'appariement manuel

### Cas 1
- **Entité locale** : Rabat
- **Référentiel cible** : Wikidata
- **Indices utilisés** : nom + pays
- **Confiance** : Élevée
- **Ambiguïté** : faible
- **Décision** : sameAs

---

### Cas 2
- **Entité locale** : Morocco
- **Référentiel cible** : Wikidata / DBpedia
- **Indices utilisés** : nom exact du pays
- **Confiance** : Très élevée
- **Ambiguïté** : aucune
- **Décision** : sameAs

---

### Cas 3
- **Entité locale** : ENSIAS
- **Référentiel cible** : Wikidata
- **Indices utilisés** : nom complet + site web + ville
- **Confiance** : Moyenne
- **Ambiguïté** : possible confusion avec d’autres écoles
- **Décision** : closeMatch

---

### Cas 4
- **Entité locale** : engineering
- **Référentiel cible** : Wikidata
- **Indices utilisés** : libellé du domaine
- **Confiance** : Moyenne
- **Ambiguïté** : différence de granularité
- **Décision** : closeMatch

---

## 5. Plan de normalisation et d'identification

Avant toute transformation en RDF, plusieurs actions sont nécessaires :

### Normalisation des valeurs

- Uniformisation des noms d’établissements (casse, accents, espaces)
- Harmonisation des villes (Marrakesh → Marrakech, Fes → Fès)
- Standardisation des types d’établissement (faculty, school, university)
- Réduction des variations dans main_field

### Règles à appliquer

- Suppression des doublons
- Uniformisation des formats (URL, texte)
- Création de listes de valeurs contrôlées

### Entités nécessitant un identifiant stable

- Établissement
- Ville
- Université de rattachement
- Domaine
- Type d’établissement

### Construction des identifiants (conceptuellement)

- URI basée sur un nom normalisé (slug)
- Exemple :
  - /institution/ensias
  - /city/rabat
  - /field/engineering

---

## 6. Analyse des risques

### Risques de faux appariement

- Noms similaires entre établissements
- Abréviations ambiguës (ex : ENSA)
- Variantes linguistiques des villes

### Désambiguïsation

- Utilisation combinée de plusieurs champs :
  - nom + ville + site web
- Vérification manuelle si nécessaire

### Limites des référentiels

- Wikidata : couverture incomplète pour certains établissements
- GeoNames : limité à la géographie

### Informations manquantes
- coordonnées géographiques
- identifiants officiels
- codes institutionnels

---

## 7. Difficultés rencontrées

Plusieurs difficultés ont été identifiées :

- absence d’identifiants globaux
- valeurs textuelles non normalisées
- ambiguïté des abréviations
- manque de documentation du dataset
- difficulté à trouver des correspondances exactes dans les référentiels

---

## 8. Conclusion

Ce travail hors séance a permis d’identifier les principaux défis liés à la transformation d’un jeu de données ouvert en données liées. Il met en évidence l’importance de la normalisation des données et de la création d’identifiants stables.

Cette étude constitue une étape essentielle pour le TP2, car elle prépare le terrain pour la modélisation RDF et l’alignement avec des référentiels externes, tout en réduisant les risques d’erreurs et d’ambiguïtés.

---
