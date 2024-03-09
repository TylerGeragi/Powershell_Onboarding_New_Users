# Powershell Script for Onboarding New Users to Active Directory
Powershell Script for automated addition of users from text file to Active Directory

# Virtualbox Machines
    Windows 10 Client 
    Windows Server Gui w/ Active Directory

# Create a text file of all names to be added 
![Screenshot 2024-03-09 140419](https://github.com/TylerGeragi/Powershell_Onboarding_New_Users/assets/112214738/04b2f31b-45c1-41cc-9410-2a0899951e5e)

# In Active Directory - 'Users and Computers'
    create an OU for new users "_USERS" for the new accounts to be assigned to

# Run Powershell script in reference to your text file

# Script
    # Set Variables
    $PASSWORD_FOR_USERS   = "Password1"
    $USER_FIRST_LAST_LIST = Get-Content .\names.txt
    # ------------------------------------------------------ #
    
    $password = ConvertTo-SecureString $PASSWORD_FOR_USERS -AsPlainText -Force
    New-ADOrganizationalUnit -Name _USERS -ProtectedFromAccidentalDeletion $false
    
    foreach ($n in $USER_FIRST_LAST_LIST) {
        $first = $n.Split(" ")[0].ToLower()
        $last = $n.Split(" ")[1].ToLower()
        $username = "$($first.Substring(0,1))$($last)".ToLower()
        Write-Host "Creating user: $($username)" -BackgroundColor Black -ForegroundColor Cyan
        
    New-AdUser -AccountPassword $password `
               -GivenName $first `
               -Surname $last `
               -DisplayName $username `
               -Name $username `
               -EmployeeID $username `
               -PasswordNeverExpires $true `
               -Path "ou=_USERS,$(([ADSI]`"").distinguishedName)" `
               -Enabled $true
      }

![Screenshot 2024-03-09 142313](https://github.com/TylerGeragi/Powershell_Onboarding_New_Users/assets/112214738/4a6c1878-0345-44e4-8c7f-d1ee22cc0ce6)

# Confirm New Users Accounts have been properly configured

![Screenshot 2024-03-09 140645](https://github.com/TylerGeragi/Powershell_Onboarding_New_Users/assets/112214738/9541d53c-8928-4203-a509-2c123b818cc0)

