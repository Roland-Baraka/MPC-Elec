# Compilation APK depuis un téléphone

La voie la plus fiable sans ordinateur reste GitHub Actions :
1. Décompresser l'archive.
2. Créer un dépôt GitHub.
3. Téléverser tout le contenu à la racine, y compris `.github`.
4. Ouvrir Actions > Construire APK SMART-THERMOÉLEC MP.
5. Run workflow.
6. Après réussite, télécharger l'artefact `SMART-THERMOELEC-MP-APK-debug`.
7. Décompresser l'artefact et installer `app-debug.apk`.

Le projet cible Java 17 et Android Gradle Plugin. La compilation doit être vérifiée par
le workflow; cette archive n'a pas été compilée ici.

## Téléphone et micro
L'application doit demander la permission microphone au moment où l'utilisateur active
l'analyse acoustique. Le micro ne doit pas enregistrer ni envoyer la voix; le module fourni
calcule seulement un niveau RMS local. Afficher un indicateur micro actif et permettre l'arrêt.

## ESP32 optionnel
Le mode ESP32 n'est pas nécessaire au fonctionnement de base du mode téléphone.
Il ajoute une visualisation thermique de surface MLX90640. Le téléphone ne peut pas
voir l'intérieur d'une machine fermée avec sa caméra standard.
