# SYSMON
L'application vit dans la **barre des menus** et ouvre à la demande un tableau de bord .
Elle est pensée pour consommer très peu de ressources : fenêtre fermée, l'interface est détruite et seules les mesures nécessaires à la barre des menus et aux périphériques continuent.

<img width="1664" height="999" alt="Capture d’écran 2026-11-07 à 15 13 26" src="https://github.com/user-attachments/assets/ee736748-7d68-4261-a6e4-d6a5604dd456" />
<img width="1664" height="999" alt="Capture d’écran 2026-11-07 à 15 14 53" src="https://github.com/user-attachments/assets/5ca1bb8f-6f9e-4987-acf0-b687892c2758" />
<img width="1664" height="999" alt="Capture d’écran 2026-11-07 à 15 14 21" src="https://github.com/user-attachments/assets/de08a0a3-60ab-4e8e-ade6-ed71f2b3218f" />
<img width="1719" height="1108" alt="Capture d’écran 2026-11-07 à 20 31 01" src="https://github.com/user-attachments/assets/dcaa424c-fdab-4245-b282-4a993ba004a0" />


Moniteur système et contrôleur de refroidissement pour macOS

# DEEPCOOL AK620 DIGITAL PRO
<img width="1200" height="1200" alt="ak620_digital_pro_1" src="https://github.com/user-attachments/assets/a95e8ec9-0dce-48c4-8b57-256c358bf22b" />

# DEEPCOOL AK620 G2 DIGITAL NYX
<img width="502" height="478" alt="DEEPCOOLAK620G2DIGITALNYX-1" src="https://github.com/user-attachments/assets/9eced368-22be-4400-9a7e-03e26d404a12" />

# ASUS ROG RYUJIN III 360 " ARGB Edition / Noctua Edition"
<img width="525" height="525" alt="fwebp" src="https://github.com/user-attachments/assets/8212b92e-e260-4f40-8115-0410d511dfca" />
<img width="525" height="525" alt="fwebp2" src="https://github.com/user-attachments/assets/38e37852-5af1-4858-9447-e52db025adba" />


### Périphériques
- **ASUS ROG Ryujin III** : courbes de ventilateur et de pompe éditables (profils Silencieux, Équilibré, Performance, Stock, Perso), source de température au choix, hystérésis, et **contenu de l'écran LCD** (3 lignes de texte, 3 fonds).
- **Deepcool AK620 Digital (Pro et G2 NYX)** : envoi de la puissance, de la température, de la charge et de la fréquence à l'afficheur.

### Confort
- **Unités** : °C / °F (aussi pour l'afficheur Deepcool).
- **Cacher les informations sensibles** : numéro de série et adresses matérielles remplacés par des étoiles (utile pour les captures d'écran).
- **4 thèmes** : macOS, ASUS ROG, ASUS ProArt, Graphite — clair et sombre automatiques.
- **11 langues**, changement immédiat : français, anglais, arabe, espagnol, allemand, russe, chinois simplifié, portugais (Brésil), italien, turc, japonais.
- **Cadence d'actualisation** réglable : 1,3 s / 2 s / 3 s.
- Fenêtre redimensionnable, adaptée aux petits écrans.

---

## Prérequis

- **macOS 11 ou plus** (certaines fonctions modernes demandent macOS 12 ou 13 ; l'ancien rendu reste utilisé en repli). Le démarrage automatique (`SMAppService`) demande macOS 13.
- **Capteurs** : les températures, ventilateurs et puissances sont lus via le **SMC**. Sur un Hackintosh, un SMC virtuel avec ses plugins capteurs (par exemple VirtualSMC avec SMCSuperIO) est nécessaire pour obtenir ventilateurs et températures de la carte mère.
- **Charge et fréquence GPU** : lues via `powermetrics`, qui demande une autorisation (règle `sudoers` NOPASSWD proposée au premier lancement).
- **Nom du réseau Wi-Fi (SSID)** : macOS 14 et plus demande l'autorisation de localisation.
- Le refroidisseur et l'AIO sont détectés en USB (HID) ; aucun pilote supplémentaire n'est requis.

---

## Compilation

1. Ouvrir le projet dans **Xcode** (SDK macOS 13 ou plus récent recommandé).
2. Vérifier la signature et l'identifiant de l'app dans *Signing & Capabilities*.
3. Compiler et lancer (`⌘R`).

> Si l'application n'est pas sandboxée, aucune autorisation réseau particulière n'est nécessaire. Avec le sandbox, cocher *Outgoing Connections (Client)* (utilisé par le ping) et l'accès USB.


---

## Faible consommation

- Fenêtre masquée : l'interface SwiftUI est détruite, les mesures purement visuelles sont suspendues, les boucles ralenties.
- Les animations lourdes (cadrans) sont confiées à **Core Animation** ; les zones denses (matrices de points, barres des cœurs) sont dessinées dans un seul `Canvas`.
- Toutes les mesures affichées suivent la même cadence pour limiter les rafraîchissements.
- La mise en page n'est transmise à SwiftUI qu'au plus 10 fois par seconde pendant un redimensionnement.

---

## Ajouter ou corriger une langue

Tous les textes de l'interface sont dans `Localization.swift` : le **texte français du code sert de clé**, et chaque ligne du tableau contient les traductions dans l'ordre *en, ar, es, de, ru, zh, pt, it, tr, ja*, séparées par ` ¦ `. Pour corriger une traduction, modifiez la ligne correspondante ; pour ajouter une langue, ajoutez un cas dans `AppLanguage` (drapeau, nom) et une colonne dans chaque ligne.

> Les traductions ont été rédigées avec l'aide d'une IA : une relecture par des locuteurs natifs est la bienvenue, surtout pour l'arabe, le russe, le chinois, le japonais et le turc.

---

## Limites connues

- Le **ventilateur du Deepcool AK620** n'est pas pilotable : il est alimenté en PWM par l'en-tête de la carte mère, et l'USB de l'AK620 ne gère que l'afficheur. La courbe se règle dans le BIOS.
- L'**écran du Ryujin III** n'affiche que des lignes de texte : les images et les GIF ne sont pas pris en charge par le protocole connu.
- L'état **SMART** peut indiquer « Non pris en charge » sur certains disques NVMe.
- L'afficheur de l'AK620 Pro n'est pas encore alimenté en données.
- Les arguments VoiceOver de certains textes comportant des valeurs variables restent en français.

---

## Remerciements et mentions

- Développé par **B. Abderahmen « Snake »**.
- Le protocole texte de l'écran du Ryujin III s'appuie sur des travaux de rétro-ingénierie communautaires.
- *Deepcool*, *ASUS* et *ROG* sont des marques de leurs propriétaires respectifs. Ce projet n'est affilié à aucune de ces sociétés ; les noms ne servent qu'à indiquer la compatibilité.
