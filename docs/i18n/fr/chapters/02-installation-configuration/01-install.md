# 2.1 Installer Terraform

Terraform est un binaire unique. Installez-le avec le gestionnaire de paquets de votre plateforme, ou téléchargez le binaire directement depuis les versions officielles.

## macOS (Homebrew)

```bash
brew tap hashicorp/tap
brew install hashicorp/tap/terraform
```

## Debian et Ubuntu (dépôt apt officiel)

HashiCorp publie un dépôt apt qui fonctionne à la fois pour Debian et pour Ubuntu :

```bash
wget -O - https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(grep -oP '(?<=UBUNTU_CODENAME=).*' /etc/os-release || lsb_release -cs) main" | \
  sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update
sudo apt install terraform
```

La première commande télécharge la clé GPG de HashiCorp et la stocke pour qu'apt puisse vérifier la signature du paquet ; la seconde ajoute le dépôt lui-même, en détectant automatiquement le nom de code de votre distribution, si bien que les trois mêmes commandes fonctionnent sur Debian comme sur Ubuntu. Mettez à jour plus tard avec :

```bash
sudo apt update && sudo apt install --only-upgrade terraform
```

## Windows

Terraform ne fournit pas d'installateur Windows officiel, mais deux gestionnaires de paquets maintenus par la communauté suivent les versions officielles :

```powershell
# winget (intégré à Windows 11 et au Windows 10 actuel)
winget install --exact --id Hashicorp.Terraform

# Chocolatey
choco install terraform
```

L'une ou l'autre commande installe le binaire et l'ajoute à votre `PATH`. Pour mettre à jour, exécutez `winget upgrade Hashicorp.Terraform` ou `choco upgrade terraform`. Vous pouvez aussi vous passer des gestionnaires de paquets : téléchargez le `windows_amd64.zip` depuis les versions officielles, décompressez-le, et placez `terraform.exe` quelque part dans votre `PATH`.

## Toute autre plateforme : binaire manuel

Chaque version est aussi distribuée sous forme d'archive zip simple :

```bash
curl -O https://releases.hashicorp.com/terraform/<version>/terraform_<version>_linux_amd64.zip
unzip terraform_<version>_linux_amd64.zip
sudo mv terraform /usr/local/bin/
```

Remplacez `<version>` et le suffixe de système d'exploitation/architecture par les valeurs de votre plateforme, listées sur la page des versions.

## Vérifiez l'installation

```bash
terraform version
```

La commande affiche la version de la CLI et, une fois qu'un répertoire de travail est initialisé, les versions des providers utilisées. Si la version est ancienne, mettez à jour avant de continuer afin que les exemples se comportent comme décrit.

## Pratique

Installez Terraform avec la méthode adaptée à votre plateforme (apt, winget, Chocolatey, Homebrew ou le binaire manuel), exécutez `terraform version` et notez la chaîne de version. Exécutez `terraform -help` et parcourez la liste des sous-commandes que vous rencontrerez au Chapitre 3.

## Références

- [Install Terraform](https://developer.hashicorp.com/terraform/install)
- [Official packaging repository (releases.hashicorp.com)](https://releases.hashicorp.com/terraform/)
- [HashiCorp apt/yum GPG key](https://apt.releases.hashicorp.com/gpg)
- [Windows Package Manager (winget) documentation](https://learn.microsoft.com/en-us/windows/package-manager/winget/)
- [Hashicorp.Terraform winget manifest (community-maintained)](https://github.com/microsoft/winget-pkgs/tree/master/manifests/h/Hashicorp/Terraform)
- [terraform package on Chocolatey (community-maintained)](https://community.chocolatey.org/packages/terraform)
- [Command: version](https://developer.hashicorp.com/terraform/cli/commands/version)
- [Basic CLI features](https://developer.hashicorp.com/terraform/cli/commands)
