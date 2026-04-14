# Plan de normalisation et d'identification

## 1. Entités prioritaires

| Type d'entité | Pourquoi est-il prioritaire ? | Identifiant actuel | Stratégie proposée |
| --- | --- | --- | --- |
| Établissement d’enseignement supérieur | C’est l’entité centrale du jeu de données : chaque ligne décrit un établissement. C’est donc l’objet principal à préparer pour une future publication en données liées. | record_id, institution_name, short_name, website | Conserver `record_id` comme identifiant interne temporaire, mais construire un identifiant pérenne fondé sur une URI stable dérivée du nom normalisé de l’établissement, éventuellement consolidée par la ville ou l’université de rattachement. |
| Ville | La ville permet de localiser les établissements et de relier le dataset à des référentiels géographiques externes. | city | Normaliser l’écriture des villes puis préparer un alignement avec un référentiel externe géographique. |
| Université de rattachement | Dans le cas réel, plusieurs établissements sont rattachés à des universités. Cette entité est structurante pour organiser le graphe de données. | nom textuel dans la source | Uniformiser le nom officiel de l’université et prévoir une URI distincte par université. |
| Type d’établissement | Cette information sert à catégoriser les établissements, mais elle est souvent hétérogène et doit être harmonisée avant toute modélisation. | institution_type | Transformer les valeurs textuelles en catégories contrôlées issues d’une nomenclature commune. |
| Domaine principal | Le champ disciplinaire est utile pour l’analyse mais souffre souvent d’ambiguïtés lexicales. | main_field | Réduire les variantes et associer chaque valeur à un libellé de référence stable. |

## 2. Champs à normaliser

| Champ | Problème observé | Règle de normalisation proposée | Impact attendu |
| --- | --- | --- | --- |
| institution_name | Variantes orthographiques, accents absents ou incohérents, majuscules/minuscules non homogènes | Supprimer les espaces superflus, uniformiser la casse, corriger les variantes les plus évidentes, conserver une forme canonique unique par établissement | Réduction des doublons apparents et meilleure construction des URI |
| short_name | Abréviations parfois ambiguës ou partagées par plusieurs établissements | Ne jamais utiliser seul comme identifiant ; le conserver uniquement comme libellé secondaire ou alias | Réduction du risque de collision |
| city | Variantes d’écriture possibles : Fes/Fez, Marrakesh/Marrakech, etc. | Définir une liste canonique des villes et remplacer toutes les variantes par une forme unique | Facilite l’appariement avec des référentiels géographiques |
| country | Dans ce cas, la valeur est stable mais peut varier selon la langue dans d’autres sources | Imposer une seule forme canonique, par exemple `Morocco`, et éventuellement associer un code normalisé | Uniformité et meilleur alignement externe |
| website | Certaines URLs peuvent avoir ou non `http`, `https`, `www`, ou contenir un slash final | Uniformiser les URLs : `https`, suppression des variantes inutiles, suppression du slash final si besoin, contrôle de validité | Améliore la comparaison et la validation des établissements |
| institution_type | Valeurs hétérogènes, parfois en anglais, parfois trop libres | Définir un vocabulaire contrôlé restreint, par exemple : `university`, `faculty`, `school`, `institute`, `research institute` | Rend la donnée comparable et exploitable sémantiquement |
| main_field | Valeurs libres ou trop générales, granularité variable | Regrouper les libellés proches dans une nomenclature cohérente, par exemple : `engineering`, `medicine`, `law`, `multidisciplinary`, `economics` | Prépare l’alignement thématique et diminue l’ambiguïté |
| year_created | Peut être correcte mais parfois non vérifiée ou incomplète | Forcer le type numérique, vérifier les valeurs aberrantes, distinguer si besoin date de création et date d’ouverture | Donnée temporelle plus fiable |
| portal_hint | Champ redondant car identique pour toutes les lignes | Le retirer du niveau établissement et le conserver seulement comme métadonnée de provenance du dataset | Allège le futur graphe et évite la redondance |

## 3. Stratégie d'identifiants

| Entité cible | Base de construction envisagée | Risque | Recommandation |
| --- | --- | --- | --- |
| Établissement | URI construite à partir du nom canonique de l’établissement, éventuellement combiné à la ville ou à l’université | Deux établissements peuvent avoir des noms proches ; risque de variation d’écriture | Utiliser une URI stable de type `.../institution/<slug>` et conserver les autres formes comme labels alternatifs |
| Ville | URI locale ou alignement vers un référentiel géographique externe après normalisation du nom | Homonymie ou variantes linguistiques | Ne lier qu’après validation du pays et de la forme canonique |
| Université | URI construite sur le nom officiel complet | Variantes du nom officiel ou sigles multiples | Préférer le nom complet à l’abréviation |
| Type d’établissement | Identifiant local contrôlé par vocabulaire interne | Catégories locales parfois trop vagues | Définir une petite nomenclature stable et documentée |
| Domaine principal | Identifiant local provisoire ou aligné à un référentiel thématique | Domaine trop large ou trop spécifique selon les cas | Commencer par une normalisation locale avant tout alignement externe |

## 4. Risques et limites

- Quels champs sont trop faibles pour servir d'identifiant ?

`short_name` est trop ambigu, car plusieurs établissements peuvent partager la même abréviation. `record_id` est uniquement interne au fichier et n’a pas de signification hors du dataset. `institution_type` et `main_field` ne sont pas des identifiants mais des catégories.

- Quelles collisions ou ambiguïtés sont possibles ?

Les noms d’établissements peuvent exister sous plusieurs formes orthographiques. Les villes peuvent être écrites différemment selon la langue ou la translittération. Les sites web peuvent changer dans le temps. Certaines écoles peuvent dépendre d’une université plus large, ce qui complique l’identification autonome.

- Quelles informations supplémentaires seraient nécessaires ?

Un code officiel institutionnel, des coordonnées géographiques, un identifiant national pérenne, le nom officiel de l’université de rattachement, ainsi qu’un dictionnaire de données plus précis permettraient de sécuriser la normalisation et la construction d’URI.