```powershell

$CSVFile = "C:\Users\Administrator\Desktop\Billu S01.csv"
$CSVData = Import-CSV -Path $CSVFile -Delimiter "," -Encoding UTF8

$CSVData



ForEach ($Utilisateur in $CSVData){

 $UtilisateurPrenom = $Utilisateur.Prenom
 $UtilisateurNom = $Utilisateur.Nom
 $UtilisateurLogin = ($UtilisateurPrenom).Substring(0,1) + "." + $UtilisateurNom
 $UtilisateurEmail = "$UtilisateurLogin@nico.com"
 $departmentOriginal = $Utilisateur.Departement
 $service = $Utilisateur.Service
 $UtilisateurFonction = $Utilisateur.Fonction 
 $UtilisateurMotDePasse = "Azerty1*"


 if (Get-ADUser -Filter {samaccountname -eq $UtilisateurLogin})
 {
  Write-Warning "L'identifiant $UtilisateurLogin existe déjà dans l'AD"
 }
 else
 {
 New-ADUser -Name "$UtilisateurNom $UtilisateurPrenom" `
                    -DisplayName "$UtilisateurNom $UtilisateurPrenom" `
                    -GivenName $UtilisateurPrenom `
                    -Surname $UtilisateurNom `
                    -SamAccountName $UtilisateurLogin `
                    -UserPrincipalName "$UtilisateurLogin@nico.com" `
                    -EmailAddress $UtilisateurEmail `
                    -Title $UtilisateurFonction `
                    -Path "OU=UserTest,DC=nico,DC=com" `
                    -AccountPassword(ConvertTo-SecureString $UtilisateurMotDePasse -AsPlainText -Force) `
                    -ChangePasswordAtLogon $true `
                    -Enabled $true

        Write-Output "Création de l'utilisateur : $UtilisateurLogin ($UtilisateurNom $UtilisateurPrenom)"
 }




}

```powershell
