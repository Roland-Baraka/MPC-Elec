# Architecture v0.6 — téléphone d'abord, ESP32 optionnel

## Mode standard : téléphone uniquement (coût minimal)
L'application utilise :
- Accéléromètre intégré : variations de mouvement/vibration du téléphone.
- Microphone : niveau RMS et caractéristiques acoustiques à développer.
- Saisie ou import d'une température de référence si elle est disponible.

Limites physiques à afficher à l'utilisateur :
- Le téléphone ne possède généralement pas de thermomètre infrarouge industriel.
- Le microphone ne mesure pas une température; il capte du bruit et peut être perturbé par
  l'environnement, la distance, le boîtier et les autres machines.
- L'accéléromètre du téléphone ne donne des résultats utiles que si le téléphone est fixé
  solidement, dans une orientation répétable et sans risque. Une mesure à la main n'est pas
  une mesure vibratoire normalisée.
- Le mode téléphone seul ne voit pas l'intérieur d'un moteur ou d'un transformateur et ne
  peut pas confirmer une température interne. Il produit uniquement des indicateurs de
  tendance acoustique et de mouvement, à valider par équipement.

## Mode expert optionnel : ESP32 + MLX90640
L'ESP32 est un accessoire facultatif. Il transmet une matrice thermique 32x24 par BLE.
L'application pourra afficher une carte thermique de la surface visible par le capteur.
Cela ne permet pas de voir à travers une carcasse métallique fermée. Pour visualiser
l'intérieur, il faut une ouverture/ligne de visée sûre, une caméra thermique adaptée,
ou des capteurs placés à l'intérieur par une installation conçue et sécurisée par des
professionnels. Ne jamais ouvrir un moteur ou transformateur sous tension.

## Affichage thermique
- palette colorimétrique calculée à partir des températures min/max de la trame;
- valeur min, moyenne et max;
- horodatage et qualité de trame;
- avertissement explicite « température apparente de surface ».

## Déploiement
Le module `PhoneSensorMonitor.kt` expose les capteurs du téléphone.
L'interface doit demander RECORD_AUDIO à l'exécution avant le microphone et libérer
les capteurs à la fermeture.
Le module `ThermalFrame.kt` fournit le modèle de trame et un décodeur fragmenté.
Le firmware ESP32 doit implémenter exactement le protocole fragmenté documenté dans
le code, être adapté au matériel et testé avant que le mode thermique soit annoncé
comme opérationnel.

## Validation scientifique
Collecter des données synchronisées téléphone + instrument de référence sur chaque type
d'équipement. Séparer les jeux d'entraînement/test par machine et par session. Documenter
les distances, le montage, le bruit ambiant, la charge et la vitesse. Sans cette validation,
les résultats restent des indicateurs expérimentaux, pas un diagnostic industriel.
