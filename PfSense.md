 
<details>
<summary><h1>🎯 Généralités<h1></summary>
 
Un pare-feu est un outil de défense de première ligne qui surveille le trafic entrant et sortant, et décide d'autoriser ou de bloquer une partie de ce trafic en fonction d'un ensemble de règles de sécurité prédéfinies. Il permet donc de faire du routage également.  
Par défaut, les identifiants sont :  
Username : admin  
Password : pfsense  
Il convient de les changer à la première connexion.  
 
</details>

<details>
<summary><h1>🎯 Cartes réseaux du PfSense<h1></summary>
Nous avons 2 cartes réseaux sur ce FireWall PfSense. Une carte WAN, qui sera du côté internet (mais qui sera reliée à mon routeur box internet) et une carte LAN qui sera côté intérieur, donc avec un réseau privé.
A savoir, pour administrer le FireWall, il est nécessaire de se connecter côté LAN, en se connectant avec l'adresse IP dans l'URL (ou le nom de la machine si enregistrement DNS a été fait).  
 
### Carte WAN  : ``192.168.1.67/24`` 
### Carte LAN  : ``192.168.2.1/24``  
</details>

<details>
<summary><h1>🎯 Création des VLANs sur PfSense<h1></summary>
 
Mon PfSense contient une interrface physique "LAN" pour l'administration, mais aussi pour relier toutes les machines du réseau local. Grâce à la norme IEEE 802.1Q je crée des VLAN pour diviser cette interface en sous interfaces logiques, de fçon à implémenter de la QoS et de la sécurité.  
 ### Cliquer sur `Interfaces->Assignments->VLANs->Add`, sélectionner la bonne carte réseau (LAN) puis paramétrer la carte comme sur les images ci-dessous et sauvegarder

![Capture d'écran 2025-04-05 184352](https://github.com/user-attachments/assets/70a32fb0-7eae-412b-9748-b3c9e81465f4)
![Capture d'écran 2025-04-05 184846](https://github.com/user-attachments/assets/d68e4f80-74e5-49ff-8ef3-05908aacd3c0)

### Voilà à quoi peut ressembler une segmentation d'un réseau en VLANs avec en description, chaque département (qui correspond à une Unité d'Organisation).  
![Capture d'écran 2025-04-05 190924](https://github.com/user-attachments/assets/fa569eec-30be-4d65-9b77-c4756c2ee393)


</details>

<details>
<summary><h1>🎯 Règles de filtrage<h1></summary>
A venir...
</details>
