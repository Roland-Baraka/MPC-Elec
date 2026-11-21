# IA : état réel et protocole de développement

Aucun modèle entraîné n'est inclus : nous ne disposons pas encore d'un jeu de données
mesurées et étiquetées provenant des équipements ciblés. Inventer un modèle « entraîné »
serait scientifiquement trompeur.

Le code `AnomalyBaseline.kt` est un détecteur statistique de référence (z-score), pas
une IA et pas un prédicteur de panne.

## Données à collecter
Pour chaque échantillon : identifiant pseudonymisé de l'équipement, type, charge, vitesse,
température ambiante, Tmax/Tmean/Tmin, vibration RMS/peak, horodatage, capteur utilisé,
état de fonctionnement et qualité de mesure. Les étiquettes doivent être établies par
un expert : fonctionnement sain, déséquilibre, défaut de roulement, surcharge, etc.

## Entraînement sérieux
1. Collecter des séries sur plusieurs équipements et conditions.
2. Faire valider les étiquettes par un technicien compétent.
3. Séparer les données par équipement et par période (train/validation/test), pas au hasard
   ligne par ligne, afin d'éviter les fuites de données.
4. Comparer une règle de base, un modèle d'anomalie et, si les données le permettent,
   un classificateur supervisé.
5. Mesurer précision, rappel, faux négatifs, faux positifs et robustesse aux capteurs manquants.
6. Exporter le modèle vers TensorFlow Lite seulement après validation, puis documenter
   sa version, ses limites et son domaine d'emploi.

Aucune prédiction de panne ne doit être annoncée avant essais indépendants sur le terrain.
