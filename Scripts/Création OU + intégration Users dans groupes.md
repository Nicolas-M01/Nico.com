```powershell

# Définition du chemin du fichier CSV
$CSVFile = "C:\Users\Administrator\Desktop\Billu S01.csv"

# Importation des données du fichier CSV
$CSVData = Import-CSV -Path $CSVFile -Delimiter "," -Encoding UTF8

# Définition du chemin de base pour les OU
$BaseOU = "OU=UserTest,DC=nico,DC=com"

# Création des OU pour chaque département
$Departements = $CSVData | Select-Object -ExpandProperty Departement -Unique
foreach ($Departement in $Departements) {
    $OUPath = "OU=$Departement,$BaseOU"
    if (-not (Get-ADOrganizationalUnit -Filter {Name -eq $Departement})) {
        New-ADOrganizationalUnit -Name $Departement -Path $BaseOU -ProtectedFromAccidentalDeletion $false
        Write-Output "OU créée : $OUPath"
    } else {
        Write-Output "L'OU $Departement existe déjà."
    }
}

# Création des groupes pour chaque service et ajout à l'OU correspondante
$Services = $CSVData | Select-Object -Property Departement, Service -Unique
foreach ($ServiceEntry in $Services) {
    $Departement = $ServiceEntry.Departement
    $Service = $ServiceEntry.Service
    $OUPath = "OU=$Departement,$BaseOU"
    $GroupName = "$Service"
    
    if (-not (Get-ADGroup -Filter {Name -eq $GroupName})) {
        New-ADGroup -Name $GroupName -GroupScope Global -Path $OUPath -PassThru
        Write-Output "Groupe créé : $GroupName dans $OUPath"
    } else {
        Write-Output "Le groupe $GroupName existe déjà."
    }
}

# Ajout des utilisateurs aux groupes
foreach ($Utilisateur in $CSVData) {
    $UtilisateurLogin = ($Utilisateur.Prenom).Substring(0,1) + "." + $Utilisateur.Nom
    $Service = $Utilisateur.Service
    
    if (Get-ADUser -Filter {samaccountname -eq $UtilisateurLogin}) {
        Add-ADGroupMember -Identity $Service -Members $UtilisateurLogin
        Write-Output "Ajout de $UtilisateurLogin au groupe $Service"
    } else {
        Write-Warning "L'utilisateur $UtilisateurLogin n'existe pas dans l'AD."
    }
}
