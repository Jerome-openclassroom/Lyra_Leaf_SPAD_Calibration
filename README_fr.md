# 🌿 Calibration SPAD Feuilles Lyra

> 📛 **Projet Lyra** – Science participative augmentée • 🌱 Écologie reproductible • 🧠 Calibration IA • 🧪 ImageJ + Scanner • 📷 SPAD sans capteur propriétaire

## 🌿 Objectif
Ce projet démontre une méthode complète, peu coûteuse et reproductible pour estimer la teneur en chlorophylle (valeurs de type SPAD) à partir d’images de feuilles scannées.  
L’approche combine :

- Un scanner à plat  
- ImageJ (Fiji)  
- Mesures manuelles SPAD (SPAD-502 Minolta)  
- Une mire de calibration RVB utilisée pour valider la densité optique numérique dans le temps  
- Modélisation statistique (régression)  

L’objectif est de démocratiser le diagnostic physiologique des plantes en utilisant uniquement des outils libres et des dispositifs courants, sans dépendance à des capteurs propriétaires.

---

## 🧭 Méthodologie (Chronologie complète)

### 1. Évaluation visuelle de la mire de calibration
- Une mire RVB type Kodak imprimée en 2009 a été récupérée.  
- L’échelle verte verticale (0 à 100) était visuellement intacte et contrastée.  
- Une inspection visuelle en lumière du jour a confirmé l’absence de décoloration apparente.  
- Un scan détaillé en 600 dpi PNG a été utilisé pour évaluer l’intégrité des canaux.

### 2. Validation de la stabilité des couleurs dans le temps
- La mire RVB a été rescannée.  
- 10 plages (10 à 100) ont été extraites.  
- Les valeurs d’intensité des pixels du canal vert ont été analysées avec Python.  
- Une dégradation très linéaire a été confirmée entre les plages 10 et 100.  
- Une attention particulière a été portée aux deux dernières plages (90–100) : le scanner a clairement capté une différence de 5 points dans les valeurs G, indétectable à l’œil nu.

### 3. Comparaison historique avec les données de densitomètre 2009
- En 2009, des mesures de densité avaient été réalisées sur la même mire avec un densitomètre canal jaune.  
- Ces valeurs ont été tracées et comparées au canal vert du scanner en 2025.  
- Après normalisation, les deux courbes étaient presque identiques (R² ≈ 0.99).  
- Cela a confirmé que le scanner est un substitut viable pour la calibration de densité optique au niveau terrain.

### 4. Validation manuelle avec ImageJ et Excel
- L’image RVB a été convertie en niveaux de gris.  
- Chaque plage a été analysée manuellement avec ImageJ (`freehand selections` + `Measure`).  
- Les données ont été exportées dans Excel.  
- Un ajustement polynomial d’ordre 2 a été effectué.  
- La courbe a confirmé la linéarité et la sensibilité des niveaux de gris sur l’échelle verte.

---

## 🍋 Expérience in vivo (Citronnier)

### 🟢 Échantillonnage
- 5 feuilles ont été collectées sur un seul citronnier en pot (stades variés : saine, en maturation, chlorotique).  
- Chaque feuille a été scannée en parallèle de sa mesure SPAD notée manuellement.  
- 3 à 6 lectures SPAD par feuille ont été moyennées.

### 🔬 Mesure
- Chaque feuille a été analysée avec ImageJ en calibrant l’image → DO verte.  
- Des ROIs ont été tracés manuellement pour correspondre aux zones foliaires.

### 📊 Jeu de données collecté
| DO vert (ImageJ) | SPAD (moyenne) |
|------------------|----------------|
| 227.9            | 18.1           |
| 322.5            | 27.0           |
| 384.0            | 32.0           |
| 323.3            | 30.0           |
| 481.4            | 56.0           |

### 📈 Modèle obtenu
- Régression polynomiale (ordre 2) : R² = 0.9705  
- Le modèle capture avec précision la non-linéarité de la saturation en chlorophylle  
- Permet l’extrapolation de DO → SPAD sur la plage typique 15–60

---

## 🖼️ Résultat

![Modèle SPAD-DO verte](result/Modele_SPAD_DO_verte.png)

Ce graphique montre l’ajustement par régression obtenu à partir de cinq échantillons de feuilles scannées et mesurées.  
Malgré le faible nombre de points, le modèle reflète le comportement physiologique de la concentration en chlorophylle : lente montée aux faibles DO, augmentation rapide dans la plage intermédiaire, puis saturation au-delà de DO ~450.  
Cette courbe sera utilisée pour simuler des données synthétiques supplémentaires pour l’entraînement Lyra_Leaf.

---

## 🤖 Intégration dans l’écosystème Lyra
- Ce protocole est compatible avec Lyra_Leaf et les pipelines IA de science participative.  
- Il permet l’entraînement de modèles basés sur GPT à partir de paires structurées SPAD–DO.  
- L’approche est peu coûteuse, reproductible, transparente et indépendante des capteurs propriétaires.

---

## 📂 Structure du Dépôt
```text
Lyra_Leaf_SPAD_Calibration/
├── Calibration/
│   ├── Corrélation gris vert imageJ.png
│   ├── Mesures_niveau_gris_mire.xlsx
│   ├── calibration ImageJ.txt
│   ├── calibration mire image J_Jérôme.xlsx
│   ├── comparaison densitomètre scanner.png
│   ├── densite_verte_calibration_mire_Lyra.xlsx
│   ├── densitométrie mire 2009.jpg
│   ├── gradation RVB sur la mire.png
│   ├── mire 600 dpi.png
│   ├── mire Kodack 100 dpi.jpg
│   └── mire densité gris.png
│
├── En Français/
│   ├── Lisez moi.pdf
│   └── Validation scanner et mire_Fr.pdf
│
├── data/
│   ├── Citronnier couleur.jpg
│   └── Citronnier greyscale.jpg
│
├── result/
│   ├── Modele_SPAD_DO_verte.png
│   └── SPAD DO Vert.xlsx
│
├── README.md
└── README_fr.md
```

---

## 🧠 Points clés à retenir
- Une mire RVB vieille de 15 ans peut encore servir à la calibration optique  
- Les scanners à plat offrent une résolution et une fidélité suffisantes pour détecter des différences indétectables à l’œil nu  
- L’analyse manuelle avec ImageJ est cohérente avec l’automatisation Python  
- Des diagnostics de type SPAD sont possibles sans aucun outil propriétaire  

Ce dépôt démontre la validité scientifique des **diagnostics écologiques participatifs** et ouvre la voie au **suivi peu coûteux de la chlorophylle**, prêt pour l’IA et l’intégration terrain.

---

🔗 **Projet lié** :  

- [Lyra_LowCost_Soil_Leaf](https://github.com/Jerome-openclassroom/Lyra_LowCost_Soil_Leaf) – Modèle intégré sol-feuille à faible coût pour la productivité primaire terrestre.  
- [TurbiditySensor_OpenScience](https://github.com/Jerome-openclassroom/TurbiditySensor_OpenScience) – Estimation optique de la turbidité aquatique et de la productivité primaire à l’aide de capteurs open source.  
- [Leaf_Chlorose_CNN_Training](https://github.com/Jerome-openclassroom/Leaf_Chlorose_CNN_Training) – Classification CNN des feuilles chlorotiques vs. saines à partir d’images scannées.  
- [Lyra_DO_Green_Mesurim](https://github.com/Jerome-openclassroom/Lyra_DO_Green_Mesurim) - Protocole low-tech combinant MesurimPro et ImageJ pour estimer les niveaux de chlorophylle à partir de feuilles scannées, avec validation par mesures SPAD et analyse corrélative assistée par IA.  
- [AI_Assisted_Lake_Ecology](https://github.com/Jerome-openclassroom/AI_Assisted_Lake_Ecology) – Modèle NPP à grande échelle combinant mesures de terrain, modélisation physique et interprétation écologique assistée par GPT-4o. Inclut une correction empirique pour une productivité annuelle réaliste dans les lacs clairs.  
- [LimonTree_NPP_Model](https://github.com/Jerome-openclassroom/LimonTree_NPP_Model) — Modèle hydrique et NPP low-cost pour un citronnier en pot.  
- [Mountain_Bocage_Soil_Analysis](https://github.com/Jerome-openclassroom/Mountain_Bocage_Soil_Analysis) — Jeu de données complet pour un site bocager de moyenne montagne (Haute-Loire, France).  
- [Lyra_Sentinel_MODIS_Site_HauteLoire](https://github.com/Jerome-openclassroom/Lyra_Sentinel_MODIS_Site_HauteLoire) — Données NDVI Sentinel-2 et LST MODIS pour un site semi-naturel en Haute-Loire.  
- [Eco_Profile_Saint_Julien_1060](https://github.com/Jerome-openclassroom/eco-profile-saint-julien-1060) — Site éco-climatologique de 1 ha (1060 m, Massif Central) avec relevés 2017–2018.  
- [Lyra_Botanical_Protocol](https://github.com/Jerome-openclassroom/lyra-botanical-protocol) — Protocole probabiliste basé sur GPT pour l’identification des espèces végétales à partir de photos et de contexte (sud de la France, flore printanière).  

---

© Jérôme-X1, 2025
