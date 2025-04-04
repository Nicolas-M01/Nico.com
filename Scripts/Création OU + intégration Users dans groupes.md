```powershell

# Définition du chemin du fichier CSV
$CSVFile = "C:\Users\Administrator\Desktop\Billu S01.csv"

# Importation des données du fichier CSV
$CSVData = Import-CSV -Path $CSVFile -Delimiter "," -Encoding UTF8

# Définition du chemin de base pour les OU (au niveau du domaine)
$BaseOU = "DC=nico,DC=com"

# Création des OU pour chaque département
# On récupère le fichier et on sélectionne les départements
$Departements = $CSVData | Select-Object -ExpandProperty Departement -Unique

# Analyse du document ligne par ligne
foreach ($Departement in $Departements) {
    # On définit l'emplacement de chaque OU
    $OUPath = "OU=$Departement,$BaseOU"
    # Si l'OU n'existe pas, on rentre dns la condition pour la créer
    if (-not (Get-ADOrganizationalUnit -LDAPFilter "(ou=$Departement)")) {
    # Création OU
    New-ADOrganizationalUnit -Name $Departement -Path $BaseOU -ProtectedFromAccidentalDeletion $false
    # On prévient que l'OU est créée et son emplacement
    Write-Output "OU créée : $OUPath"
    # Si OU déjà existante, on prévient
    } else {
        Write-Output "L'OU $Departement existe déjà."
    }
}

# Création des groupes pour chaque service et ajout à l'OU correspondante
# On récupère le fichier et on sélectionne les départements
$Services = $CSVData | Select-Object -Property Departement, Service -Unique
# Boucle pour analyser ligne par ligne
foreach ($ServiceEntry in $Services) {
    # On rentre dans des variables, les départements et services et définit le DN pour chaque
    $Departement = $ServiceEntry.Departement
    $Service = $ServiceEntry.Service
    $OUPath = "OU=$Departement,$BaseOU"
    $GroupName = "$Service"

    # Si le groupe AD contenant le nom du service n'existe pas on rentre dans la condition
    if (-not (Get-ADGroup -Filter "Name -eq '$GroupName'")) {
        # Création du groupe, prenant le nom du service
        New-ADGroup -Name $GroupName -GroupScope Global -Path $OUPath -PassThru
        # On avertit que le groupe est créé et on donne son nom.
        Write-Output "Groupe créé : $GroupName dans $OUPath"
    } else {
        # On avertit si le groupe existe déjà et on précise lequel
        Write-Output "Le groupe $GroupName existe déjà."
    }
}

# Ajout des utilisateurs aux groupes
# Boucle qui analyse le fichier CSV
foreach ($Utilisateur in $CSVData) {
    # On crée l'utilisateur avec la première lettre de son prénom, suivi d'un "." puis son nom de famille
    $UtilisateurLogin = ($Utilisateur.Prenom).Substring(0,1) + "." + $Utilisateur.Nom
    # On rentre dans une variable le service de l'utilisateur
    $Service = $Utilisateur.Service

    # Condition si l'utilisateur existe dans l'AD en cherchant dans les "samaccountname" de l'AD et "$UtilisateurLogin" nouvellement créé depuis le fichier CSV
    if (Get-ADUser -Filter "samaccountname -eq '$UtilisateurLogin'") {
        # Si le groupe AD contenant le nom du service existe, on rentre l'utilisateur "$UtilisateurLogin" dans ce groupe "$Service"
        if (Get-ADGroup -Filter "Name -eq '$Service'") {
            Add-ADGroupMember -Identity "$Service" -Members $UtilisateurLogin
            # On prévient quel utilisateur a été créé et dans quel groupe
            Write-Output "Ajout de $UtilisateurLogin au groupe $Service"
        } else {
            # Si le "Service" n'existe pas, on prévient
            Write-Warning "Le groupe $Service n'existe pas."
        }
    } else {
        # On informe si l'utilisateur n'existe pas dans l'AD
        Write-Warning "L'utilisateur $UtilisateurLogin n'existe pas dans l'AD."
    }
}

