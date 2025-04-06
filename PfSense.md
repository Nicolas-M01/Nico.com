 
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
### Carte LAN  : ``192.168.20.1/24``  
</details>

<details>
<summary><h1>🎯 Configuration machine Administration<h1></summary>
 
![Capture d'écran 2025-04-06 095143](https://github.com/user-attachments/assets/9771774c-59d6-409b-bc05-6b4972e46c1e)



<details>
<summary><h1>🎯 Création des VLANs sur PfSense<h1></summary>
 
Mon PfSense contient une interrface physique "LAN" pour l'administration, mais aussi pour relier toutes les machines du réseau local. Grâce à la norme IEEE 802.1Q je crée des VLAN pour diviser cette interface en sous interfaces logiques, de fçon à implémenter de la QoS et de la sécurité.  
 ### Cliquer sur `Interfaces->Assignments->VLANs->Add`, sélectionner la bonne carte réseau (LAN) puis paramétrer la carte comme sur les images ci-dessous et sauvegarder

![Capture d'écran 2025-04-05 184352](https://github.com/user-attachments/assets/70a32fb0-7eae-412b-9748-b3c9e81465f4)
![Capture d'écran 2025-04-05 184846](https://github.com/user-attachments/assets/d68e4f80-74e5-49ff-8ef3-05908aacd3c0)

### Voilà à quoi peut ressembler une segmentation d'un réseau en VLANs avec en description, chaque département (qui correspond à une Unité d'Organisation).  
![Capture d'écran 2025-04-05 190924](https://github.com/user-attachments/assets/fa569eec-30be-4d65-9b77-c4756c2ee393)

### Assignation des VLANs à un port :  
Je choisis le `network ports` que que j'ai créé puis je clique sur ``Add``
![Capture d'écran 2025-04-05 191237](https://github.com/user-attachments/assets/cc79273f-082b-48b2-9a63-ff88f4d58b23)

Paramétrage de chaque interface logique : Activation de l'interface, nommage, attribution de l'adresse de passerelle avec son CIDR.  
![Capture d'écran 2025-04-05 195821](https://github.com/user-attachments/assets/af3a5d44-47fa-4fac-8c4f-e9aa6879a1cb)

</details>


Suppression du lan de base pour n'avoir accès que par des VLANs.  

![Capture d'écran 2025-04-06 102641](https://github.com/user-attachments/assets/c2834919-223c-400d-be72-ba573449f1d3)










<details>
<summary><h1>🎯 Règles de filtrage<h1></summary>
A venir...
</details>
