 
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
A venir...
</details>

<details>
<summary><h1>🎯 Règles de filtrage<h1></summary>
A venir...
</details>
