# Wall-A_Kicad
Conception of PCB for robot Wall-A

# Fonctionnalités du PCB
- Entrée d'alimentation entre 16V et 24V
- Aimentations:
    - 5V 3,5A pour le PC embarqué et le µC
    - 5V 3,5A pour tous les types d'actionneurs (servomoteurs, pompes, canon, distances mètre, Lidar, ...)
    - 12V 5,8A pour tous les types d'actionneurs (servomoteurs, ...)
    - 24V 5,4A pour les moteurs des roues
- Moteurs:
    - 2 moteurs principaux
        - Transmotec PD4266-24-17-BFEC
    - 2 moteurs secondaires
        - Alimentation 5V ou 12V
        - Courant maximum en continu par moteurs de 2A
    - 4 servomoteurs type AX12/AX18
    - 4 servomoteurs classiques type PWM
- Capteurs:
    - 2 encodeurs 5V
- Communications:
    - 1 USB FS 12Mb/s pour PC embarqué
    - 1 Ethernet 100Mb/s pour PC embarqué
    - 1 UART 115kbaud pour debug
    - Toutes les broches du µC sorties sur des pinHeaders


# Ce qu'il manque actuellement
- Entrées interrupteurs (des on/off classique)
- savoir combien de capteurs de distance
- sorties d'alimentation 12V et 5V (2 de chaque)
    - Pour un Lidar ou autre
- Entrée interupteurs pour config de stratégie
- LEDS de debug 
- Diode ESD devant toutes les entrées

