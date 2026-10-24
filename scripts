<#
.SYNOPSIS
    Populates the Apex Active Directory structure with 5 core departmental OUs, 
    security groups, and bulk-provisions user accounts.
.DESCRIPTION
    Author: System Administrator
    Environment: adlab.test (Windows Server AD DS)
#>

Import-Module ActiveDirectory

$DomainDN = "DC=adlab,DC=test"
Write-Host "[*] Initializing Apex Active Directory Infrastructure Deployment..." -ForegroundColor Cyan

# 1. Create Root Apex OU
$ApexRoot = "OU=Apex,$DomainDN"

try {
    New-ADOrganizationalUnit -Name "Apex" -Path $DomainDN -ProtectedFromAccidentalDeletion $false -ErrorAction Stop
    Write-Host "[+] Created Root OU: Apex" -ForegroundColor Green
} catch {
    Write-Warning "[!] Root OU 'Apex' already exists."
}

# 2. Define and Create the 5 Core Departmental OUs
$Departments = @("Administration", "Development", "Engineering", "FieldService", "Workstations")

foreach ($dept in $Departments) {
    try {
        New-ADOrganizationalUnit -Name $dept -Path $ApexRoot -ProtectedFromAccidentalDeletion $false -ErrorAction Stop
        Write-Host "[+] Created Department OU: $dept" -ForegroundColor Green
    } catch {
        Write-Warning "[!] OU already exists or could not be created: $dept"
    }
}

# 3. Create Core Security Groups inside Administration and Departments
$SecurityGroups = @(
    @{ Name = "GG_Tier1_Server_Admins"; Path = "OU=Administration,$ApexRoot"; Scope = "Global"; Category = "Security" },
    @{ Name = "GG_Tier2_Helpdesk_Admins"; Path = "OU=Administration,$ApexRoot"; Scope = "Global"; Category = "Security" },
    @{ Name = "GG_Development_Users"; Path = "OU=Development,$ApexRoot"; Scope = "Global"; Category = "Security" },
    @{ Name = "GG_Engineering_Users"; Path = "OU=Engineering,$ApexRoot"; Scope = "Global"; Category = "Security" }
)

foreach ($group in $SecurityGroups) {
    try {
        New-ADGroup -Name $group.Name -Path $group.Path -GroupScope $group.Scope -GroupCategory $group.Category -ErrorAction Stop
        Write-Host "[+] Created Security Group: $($group.Name)" -ForegroundColor Green
    } catch {
        Write-Warning "[!] Group already exists: $($group.Name)"
    }
}

# 4. Bulk Provision Users Distributed Across the 5 Departments
Write-Host "[*] Provisioning user accounts into the 5 departmental OUs..." -ForegroundColor Cyan

$FirstNames = @("Jennifer", "Jessica", "Joseph", "Joshua", "James", "Jasmine", "Justin", "Jordan", "Jocelyn", "Jonathan")
$LastNames  = @("Taylor", "Lopez", "Johnson", "Smith", "Williams", "Brown", "Davis", "Miller", "Wilson", "Moore")

$Counter = 1
foreach ($fname in $FirstNames) {
    foreach ($lname in $LastNames) {
        $SamAccountName = "$($fname.ToLower()).$($lname.ToLower())$Counter"
        $UserPrincipalName = "$SamAccountName@adlab.test"
        $DisplayName = "$fname $lname $Counter"
        
        # Distribute users cleanly and cyclically across the 5 departments
        $SelectedDept = $Departments[($Counter - 1) % $Departments.Count]
        $TargetOU = "OU=$SelectedDept,$ApexRoot"

        $SecurePassword = ConvertTo-SecureString "P@ssw0rd2026!" -AsPlainText -Force

        try {
            New-ADUser `
                -Name $DisplayName `
                -SamAccountName $SamAccountName `
                -UserPrincipalName $UserPrincipalName `
                -Path $TargetOU `
                -AccountPassword $SecurePassword `
                -Enabled $true `
                -ChangePasswordAtLogon $false `
                -ErrorAction Stop

            Write-Host "[+] Created User: $DisplayName -> $SelectedDept" -ForegroundColor DarkGreen
        } catch {
            Write-Warning "[!] Failed to create user $SamAccountName : $_"
        }

        $Counter++
    }
}

Write-Host "[+] Apex Infrastructure successfully populated with 5 departments!" -ForegroundColor Green
