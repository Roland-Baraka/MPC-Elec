# Plan de validation terrain — SMART-THERMOÉLEC MP

## Phase A — établir la référence saine
1. Sélectionner un seul équipement pilote (ex. moteur asynchrone) et relever sa plaque signalétique.
2. Obtenir manuel constructeur, classe d'isolation, régime, mode de refroidissement et limites applicables.
3. Vérifier/étalonner le MLX90640 et le capteur d'ambiance ; documenter émissivité, distance et angle.
4. Fixer l'accéléromètre sur un point répétable proche d'un palier ; ne pas comparer des mesures prises à des positions différentes.
5. Enregistrer le microphone avec le même appareil, distance, orientation et réglage. Tant qu'il n'est pas étalonné, enregistrer le niveau relatif en dBFS/RMS, pas en dB SPL.
6. Collecter des mesures à plusieurs charges et vitesses lorsque l'exploitation le permet sans créer de risque.
7. Faire confirmer par un technicien que l'équipement est sain pendant la constitution de la référence.

## Phase B — établir tendance et anomalie
1. Collecter des cas de dérive ou défauts réels, uniquement lorsqu'ils sont constatés et documentés par maintenance.
2. Ne jamais provoquer volontairement une surchauffe ou un défaut pour entraîner l'algorithme.
3. Associer chaque étiquette à un rapport d'inspection/maintenance et à l'avis d'un spécialiste.
4. Définir les seuils par équipement et conditions de charge, puis figer une version du profil.
5. Tester sur des périodes et équipements non utilisés pour régler les seuils.

## Phase C — critères d'acceptation
- température ambiante et température de surface correctement horodatées ;
- unités explicites et absence de mélange dBFS/dB SPL ;
- détection des données manquantes et capteurs déconnectés ;
- courbes lisibles avec zones et unités ;
- mesure de faux négatifs particulièrement surveillée ;
- aucune alerte de sécurité automatique fondée uniquement sur le prototype.

## Priorité produit
Le premier démonstrateur fiable doit afficher la température de surface, l'ambiance, ΔT, l'acoustique relative et l'accélération disponible, avec état de qualité. L'IA supervisée ne vient qu'après collecte et validation des données terrain.
