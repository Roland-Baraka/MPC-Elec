# Firmware ESP32 — état du développement

Ce dossier est réservé au futur firmware ESP32. Aucun code de lecture capteur n'est
fourni dans cette archive, car les références exactes des cartes, bibliothèques et
connexions ne sont pas confirmées. Ne pas téléverser un firmware fictif sur une machine.

Étapes à réaliser :
1. Confirmer la carte ESP32 et les références exactes des capteurs.
2. Câbler et tester séparément le MLX90640 et l'ADXL345.
3. Vérifier les mesures avec un instrument de référence.
4. Implémenter le service BLE et le paquet décrit dans `../PROTOCOLE_BLE.md`.
5. Tester pertes de connexion, données invalides, CRC et redémarrages.
