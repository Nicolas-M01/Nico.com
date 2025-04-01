```powershell
# On rentre dans une variable le chemin du fichier CSV
$CSVFile = "C:\Users\Administrator\Desktop\Billu S01.csv"

# Importation des données du fichier CSV en précisant le délimiteur et l'encodage
$CSVData = Import-CSV -Path $CSVFile -Delimiter "," -Encoding UTF8

# Affichage des données importées
$CSVData


# Boucle pour traiter chaque utilisateur présent dans le fichier CSV
ForEach ($Utilisateur in $CSVData){

 # Récupération des informations de l'utilisateur depuis le fichier CSV
 $UtilisateurPrenom = $Utilisateur.Prenom
 $UtilisateurNom = $Utilisateur.Nom

# Génération du login utilisateur en prenant la première lettre du prénom et le nom, en séparant par un "point"
 $UtilisateurLogin = ($UtilisateurPrenom).Substring(0,1) + "." + $UtilisateurNom

# Création de l'adresse e-mail associée
 $UtilisateurEmail = "$UtilisateurLogin@nico.com"

# Récupération des informations supplémentaires de l'utilisateur (Departement, Service, Fonction)
 $departmentOriginal = $Utilisateur.Departement
 $service = $Utilisateur.Service
 $UtilisateurFonction = $Utilisateur.Fonction

# Définition d'un mot de passe par défaut
 $UtilisateurMotDePasse = "Azerty1*"

# Vérification si l'utilisateur existe déjà dans l'Active Directory
 if (Get-ADUser -Filter {samaccountname -eq $UtilisateurLogin})
 {
# Avertissement si l'utilisateur existe déjà
  Write-Warning "L'identifiant $UtilisateurLogin existe déjà dans l'AD"
 }
 else
 {
 # Création du compte utilisateur dans l'Active Directory avec les informations récupérées
 New-ADUser -Name "$UtilisateurNom $UtilisateurPrenom" `
                    -DisplayName "$UtilisateurNom $UtilisateurPrenom" `
                    -GivenName $UtilisateurPrenom `
                    -Surname $UtilisateurNom `
                    -SamAccountName $UtilisateurLogin `
                    -UserPrincipalName "$UtilisateurLogin@nico.com" `
                    -EmailAddress $UtilisateurEmail `
                    -Title $UtilisateurFonction `
                    -Path "OU=UserTest,DC=nico,DC=com" ` # On envoie l'utilisateur dans l'OU "UserTest"
                    -AccountPassword(ConvertTo-SecureString $UtilisateurMotDePasse -AsPlainText -Force) `
                    -ChangePasswordAtLogon $true `
                    -Enabled $true

 # Affichage d'un message confirmant la création de l'utilisateur
        Write-Output "Création de l'utilisateur : $UtilisateurLogin ($UtilisateurNom $UtilisateurPrenom)"
 }




}
