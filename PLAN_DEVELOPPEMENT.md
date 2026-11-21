# Plan de développement et validation

## Étape 1 — prototype Android
- Compiler et installer l'APK debug.
- Vérifier saisies, seuils, états et ergonomie.
- Corriger les anomalies observées sur le téléphone.

## Étape 2 — capteurs
- Choisir et documenter les capteurs et leur montage.
- Valider les mesures par comparaison à un instrument de référence.
- Définir les incertitudes et les conditions d'utilisation.

## Étape 3 — BLE
- Développer le firmware réel.
- Ajouter scan, connexion, abonnement NOTIFY, décodage et gestion des erreurs.
- Afficher clairement la date, la qualité et la provenance des mesures.

## Étape 4 — historique et alertes
- Ajouter stockage local, export et horodatage.
- Ajouter notifications avec autorisations Android et préférences utilisateur.
- Prévoir fonctionnement hors connexion.

## Étape 5 — analyse avancée
- Constituer un jeu de données étiqueté et traçable.
- Évaluer les algorithmes sur des données séparées d'entraînement et de test.
- Ne parler de prédiction de panne qu'après validation expérimentale.

## Critères de sécurité
- Ne pas inventer de seuil universel : utiliser la plaque et la documentation constructeur.
- Indiquer les données manquantes et les mesures périmées.
- Ne jamais commander une machine ni remplacer les protections réglementaires.
- Faire valider le système par des professionnels compétents avant tout usage industriel.
