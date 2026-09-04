# 2.1 Install Terraform

Terraform is a single binary. Install it with your platform's package manager, or download the binary directly from the official releases.

## macOS (Homebrew)

```bash
brew tap hashicorp/tap
brew install hashicorp/tap/terraform
```

## Debian and Ubuntu (official apt repository)

HashiCorp publishes an apt repository that works for both Debian and Ubuntu:

```bash
wget -O - https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(grep -oP '(?<=UBUNTU_CODENAME=).*' /etc/os-release || lsb_release -cs) main" | \
  sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update
sudo apt install terraform
```

The first command downloads HashiCorp's GPG key and stores it so apt can verify the package signature; the second adds the repository itself, picking your distribution's codename automatically so the same three commands work on Debian and Ubuntu. Upgrade later with:

```bash
sudo apt update && sudo apt install --only-upgrade terraform
```

## Windows

Terraform does not ship an official Windows installer, but two community-maintained package managers track official releases:

```powershell
# winget (built into Windows 11 and current Windows 10)
winget install --exact --id Hashicorp.Terraform

# Chocolatey
choco install terraform
```

Either command installs the binary and puts it on your `PATH`. To upgrade, run `winget upgrade Hashicorp.Terraform` or `choco upgrade terraform`. You can also skip package managers entirely: download the `windows_amd64.zip` from the official releases, unzip it, and place `terraform.exe` somewhere on your `PATH`.

## Any other platform: manual binary

Every release also ships as a plain zip archive:

```bash
curl -O https://releases.hashicorp.com/terraform/<version>/terraform_<version>_linux_amd64.zip
unzip terraform_<version>_linux_amd64.zip
sudo mv terraform /usr/local/bin/
```

Replace `<version>` and the OS/architecture suffix with the values for your platform, listed on the releases page.

## Verify the installation

```bash
terraform version
```

The command prints the CLI version and, once a working directory is initialized, the provider versions in use. If the version is old, upgrade before continuing so the examples behave as described.

## Practice

Install Terraform with the method for your platform (apt, winget, Chocolatey, Homebrew, or the manual binary), run `terraform version`, and record the version string. Run `terraform -help` and skim the list of subcommands you will meet in Chapter 3.

## References

- [Install Terraform](https://developer.hashicorp.com/terraform/install)
- [Official packaging repository (releases.hashicorp.com)](https://releases.hashicorp.com/terraform/)
- [HashiCorp apt/yum GPG key](https://apt.releases.hashicorp.com/gpg)
- [Windows Package Manager (winget) documentation](https://learn.microsoft.com/en-us/windows/package-manager/winget/)
- [Hashicorp.Terraform winget manifest (community-maintained)](https://github.com/microsoft/winget-pkgs/tree/master/manifests/h/Hashicorp/Terraform)
- [terraform package on Chocolatey (community-maintained)](https://community.chocolatey.org/packages/terraform)
- [Command: version](https://developer.hashicorp.com/terraform/cli/commands/version)
- [Basic CLI features](https://developer.hashicorp.com/terraform/cli/commands)
