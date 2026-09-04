# 2.1 Instalar Terraform

Terraform es un único binario. Instálalo con el gestor de paquetes de tu plataforma, o descarga el binario directamente desde las versiones oficiales.

## macOS (Homebrew)

```bash
brew tap hashicorp/tap
brew install hashicorp/tap/terraform
```

## Debian y Ubuntu (repositorio apt oficial)

HashiCorp publica un repositorio apt que funciona tanto para Debian como para Ubuntu:

```bash
wget -O - https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(grep -oP '(?<=UBUNTU_CODENAME=).*' /etc/os-release || lsb_release -cs) main" | \
  sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update
sudo apt install terraform
```

El primer comando descarga la clave GPG de HashiCorp y la guarda para que apt pueda verificar la firma del paquete; el segundo agrega el repositorio en sí, detectando automáticamente el nombre en clave de tu distribución, así que los mismos tres comandos funcionan tanto en Debian como en Ubuntu. Para actualizar más adelante:

```bash
sudo apt update && sudo apt install --only-upgrade terraform
```

## Windows

Terraform no incluye un instalador oficial para Windows, pero dos gestores de paquetes mantenidos por la comunidad siguen las versiones oficiales:

```powershell
# winget (integrado en Windows 11 y en el Windows 10 actual)
winget install --exact --id Hashicorp.Terraform

# Chocolatey
choco install terraform
```

Cualquiera de los dos comandos instala el binario y lo agrega a tu `PATH`. Para actualizar, ejecuta `winget upgrade Hashicorp.Terraform` o `choco upgrade terraform`. También puedes prescindir de los gestores de paquetes: descarga el `windows_amd64.zip` de las versiones oficiales, descomprímelo y coloca `terraform.exe` en algún lugar de tu `PATH`.

## Cualquier otra plataforma: binario manual

Cada versión también se distribuye como un archivo zip simple:

```bash
curl -O https://releases.hashicorp.com/terraform/<version>/terraform_<version>_linux_amd64.zip
unzip terraform_<version>_linux_amd64.zip
sudo mv terraform /usr/local/bin/
```

Reemplaza `<version>` y el sufijo de sistema operativo/arquitectura con los valores de tu plataforma, listados en la página de versiones.

## Verifica la instalación

```bash
terraform version
```

El comando imprime la versión de la CLI y, una vez que un directorio de trabajo está inicializado, las versiones de los providers en uso. Si la versión es antigua, actualiza antes de continuar para que los ejemplos se comporten como se describe.

## Práctica

Instala Terraform con el método para tu plataforma (apt, winget, Chocolatey, Homebrew o el binario manual), ejecuta `terraform version` y anota la cadena de versión. Ejecuta `terraform -help` y revisa la lista de subcomandos que verás en el Capítulo 3.

## Referencias

- [Install Terraform](https://developer.hashicorp.com/terraform/install)
- [Official packaging repository (releases.hashicorp.com)](https://releases.hashicorp.com/terraform/)
- [HashiCorp apt/yum GPG key](https://apt.releases.hashicorp.com/gpg)
- [Windows Package Manager (winget) documentation](https://learn.microsoft.com/en-us/windows/package-manager/winget/)
- [Hashicorp.Terraform winget manifest (community-maintained)](https://github.com/microsoft/winget-pkgs/tree/master/manifests/h/Hashicorp/Terraform)
- [terraform package on Chocolatey (community-maintained)](https://community.chocolatey.org/packages/terraform)
- [Command: version](https://developer.hashicorp.com/terraform/cli/commands/version)
- [Basic CLI features](https://developer.hashicorp.com/terraform/cli/commands)
