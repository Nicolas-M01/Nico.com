
Les explications de l'administration de l'AD avec Powershell intègre les scripts et le fichier "CSV" dans le dossier `Script` de ce dépôt.

## Introduction  
Une fois le rôle `AD DS` installé, il est nécéssaire d'intégrer les utilisateurs à l'annuaire AD, ainsi que de créer les groupes, intégrer les utilisateurs dans les groupes, créer 1 OU par département et rentrer automatiquement les groupes et utilisateurs dans les bonnes "OU" ( donc dans les bons départements ). Nous commençons à l'aide d'un fichier ".CSV" qui nous permettra de contenir toutes les informations nécéssaires à l'automatisation à l'aide de scripts PowerShell.

## Conversion `.xlsx` en `.csv`  
Si le fichier fourni est un fichier Excel, il sera nécessaire de le convertir en fichier CSV. Voici la méthode :
```Powershell
# Installe le module ImportExcel, qui permet de lire, écrire et manipuler des fichiers Excel (.xlsx) sans nécessiter Excel installé sur la machine  
Install-Module importexcel

# Ici On importe le fichier Excel depuis le chemin spécifié, puis on l'envoie dans une commande qui va exporter au format CSV dans le chemin spécifié, supprimier la première ligne d'information et l'exportation se fait en UTF8 pour conserver les accents et caractère spéciaux.  
Import-Excel -path "C:\Users\Administrator\Desktop\Copy of s01_BillU (1).xlsx" | Export-Csv "C:\Users\Administrator\Desktop\Billu S01.csv" -NoTypeInformation -Encoding UTF8
