# Windows Automation
## Install [WinGet](https://github.com/microsoft/winget-cli) Package Manager

Windows Server 2022:

Install dependencies of WinGet: https://learn.microsoft.com/en-us/windows/package-manager/winget/#install-winget-on-windows-sandbox


Download WinGet with its license file:

```powershell
$winget_version = "v1.7.11261"
$winget = "Microsoft.DesktopAppInstaller_8wekyb3d8bbwe.msixbundle"
$winget_license_file = "64b03c63cd5d4be8a28e8a271ff853cc_License1.xml"
$winget_release_url = "https://github.com/microsoft/winget-cli/releases/download"

$winget_url = $winget_release_url + "/" + $winget_version + "/" + $winget
$winget_license_url = $winget_release_url + "/" + $winget_version + "/" + $winget_license_file

Invoke-WebRequest -Uri $winget_url -OutFile $winget
Invoke-WebRequest -Uri $winget_license_url -OutFile $winget_license_file
```

Install WinGet:

```powershell
Add-AppxProvisionedPackage -Online -PackagePath .\$winget -LicensePath .\$winget_license_file -Verbose
```

Check if WinGet properly installed and fully functional

```powershell
winget --info
```

Upgrade all installed packages before starting to install other packages

```powershell
winget upgrade --all
```

Accept all agrements

```powershell
winget --accept-source-agreements
```

Useful packages:

- `Microsoft.PowerShell`
- `Microsoft.WindowsTerminal`
- `Microsoft.OpenSSH.Beta`
- `Git.Git`
- `cURL.cURL`
- `jqlang.jq`
- `Python.Python.3.12`
- `EclipseAdoptium.Temurin.21.JDK`
- `Hashicorp.Vagrant`
- `Hashicorp.Packer`
- `Hashicorp.Terraform`
- `Pulumi.Pulumi`
- `Neovim.Neovim`
- `VSCodium.VSCodium`
- `Mozilla.Firefox`
- `TigerVNCproject.TigerVNC`
- `GnuPG.GnuPG` or `GnuPG.Gpg4win`
- `aria2.aria2`
- `GnuWin32.Tar`
- `Meta.Zstandard`
- `7zip.7zip`
- `Amazon.AWSCLI`
- `Microsoft.AzureCLI`
- `Microsoft.Azure.AZCopy.10` ?
- `OpenVPNTechnologies.OpenVPN`
- `Docker.DockerDesktop` or `Docker.DockerCLI` and `Docker.DockerCompose`
- `RedHat.Podman` or `RedHat.Podman-Desktop`
- `MSYS2.MSYS2`
- `Microsoft.VisualStudio.2022.Community`
- `Microsoft.VisualStudio.2022.BuildTools`

## Install Powershell Core

```powershell
winget install Microsoft.PowerShell
```
Check if Powershell Core was correctly installed:

```powershell
$PSVersionTable
```

## Install Hyper-V

```powershell
Install-WindowsFeature -Name Hyper-V -IncludeManagementTools -Restart
```

Check if Hyper-V role is installed

```powershell
Get-WindowsFeature -Name Hyper*
```

## Install WSL

```powershell
wsl --install --no-distribution
```
Reboot the system after the installation.

Update all WSL components.

```powershell
wsl --update
```

Make sure that WSL properly installed and functional.

```powershell
wsl --version
```

Exptected output.

```powershell
PS C:\Users\Administrator> wsl --version
WSL version: 2.1.5.0
Kernel version: 5.15.146.1-2
WSLg version: 1.0.60
MSRDC version: 1.2.5105
Direct3D version: 1.611.1-81528511
DXCore version: 10.0.25131.1002-220531-1700.rs-onecore-base2-hyp
Windows version: 10.0.20348.2527
```

Install AlmaLinux OS 9 WSL2 image
```powershell
PS C:\Users\Administrator> winget show "AlmaLinux 9"
Found AlmaLinux 9 [9P5RWLM70SN9]
Version: Unknown
Publisher: AlmaLinux OS Foundation
Publisher Url: https://almalinux.org/
Publisher Support Url: https://almalinux.org/
Description:
  AlmaLinux 9 for WSL, all your Linux needs right within Windows. Leverage the power of the EL ecosystem and power all your development and deployment workflows using the distribution you know and love.


  AlmaLinux is the Community Owned and Governed Enterprise-Grade Linux OS and is 1:1 RHEL and CentOS compatible.
License: ms-windows-store://pdp/?ProductId=9P5RWLM70SN9
Privacy Url: https://almalinux.org/p/privacy-policy/
Agreements:
  Category: Developer tools
  Pricing: Free
  Free Trial: No
  Terms of Transaction: https://aka.ms/microsoft-store-terms-of-transaction
  Seizure Warning: https://aka.ms/microsoft-store-seizure-warning
  Store License Terms: https://aka.ms/microsoft-store-license
Installer:
  Installer Type: msstore
  Store Product Id: 9P5RWLM70SN9
```

Error on installation with `winget`:
```poweshell
An unexpected error occurred while executing the command:
0x803fb104 : unknown error
```

Offline installtion:
```powershell
Invoke-WebRequest -Uri https://wsl.almalinux.org/9/AlmaLinuxOS-9_9.3.0.0_x64.appx -OutFile AlmaLinuxOS-9_9.3.0.0_x64.appx
Import-Module -Name Appx -UseWindowsPowerShell
Add-AppPackage .\AlmaLinuxOS-9_latest_x64.appx
```

Open installed "AlmaLinux OS 9" application to install the distribution.

```powershell
wsl --list --all
wsl -l -v
wsl --set-default
```


on AlmaLinux OS WSL:

```sh
dnf clean all && dnf -y upgrade
# exit and back
sudo dnf config-manager --set-enabled crb
sudo dnf -y install epel-release && sudo dnf -y upgrade
sudo dnf -y install ansible-core git-core zstd
```

## Container Engines

After the "Install WSL" the system is fully ready to install a container engine.

- `Docker.DockerDesktop` or `Docker.DockerCLI` and `Docker.DockerCompose`
- `RedHat.Podman` or `RedHat.Podman-Desktop`



- The "Use the WSL 2 based engine" should be ticked already on Settings >> General.
- Put a tick on "Start Docker Desktop when you sign in to your computer" checkbox on Settings >> General.


To check if Docker Desktop properly installed and fully functional.

```poweshell
docker info
```

Enable multi-arch support for `arm64`, `ppc64le` and `s390x`.

```powershell
PS C:\Users\Administrator> docker run --privileged --rm tonistiigi/binfmt --install arm64,ppc64le,s390x
Unable to find image 'tonistiigi/binfmt:latest' locally
latest: Pulling from tonistiigi/binfmt
e9c608ddc3cb: Download complete
8d4d64c318a5: Download complete
Digest: sha256:66e11bea77a5ea9d6f0fe79b57cd2b189b5d15b93a2bdb925be22949232e4e55
Status: Downloaded newer image for tonistiigi/binfmt:latest
installing: arm64 OK
installing: ppc64le OK
installing: s390x OK
{
  "supported": [
    "linux/amd64",
    "linux/arm64",
    "linux/riscv64",
    "linux/ppc64le",
    "linux/s390x",
    "linux/386",
    "linux/mips64le",
    "linux/mips64",
    "linux/arm/v7",
    "linux/arm/v6"
  ],
  "emulators": [
    "WSLInterop",
    "WSLInterop-late",
    "aarch64",
    "arm",
    "mips64",
    "mips64le",
    "ppc64le",
    "qemu-aarch64",
    "qemu-ppc64le",
    "qemu-s390x",
    "riscv64",
    "s390x"
  ]
}
```

### Windows Containers

Switch to windows containers on the system tray icon of Docker Desktop

```sh
docker run mcr.microsoft.com/windows/server:ltsc2022
```

## Install Ansible

### To directly call ansible outside of WSL
#### Powershell Alias

This aproach only works inside the Powershell.

```powershell
function Wsl-Ansible {wsl ansible}
Set-Alias -Name ansible -Value Wsl-Ansible

function Wsl-Ansible-Connection {wsl ansible-connection}
Set-Alias -Name ansible-connection -Value Wsl-Ansible-Connection

function Wsl-Ansible-Config {wsl ansible-config}
Set-Alias -Name ansible-config -Value Wsl-Ansible-Config

function Wsl-Ansible-Console {wsl ansible-console}
Set-Alias -Name ansible-console -Value Wsl-Ansible-Console

function Wsl-Ansible-Doc {wsl ansible-doc}
Set-Alias -Name ansible-doc -Value Wsl-Ansible-Doc

function Wsl-Ansible-Galaxy {wsl ansible-galaxy}
Set-Alias -Name ansible-galaxy -Value Wsl-Ansible-Galaxy

function Wsl-Ansible-Inventory {wsl ansible-inventory}
Set-Alias -Name ansible-inventory -Value Wsl-Ansible-Inventory

function Wsl-Ansible-Playbook {wsl ansible-playbook}
Set-Alias -Name ansible-playbook -Value Wsl-Ansible-Playbook

function Wsl-Ansible-Pull {wsl ansible-pull}
Set-Alias -Name ansible-pull -Value Wsl-Ansible-Pull

function Wsl-Ansible-Vault {wsl ansible-vault}
Set-Alias -Name ansible-vault -Value Wsl-Ansible-Vault
```

https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_automatic_variables?view=powershell-7.4#profile

```powershell
Get-Variable profile | Format-List
nvim C:\Users\Administrator\Documents\PowerShell\Microsoft.PowerShell_profile.ps1
```

#### Wrapper in batch file

`ansible.bat`
```bat
@echo off
REM "alias" for packer ansible provisioner running in cmd/powershell
set args=%*
REM replace backslashes with slashes
call set args=%%args:\=/%%
REM fix drive letters
call set args=%%args:C:=/mnt/c%%
REM call set args=%%args:D:=/mnt/d%%
REM call set args=%%args:E:=/mnt/e%%

wsl -d AlmaLinuxOS-9-WSL1 ANSIBLE_PIPELINING=%ANSIBLE_PIPELINING% ANSIBLE_REMOTE_TEMP=%ANSIBLE_REMOTE_TEMP% ANSIBLE_SCP_EXTRA_ARGS=%ANSIBLE_SCP_EXTRA_ARGS% ANSIBLE_HOST_KEY_CHECKING=%ANSIBLE_HOST_KEY_CHECKING% ansible %args%
```

`ansible-galaxy.bat`
```bat
@echo off
REM "alias" for packer ansible provisioner running in cmd/powershell
set args=%*
REM replace backslashes with slashes
call set args=%%args:\=/%%
REM fix drive letters
call set args=%%args:C:=/mnt/c%%
REM call set args=%%args:D:=/mnt/d%%
REM call set args=%%args:E:=/mnt/e%%

wsl -d AlmaLinuxOS-9-WSL1 ANSIBLE_PIPELINING=%ANSIBLE_PIPELINING% ANSIBLE_REMOTE_TEMP=%ANSIBLE_REMOTE_TEMP% ANSIBLE_SCP_EXTRA_ARGS=%ANSIBLE_SCP_EXTRA_ARGS% ANSIBLE_HOST_KEY_CHECKING=%ANSIBLE_HOST_KEY_CHECKING% ansible-galaxy %args%
```

`ansible-playbook.bat`
```bat
@echo off
REM "alias" for packer ansible provisioner running in cmd/powershell
set args=%*
REM replace backslashes with slashes
call set args=%%args:\=/%%
REM fix drive letters
call set args=%%args:C:=/mnt/c%%
REM call set args=%%args:D:=/mnt/d%%
REM call set args=%%args:E:=/mnt/e%%

wsl -d AlmaLinuxOS-9-WSL1 ANSIBLE_PIPELINING=%ANSIBLE_PIPELINING% ANSIBLE_REMOTE_TEMP=%ANSIBLE_REMOTE_TEMP% ANSIBLE_SCP_EXTRA_ARGS=%ANSIBLE_SCP_EXTRA_ARGS% ANSIBLE_HOST_KEY_CHECKING=%ANSIBLE_HOST_KEY_CHECKING% ansible-playbook %args%
```

For the rest of commands
```bat
@echo off
wsl -d AlmaLinuxOS-9-WSL1 $COMMAND
```

Create a directory for the wrappers.

```powershell
mkdir -p $Env:ProgramFiles\Ansible-WSL
```
Use the "Environment Variables" section to add this directory to the `$PATH`.

Install WSL1
```poweshell
wsl -l -v
wsl --shutdown AlmaLinuxOS-9
wsl --export AlmaLinuxOS-9 almalinux_wsl_backup.tar
wsl --import AlmaLinuxOS-9-WSL1 .\WSL1\AlmaLinuxOS-9\ .\almalinux_wsl_backuo.tar --version 1
```








## OpenSSH
### Client
### Server
### Service

Check status of `sshd` service
```powershell
Get-Service -Name sshd
```

Check if `sshd` service is enabled/Automatic
```powershell
(Get-Service 'sshd').StartupType
```

Note: You can get other member of `Get-Service` with:

```powershell
Get-Service | Get-Member | Format-List
```

Start the sshd service
```powershell
Start-Service sshd
```

Enable the `sshd` service
```powershell
Set-Service -Name sshd -StartupType 'Automatic'
```

Since there's no user associated with the sshd service, the host keys are stored under `$Env:ProgramData\ssh`

### Firewall

Get current connection profile Private or Guest or Public.

```powershell
Get-NetConnectionProfile
```

Sometimes, firewall rule may exist already. Let's check if it is.

```powershell
Get-NetFirewallRule -DisplayName '*ssh*'
```

If the firewall rule is already exist but profile is different with the current connection profile. Change the profile of the firewall rule.

To change from "Private" to "Public" profile:
```powershell
Set-NetFirewallRule -DisplayName 'OpenSSH SSH Server Preview (sshd)' -Profile Public
```

If there is no firewall rule present for OpenSSH Server, run this:

```powershell
if (!(Get-NetFirewallRule -Name "OpenSSH-Server-In-TCP" -ErrorAction SilentlyContinue | Select-Object Name, Enabled)) {
    Write-Output "Firewall Rule 'OpenSSH-Server-In-TCP' does not exist, creating it..."
    New-NetFirewallRule -Name 'OpenSSH-Server-In-TCP' -DisplayName 'OpenSSH Server (sshd)' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
} else {
    Write-Output "Firewall rule 'OpenSSH-Server-In-TCP' has been created and exists."
}
```

### Authentication (Public Key)

Disable password based authentication

Check if `passwordauthentication yes`:

```powershell
sshd -T | Select-String 'password'
```

```powershell
nvim $Env:ProgramData\ssh\sshd_config
```
Set this option:

`PasswordAuthentication no`

Restart the `sshd` service
```powershell
Restart-Service sshd
```

Add public key

For Users:

```powershell
$Env:SystemDrive\Users\$Env:USERNAME\.ssh\authorized_keys
```

For Administrator:
```powershell
$Env:ProgramData\ssh\administrators_authorized_keys
```

### Default shell

#### Powershell Core

Find the location of Powershell Core
```poweshell
Get-Variable PSHOME | Format-List
```


Powershell Core/OSS

Path: `C:\Program Files\PowerShell\7\pwsh.exe`

```powershell
New-ItemProperty -Path "HKLM:\SOFTWARE\OpenSSH" -Name DefaultShell -Value "C:\Program Files\PowerShell\7\pwsh.exe" -PropertyType String -Force
```
TODO: Coloring doesn't work well with linux dark terminals
- https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_ansi_terminals?view=powershell-7.4#disabling-ansi-output

## Environment Variables

```powershell
$Env:Path += ';C:\Tools'
[Environment]::SetEnvironmentVariable('Path', $Env:Path, 'User')
```

See:
https://learn.microsoft.com/en-us/dotnet/api/system.environment.setenvironmentvariable


Get all system environment variables

```powershell
Get-ChildItem Env:
```

- `HOMEPATH`
- `Path`
- `ProgramData`
- `ProgramFiles`
- `ProgramFiles(x86)`
- `SystemDrive`
- `SystemRoot`
- `USERNAME`
- `USERPROFILE`
- `windir`

Get all Powershell variables

```powershell
Get-Variable
```
- `HOME`
- `PID`
- `PROFILE`
- `PSHOME`
- `PWD`

## Packer

Create Hyper-V Virtual Switch.

```powershell
New-VMSwitch -Name 'VirtualMachines1' -SwitchType 'Internal'
```

Get Virtual Ethernet Interface of created virtual switch

```powershell
Get-NetAdapter | Format-List
```

Assign IP address to the Virtual Ethernet Interface.

```powershell
New-NetIPAddress -IPAddress 192.168.3.1 -PrefixLength 24 -InterfaceIndex 54
```

Create a NAT

```powershell
New-NetNat -Name 'VirtualMachines1' -InternalIPInterfaceAddressPrefix 192.168.3.0/24
```

Check
```powershell
Get-NetNat
```

Create IPv4 scope on DHCP server service.

```powershell
Add-DhcpServerv4Scope -Name 'VirtualMachines1' -StartRange 192.168.3.2 -EndRange 192.168.3.254 -SubnetMask 255.255.255.0
```

Set values for DHCP scope

```powershell
Set-DhcpServerv4OptionValue -ScopeId 192.168.3.0 -Router 192.168.3.1 -DnsServer 9.9.9.9, 149.112.112.112
```

Create a firewall rule for Packer

```powershell
New-NetFirewallRule -DisplayName 'HashiCorp Packer HTTP Server' -Direction Inbound -Action Allow -LocalPort 8000-9000 -Profile Public -Protocol TCP
```

## CICD

Packages:

- `Microsoft.PowerShell`
- `Microsoft.WindowsTerminal`
- `Microsoft.OpenSSH.Beta`
- `Git.Git`
- `cURL.cURL`
- `jqlang.jq`
- `Python.Python.3.12`
- `EclipseAdoptium.Temurin.21.JDK`
- `Hashicorp.Vagrant`
- `Hashicorp.Packer`
- `Hashicorp.Terraform`
- `Pulumi.Pulumi`
- `Neovim.Neovim`
- `Mozilla.Firefox`
- `TigerVNCproject.TigerVNC`
- `GnuPG.GnuPG` or `GnuPG.Gpg4win`
- `aria2.aria2`
- `GnuWin32.Tar`
- `Meta.Zstandard`
- `7zip.7zip`
- `Amazon.AWSCLI`
- `Docker.DockerDesktop` or `Docker.DockerCLI` and `Docker.DockerCompose`
- `RedHat.Podman` or `RedHat.Podman-Desktop`
- `MSYS2.MSYS2`
- `Microsoft.VisualStudio.2022.Community`

Windows Builder Image:

Base Images:
- AMI Name: `Windows_Server-2022-English-Full-Base-2024.05.15`
- AMI ID: `ami-0069eac59d05ae12b`

Builder Image:
`Windows_Server_2022_Agent_15052024`

`Windows_Server-2022-English-Full-Base-2024.05.15`

### Jenkins



```yaml
- amazonEC2:
    credentialsId: "alcib-user-prod-aws-creds"
    name: "windows_server_2022_x86_64_bm_aws"
    region: "us-east-1"
    sshKeysCredentialsId: "alcib-ssh-private-key-ec2-user"
    templates:
    - ami: "ami-0dfb4e52afb3ed992"
      amiType:
        unixData:
          bootDelay: "120"
          sshPort: "22"
      associatePublicIp: true
      connectBySSHProcess: false
      connectionStrategy: PUBLIC_DNS
      deleteRootOnTermination: false
      description: "Windows Server 2022 x86_64"
      ebsEncryptRootVolume: DEFAULT
      ebsOptimized: false
      hostKeyVerificationStrategy: 'OFF'
      iamInstanceProfile: "arn:aws:iam::764336703387:instance-profile/S3AlmaLinuxImages"
      idleTerminationMinutes: "60"
      javaPath: "java"
      labelString: "windows server 2022 bm x86_64 aws"
      maxTotalUses: -1
      metadataEndpointEnabled: true
      metadataHopsLimit: 1
      metadataSupported: true
      metadataTokensRequired: true
      minimumNumberOfInstances: 0
      minimumNumberOfSpareInstances: 0
      mode: EXCLUSIVE
      monitoring: false
      numExecutors: 16
      remoteAdmin: "Administrator"
      remoteFS: "C:\\Users\\Administrator\\jenkins"
      stopOnTerminate: false
      t2Unlimited: false
      tags:
      - name: "Name"
        value: "Jenkins Agent Windows Server 2022"
      tenancy: Default
      tmpDir: "C:\\Users\\Administrator\\AppData\\Local\\Temp"
      type: I3Metal
      useEphemeralDevices: false
    useInstanceProfileForCredentials: false
```

Create a Windows Instance

Only RSA type of SSH keys are supported to decrypt RDP password.

Convert private pair of SSH key from OpenSSH to PEM format.
```sh
ssh-keygen -p -N '' -f $key -m 'PEM'
```

```sh
aws --profile cloudsig ec2 run-instances \
  --block-device-mappings "DeviceName=/dev/sda1,Ebs={VolumeSize=200,VolumeType=gp3}" \
  --image-id $ami_id \
  --instance-type i3.metal \
  --key-name elkhan@almalinux.org \
  --security-groups Packer \
  --iam-instance-profile "Name=$iam_instance_profile" \
  --tag-specifications "ResourceType=instance,Tags=[{Key=Name,Value=Windows Jenkins Agent}]"
```


Useful links:
- https://github.com/PowerShell/Win32-OpenSSH/wiki/Security-protection-of-various-files-in-Win32-OpenSSH
- https://github.com/PowerShell/Win32-OpenSSH/wiki/DefaultShell
- https://github.com/PowerShell/Win32-OpenSSH/wiki/ssh.exe-examples


## WSL

Windows 11 is needed to build WSL distros.


Enable Developer Mode.

Run PowerShell with administrator privileges.
```powershell
reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\AppModelUnlock" /t REG_DWORD /f /v "AllowAllTrustedApps" /d "1"
```

Enable developer mode.


```powershell
PS C:\WINDOWS\system32> reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\AppModelUnlock" /t REG_DWORD /f /v "AllowDevelopmentWithoutDevLicense" /d "1"
```

### Install Visual Studio Community 2022


See component directory for Visual Studio Community 2022
- https://learn.microsoft.com/en-us/visualstudio/install/workload-component-id-vs-community?view=vs-2022&preserve-view=true

We need to install [Desktop development with C++](https://learn.microsoft.com/en-us/visualstudio/install/workload-component-id-vs-community?view=vs-2022&preserve-view=true#desktop-development-with-c) via `Microsoft.VisualStudio.Workload.NativeDesktop` component ID and with all recommended components.

Make sure to have latest Windows 11 SDK installed
- https://developer.microsoft.com/en-us/windows/downloads/windows-sdk/

For instance tha latest version of Windows 11 SDK is `10.0.26100` (`5/22/2024`). Add the patch version to the component ID like this `Microsoft.VisualStudio.Component.Windows11SDK.26100`

Install these optional components too
- `Microsoft.VisualStudio.ComponentGroup.UWP.Support`
- `Microsoft.VisualStudio.ComponentGroup.UWP.VC`
- `Microsoft.VisualStudio.Component.Windows11SDK.26100`


Generate self-signed certificate.

```powershell
New-SelfSignedCertificate -Type Custom -Subject "CN=Contoso Software, O=Contoso Corporation, C=US" -KeyUsage DigitalSignature -FriendlyName "Your friendly name goes here" -CertStoreLocation "Cert:\CurrentUser\My" -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.3", "2.5.29.19={text}")
```

```powershell
$params = @{
  TextExtension = @("2.5.29.37={text}1.3.6.1.5.5.7.3.3", "2.5.29.19={text}")
  KeyUsage = 'DigitalSignature'
  KeyLength = 4096
  KeyAlgorithm = 'RSA'
  Type = 'Custom'
  Subject = 'CN=AlmaLinux OS, O=AlmaLinux OS Foundation, C=US'
  FriendlyName = 'AlmaLinux OS WSL'
  HashAlgorithm = 'SHA256'
  CertStoreLocation = 'Cert:\CurrentUser\My'
}
New-SelfSignedCertificate @params
```

Expected output:

```powershell
PowerShell 7.4.3
PS C:\Users\PurpleManul> $params = @{
>>   TextExtension = @("2.5.29.37={text}1.3.6.1.5.5.7.3.3", "2.5.29.19={text}")
>>   KeyUsage = 'DigitalSignature'
>>   KeyLength = 4096
>>   KeyAlgorithm = 'RSA'
>>   Type = 'Custom'
>>   Subject = 'CN=AlmaLinux OS, O=AlmaLinux OS Foundation, C=US'
>>   FriendlyName = 'AlmaLinux OS WSL'
>>   HashAlgorithm = 'SHA256'
>>   CertStoreLocation = 'Cert:\CurrentUser\My'
>> }
PS C:\Users\PurpleManul> New-SelfSignedCertificate @params

   PSParentPath: Microsoft.PowerShell.Security\Certificate::CurrentUser\My

Thumbprint                                Subject              EnhancedKeyUsageList
----------                                -------              --------------------
B82DCD8E30C3C23DBDC3395894C395A32880B83D  CN=AlmaLinux OS, O=… Code Signing

PS C:\Users\PurpleManul>
```
Export certificate:

Source:
- https://learn.microsoft.com/en-us/powershell/module/pki/export-pfxcertificate?view=windowsserver2022-ps

