
# PowerShell Snippets for Active Directory Lab

## 🚀 Bulk User Creation
Tento skript vytvoří 10 testovacích uživatelů pro simulaci provozu a sledování logů ve Wazuhu.

```powershell
# =================================================================
# NÁZEV: Bulk_User_Creation.ps1
# POPIS: Hromadné vytvoření 10 testovacích uživatelů v Active Directory.
# POUŽITÍ: Spustit jako Administrator na Domain Controlleru.
# =================================================================

# 1. Definice základních parametrů
$PasswordText = "LabPassword123!" 
$SecurePassword = ConvertTo-SecureString $PasswordText -AsPlainText -Force
$DomainName = (Get-ADDomain).DistinguishedName 

Write-Host "--- Zahajuji vytváření uživatelů v doméně: $DomainName ---" -ForegroundColor Cyan

# 2. Cyklus pro vytvoření 10 uživatelů
foreach ($i in 1..10) {
    $UserName = "TestUser$i"
    $UserPrincipalName = "$UserName@" + (Get-ADDomain).DNSRoot

    try {
        # Kontrola, zda uživatel již neexistuje
        if (!(Get-ADUser -Filter "SamAccountName -eq '$UserName'")) {
            
            New-ADUser -Name $UserName `
                       -SamAccountName $UserName `
                       -UserPrincipalName $UserPrincipalName `
                       -AccountPassword $SecurePassword `
                       -Enabled $true `
                       -ChangePasswordAtLogon $false `
                       -Path $DomainName 

            Write-Host "[OK] Uživatel $UserName byl úspěšně vytvořen." -ForegroundColor Green
        } 
        else {
            Write-Host "[!] Uživatel $UserName již existuje, přeskakuji." -ForegroundColor Yellow
        }
    }
    catch {
        Write-Host "[CHYBA] Nepodařilo se vytvořit uživatele $UserName. Detail: $($_.Exception.Message)" -ForegroundColor Red
    }
}

Write-Host "--- Hotovo! ---" -ForegroundColor Cyan





