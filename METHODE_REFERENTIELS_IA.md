# SMART-THERMOÉLEC MP — Référentiels de fonctionnement et moteur d'analyse (v0.7)

## 1. Objectif

L'application doit comparer les mesures d'un équipement à trois plages propres à cet équipement :

1. **NORMAL** : fonctionnement sain établi par mesures répétées et documentation constructeur ;
2. **TENDANCE** : dérive mesurable par rapport à l'état sain, à surveiller ;
3. **ÉCHAUFFEMENT / ANOMALIE** : dépassement d'une limite technique définie et validée.

Ces plages ne doivent pas être inventées ni copiées d'un moteur à un autre. Elles dépendent notamment du modèle, de la puissance, de la classe d'isolation, du refroidissement, du régime, de la charge, des paliers, de la position de mesure et de l'environnement.

## 2. Grandeurs à acquérir

| Grandeur | Source proposée | Unité / remarque |
|---|---|---|
| Température apparente maximale, moyenne, minimale | ESP32 + MLX90640 | °C, surface visible uniquement |
| Température ambiante | capteur ambiant séparé (ex. SHT31/BME280) ou mesure ambiante fiable du module thermique | °C ; enregistrer séparément de la température de surface |
| Bruit acoustique | microphone téléphone | dB SPL seulement après étalonnage ; sinon dBFS/RMS relatif, jamais présenté comme dB SPL |
| Vibration mécanique | accéléromètre fixé à l'équipement | accélération RMS/peak (mg ou m/s²), puis éventuellement vitesse vibratoire avec méthode validée |
| Contexte | opérateur / automate | marche-arrêt, charge %, vitesse tr/min, fréquence VSD, emplacement et heure |
| Image visible | caméra du téléphone | état extérieur, corrosion, fuite, fumée, ventilation, dommages visibles ; pas une mesure de température |

**Point physique essentiel :** le microphone mesure une pression acoustique, pas directement la vibration mécanique. Le son peut aider à repérer un changement de signature acoustique, mais un vrai indicateur vibratoire exige un accéléromètre correctement fixé et une chaîne de mesure adaptée. Le téléphone posé à proximité ne remplace pas un capteur vibratoire industriel.

Le niveau en dB est logarithmique. Pour une pression acoustique, `Lp = 20 log10(p/p0)` avec `p0 = 20 µPa`. Un microphone de smartphone non étalonné ne donne pas une mesure absolue fiable en dB SPL. Le mode téléphone doit donc afficher « niveau micro relatif (dBFS/RMS) » et établir une référence avec le même téléphone, la même distance, la même orientation et le même environnement. Ne jamais mélanger dBFS et dB SPL.

## 3. Compensation de la température ambiante

La grandeur thermique de suivi principale est l'écart :

`ΔT = T_surface,max − T_ambiante`

Enregistrer également `T_surface,max`, `T_surface,moy`, `T_surface,min` et `T_ambiante` séparément. L'écart ΔT aide à comparer des mesures prises à des ambiances différentes, mais **ne remplace pas** la limite absolue constructeur, la température des enroulements ou la mesure de résistance/sonde intégrée. La température de surface du carter n'est pas la température interne du bobinage.

La température ambiante doit être mesurée près de l'entrée d'air/refroidissement, à l'abri du rayonnement direct et sans contact avec une surface chaude. Le MLX90640 observe des températures apparentes dans son champ de vision ; sa propre température interne ne doit pas être automatiquement assimilée à l'ambiance de la salle. Prévoir idéalement un capteur d'ambiance dédié.

## 4. Construction des trois plages

Pour chaque référence d'équipement, construire un profil avec :

- identification (constructeur, modèle, puissance, tension, vitesse, classe d'isolation, type de refroidissement) ;
- conditions de test (charge, vitesse, fréquence VSD, température ambiante, emplacement/orientation des capteurs) ;
- mesures synchronisées de ΔT, température de surface, acoustique, vibration et contexte ;
- état validé par technicien : sain, à surveiller, défaut confirmé / échauffement confirmé ;
- source de la limite : plaque, manuel constructeur, essai, norme applicable, rapport de maintenance ;
- date, instrument, numéro d'étalonnage et incertitude.

**Normal** : séries répétées d'un équipement déclaré sain, stabilisé, à plusieurs charges et régimes pertinents. Calculer médiane, dispersion (écart-type ou MAD) et quantiles ; ne pas prendre un unique relevé comme référence.

**Tendance** : dérive au-delà de la variabilité saine, mais en deçà de la limite d'alerte validée. Le seuil doit être établi sur les données historiques et confirmé par un spécialiste. La pente temporelle `d(ΔT)/dt` et la variation du signal acoustique/vibratoire sont utiles ; une seule valeur instantanée est insuffisante.

**Échauffement/anomalie** : limite d'alerte issue en priorité du constructeur, de la classe thermique, de la méthode de mesure et des normes pertinentes. Les seuils d'acceptation d'une vibration dépendent du type de machine, puissance, vitesse, support et emplacement ; ils ne sont pas interchangeables avec un niveau sonore en dB.

Ne pas publier de seuil universel « moteur normal = x °C » ou « vibration normale = y dB ». Les valeurs doivent être propres au profil et traçables à leur source.

## 5. Logique de fusion

Le module `ai/ConditionAssessmentEngine.kt` fournit une première classification explicable, pas un réseau neuronal entraîné. Il compare indépendamment :

- thermique : ΔT calculé avec température ambiante ;
- acoustique : niveau dB SPL seulement s'il est étalonné ;
- vibration : accélération RMS si disponible et mesurée correctement.

Il retourne NORMAL, SURVEILLANCE, ALERTE ou DONNÉES INSUFFISANTES, avec le détail de chaque canal et la pente thermique. La règle est volontairement prudente : si le profil n'est pas validé ou si le canal thermique manque, le système ne conclut pas « normal ».

La caméra visible est un canal d'inspection visuelle distinct : elle ne doit pas être fusionnée comme une température. Une photo peut documenter l'état apparent, mais l'identification automatique de défauts nécessitera une base d'images annotées et une validation.

## 6. Courbes à afficher

Sur un même axe temporel, afficher au minimum :

- `T_surface,max` et `T_ambiante` (°C) ;
- `ΔT` (°C) avec trois zones colorées issues du profil validé ;
- acoustique (dB SPL calibré) ou microphone relatif (dBFS/RMS), sur un graphique séparé ;
- vibration RMS (mg ou m/s²), sur un graphique séparé ;
- charge %, vitesse et fréquence VSD en annotations.

Les axes et unités doivent rester séparés. Ne pas additionner directement des degrés Celsius, des décibels et des mg : la fusion s'effectue par classes normalisées et tendances, pas par somme brute.

## 7. Références techniques à consulter

- **IEC 60034-1** : caractéristiques assignées et performances des machines électriques tournantes ; limites thermiques et méthodes de mesure. Utiliser l'édition en vigueur et les données constructeur applicables au moteur précis.
- **ISO 20816-1:2026** : principes généraux de mesure et d'évaluation des vibrations de machines ; les critères propres aux types de machines figurent dans les parties pertinentes de la série. Cette norme distingue notamment les grandeurs de vibration (déplacement, vitesse, accélération) et les conditions/positions de mesure.
- **ISO 20816-3** : guide pertinent pour certaines machines industrielles couplées dans les plages de vitesse et conditions de son domaine d'application ; vérifier l'applicabilité avant d'utiliser des limites.
- **ISO 3744** : méthode de détermination du niveau de puissance acoustique à partir de mesures de pression acoustique dans des conditions définies. Ce n'est pas une table universelle de diagnostic de moteur à partir du micro d'un téléphone.

Les normes complètes sont souvent payantes. Ce document ne reproduit pas leurs tableaux ni ne prétend les remplacer.

## 8. Données minimales et qualité

Chaque ligne d'enregistrement doit inclure : date/heure, équipement, profil/version, état marche, charge, rpm, fréquence VSD, ambiance, Tmax/Tmean/Tmin, ΔT, type de capteur, emplacement, distance/orientation, acoustique et unité, vibration et unité, qualité/étalonnage, opérateur, étiquette experte et source de l'étiquette.

Collecter plusieurs semaines si possible, plusieurs équipements et plusieurs états de charge. Séparer entraînement, validation et test par machine et par période afin qu'une même machine ne se retrouve pas artificiellement dans tous les ensembles. Rapporter précision, rappel, faux négatifs, faux positifs, matrice de confusion et résultats par type de machine.

## 9. Limites et sécurité

- MLX90640 = image thermique de surface en ligne de visée, pas vision à travers une carcasse métallique.
- Caméra RGB = inspection visuelle extérieure, pas thermographie.
- Téléphone seul = indicateurs acoustiques et inertiels exploratoires, pas appareil de diagnostic industriel certifié.
- L'application n'est ni un relais thermique, ni une protection moteur, ni un dispositif de déclenchement.
- Ne jamais ouvrir un moteur, transformateur, VSD ou tableau sous tension pour obtenir une image interne. Les inspections et mesures internes relèvent du personnel qualifié, des procédures de consignation et des équipements appropriés.

## 10. Sources officielles / points de départ

- ISO 20816-1:2026 : https://www.iso.org/standard/89921.html
- ISO 20816-3 (domaine d'application) : https://www.iso.org/obp/ui/?_escaped_fragment_=iso:std:iso:20816:-3:dis:ed-2:v1:en
- ISO 3744:2025 : https://committee.iso.org/cms/live/live/en/sites/isoorg/contents/data/standard/08/08/80866.html
- IEC 60034-1 : consulter le catalogue officiel IEC pour l'édition en vigueur et le manuel constructeur.
