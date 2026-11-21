# Protocole BLE proposé — non intégré dans v0.4

Service UUID : `6d4f0001-8a3b-4c7d-9e11-001122334455`  
Characteristic UUID : `6d4f0002-8a3b-4c7d-9e11-001122334455`  
Propriétés visées : READ + NOTIFY.

Paquet logique proposé :
- version (uint8)
- sequence (uint32)
- timestamp_ms (uint64)
- Tmax_centiC (int16)
- Tmean_centiC (int16)
- Tmin_centiC (int16)
- accel_rms_mg (uint16)
- accel_peak_mg (uint16)
- status_flags (uint16)
- crc16 (uint16)

Avant intégration, définir précisément l'endianness, les unités, l'échelle, le CRC,
les valeurs invalides, les débordements et la version du protocole. Le firmware
doit transmettre des mesures vérifiées; ne jamais traiter une chaîne DEMO comme mesure.

## Mesures et unités pour l'analyse v0.7
Le protocole BLE actuel transmet des statistiques thermiques et une accélération RMS/peak en mg. Il ne transmet pas encore la température ambiante dédiée, le niveau acoustique du téléphone ni une image thermique complète. Ces canaux doivent être synchronisés et ajoutés au protocole avant une fusion automatique en temps réel.

Ne pas interpréter le niveau RMS brut du microphone comme des dB SPL. Un microphone de smartphone non étalonné fournit un indicateur relatif (dBFS/RMS). La mesure mécanique vibratoire doit provenir d'un accéléromètre fixé de manière répétable.
