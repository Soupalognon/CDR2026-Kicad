# Robot CDF 2026

Carte électronique principale du robot **Wall-A**, conçu pour la **Coupe de France de Robotique 2026**.

<p align="center">
  <img src=".images/PCB%20Robot%20Wall-A%20v1.0%20-%20Top%20Right.png" alt="Vue 3D du PCB Wall-A v1.0" width="85%">
</p>

## Description

Ce PCB regroupe tout ce dont le robot a besoin pour fonctionner :

- **Alimentation** : génère les rails 24 V, 12 V, 5 V et 3,3 V à partir de la batterie
- **Moteurs et actionneurs** : commande des moteurs des roues, des moteurs secondaires et des servomoteurs
- **Capteurs** : encodeurs, capteurs de distance, interrupteurs et sondes de température
- **Contrôle bas niveau** : un microcontrôleur STM32F407 pilote le tout en temps réel
- **Liaison avec le PC embarqué** : USB et Ethernet pour le haut niveau. La prise USB Type-C du PC embarqué est une **sortie d'alimentation 5 V**

### Alimentation

La carte est alimentée par la batterie (LiPo 4S 14.8V, connecteur XT90) avec une tension d'entrée de 16 V à 24 V. Elle génère cinq rails avec trois types de convertisseurs :

| Rail | Type de convertisseur | Composant | Courant | Utilisation |
|---|---|---|---|---|
| 24 V | Élévateur (boost) | Module DC/DC ABB ABXS005 | 5,4 A | Moteurs des roues |
| 12 V | Abaisseur (buck) | TI PTN78020W | 5,8 A | Actionneurs (servomoteurs, ...) |
| 5 V actionneurs | Abaisseur (buck) | TI PTN78060W | 3,5 A | Actionneurs (servomoteurs, pompes, canon, télémètres, Lidar, ...) |
| 5 V calcul | Abaisseur (buck) | TI PTN78060W | 3,5 A | PC embarqué et µC |
| 3,3 V | Régulateur linéaire (LDO) | TI TLV75533P | | Logique du µC |

Les rails 24 V, 12 V et 5 V sont réglables par potentiomètre. Les rails 24 V, 12 V et 5 V actionneurs peuvent être activés par le µC. Les rails 5 V et 12 V sont aussi disponibles en sortie sur connecteurs.

### Contrôleurs moteurs

| Moteurs | Driver | Alimentation | Détails |
|---|---|---|---|
| 2 moteurs principaux (roues) | TI DRV8262, double pont en H | 24 V | Transmotec PD4266-24-17-BFEC, avec encodeurs 5 V |
| 2 moteurs secondaires | ST L298N, double pont en H | 5 V ou 12 V | 2 A maximum en continu par moteur |
| 4 servomoteurs AX12/AX18 | Bus série half-duplex (UART) avec adaptation de niveau 3,3 V / 5 V | 12 V | |
| 4 servomoteurs PWM | Signal PWM direct du µC | 5 V ou 12 V | |

Le µC mesure le courant des deux drivers et la température sous chacun d'eux.

### Capteurs

| Capteur | Quantité | Interface | Détails |
|---|---|---|---|
| Encodeurs | 2 | 5 V | Gauche et droite |
| Capteurs de distance Pulse Width | 4 | Largeur d'impulsion | |
| Capteurs de distance analogiques | 4 | ADC, 0 - 3,3 V | |
| Interrupteurs | 4 | Tout ou rien (on/off) | |
| Sondes de température internes | 3 | Thermistances CTN, ADC | Sous le driver moteur principal, sous le driver moteur secondaire, près des alimentations 12 V et 24 V |

### Communications et debug

| Interface | Détails | Utilisation |
|---|---|---|
| USB FS 12 Mb/s | Connecteur USB Type-C | PC embarqué. Cette prise est aussi une **sortie d'alimentation 5 V** |
| Ethernet 100 Mb/s | PHY LAN8742A + RJ45 | PC embarqué |
| UART 115 kbaud | FT231XS (USB-UART) sur micro-USB | Debug |
| Pin headers | Toutes les broches du µC | Accès direct au µC |
| LEDs | 5 LEDs connectées au µC | Debug |

### Informations générales

Le firmware du microcontrôleur est dans le dépôt [CDR2026-Software](https://github.com/Soupalognon/CDR2026-Software).

|  |  |
|---|---|
| Version | v1.0 (septembre 2025) |
| Dimensions | 170 × 100 mm |
| PCB | 4 couches, 1,6 mm |
| Logiciel | KiCad 9 |
| Auteurs | GDU, THO |

## Images

| Dessus | Dessous |
|:---:|:---:|
| ![Dessus](.images/PCB%20Robot%20Wall-A%20v1.0%20-%20Top.png) | ![Dessous](.images/PCB%20Robot%20Wall-A%20v1.0%20-%20Bottom.png) |
| ![Vue 3D dessus](.images/PCB%20Robot%20Wall-A%20v1.0%20-%20Top%20Right.png) | ![Vue 3D dessous](.images/PCB%20Robot%20Wall-A%20v1.0%20-%20Bottom%20Left.png) |

## Composants principaux

| Fonction | Composant |
|---|---|
| Microcontrôleur | STM32F407IGT6 |
| Alimentation 24V | Module DC/DC ABB ABXS005 |
| Alimentation 12V | TI PTN78020W |
| Alimentations 5V (calcul et actionneurs) | 2 × TI PTN78060W |
| Alimentation 3,3V | TI TLV75533P |
| Driver moteurs principaux | TI DRV8262 |
| Driver moteurs secondaires | ST L298N |
| Ethernet | PHY LAN8742A + transformateur H1102NL + RJ45 |
| USB PC embarqué | Connecteur USB Type-C |
| Debug | FT231XS (USB-UART) sur micro-USB |

La liste complète est dans la BOM : [Robot WallA.xlsx](Robot%20WallA/_output/Robot%20WallA.xlsx).

## Organisation du projet

Le schéma est hiérarchique : la feuille racine contient quatre blocs, eux-mêmes découpés en sous-feuilles.

| Bloc | Sous-feuilles |
|---|---|
| `Power` | `Power_Boost_24V`, `Power_Buck_12V`, `Power_Buck_5V_Actuators`, `Power_Buck_5V_Compute` |
| `MCU` | `MCU_Core`, `MCU_Config`, `MCU_Debug`, `MCU_GPIO`, `MCU_Peripherals` |
| `Motor` | `Motor_WheelDriver`, `Motor_Actuators`, `Motor_Encoders` |
| `Sensors` | |

```
.
├── .images/                 Captures du PCB utilisées dans ce README
├── Docs/                    Documents de référence et configuration de commande JLCPCB
├── Robot WallA/             Projet KiCad
│   ├── Robot WallA.kicad_pro / .kicad_sch / .kicad_pcb
│   ├── _Libraries/          Symboles, empreintes et modèles 3D spécifiques
│   └── _output/             Fichiers de fabrication
│       ├── Fab/ et Fab.zip      Gerbers et perçages
│       ├── Robot WallA.pdf      Schéma
│       ├── Robot WallA.xlsx     BOM
│       └── ibom.html            BOM interactive
├── LICENSE
└── README.md
```

## Fabrication

Les fichiers de fabrication sont dans [Robot WallA/_output](Robot%20WallA/_output). La commande a été faite chez JLCPCB, les réglages utilisés sont dans [Docs](Docs). Le dossier contient aussi la configuration du stencil.

Le dossier [Docs](Docs) contient aussi le schéma de la carte STM32F4 Discovery ([MB1137](Docs/MB1137.pdf)), qui sert de référence pour la partie microcontrôleur.

## Licence

Projet sous licence MIT, voir [LICENSE](LICENSE).
