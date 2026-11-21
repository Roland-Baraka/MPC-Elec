# Mise en service matérielle — points à confirmer

Avant d'alimenter le montage, confirmer :
- La référence exacte de la carte ESP32 (ESP32, ESP32-S3, fabricant et révision).
- La carte MLX90640 (fabricant, tension logique, fréquence I2C).
- La carte ADXL345 et son adresse I2C (0x53 ou 0x1D).
- Les broches SDA/SCL exactes et l'alimentation 3,3 V compatible.
- La fixation mécanique, l'émissivité, le champ de vision et la distance de mesure.

Le firmware utilise les broches I2C par défaut de la carte. Ce choix n'est pas universel.
Ne jamais alimenter un module 3,3 V en 5 V sans confirmation de sa fiche technique.

Le MLX90640 mesure une matrice de températures infrarouges apparentes. Le maximum d'un
pixel n'est pas automatiquement la température exacte d'un bobinage ou d'un palier :
l'émissivité, le champ de vision, la distance, les réflexions et l'étalonnage influencent
la mesure. L'ADXL345 fournit une accélération; une mesure vibratoire industrielle exige
une fréquence d'échantillonnage et un montage mécanique adaptés.

Faire d'abord les essais sur établi, avec une source de température et un instrument
de référence, sans connexion à une machine en production.
