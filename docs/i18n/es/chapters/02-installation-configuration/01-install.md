# 2.1 Instalar Terraform

Terraform es un único binario. Instálalo con el gestor de paquetes de tu plataforma o descárgalo desde las releases oficiales.

## Opciones habituales

```bash
# macOS (Homebrew)
brew tap hashicorp/tap
brew install hashicorp/tap/terraform

# Debian and Ubuntu use the official HashiCorp apt repository
# Windows uses winget or Chocolatey
```

## Verifica la instalación

```bash
terraform version
```

El comando imprime la versión de la CLI y, una vez inicializado un directorio de trabajo, las versiones de los providers en uso. Si la versión es antigua, actualiza antes de continuar para que los ejemplos se comporten como se describe.

## Práctica

Instala Terraform, ejecuta `terraform version` y anota la cadena de versión. Ejecuta `terraform -help` y echa un vistazo a la lista de subcomandos que encontrarás en el Capítulo 3.

## Referencias

- [Install Terraform](https://developer.hashicorp.com/terraform/install)
- [Official packaging repository (releases.hashicorp.com)](https://releases.hashicorp.com/terraform/)
- [Command: version](https://developer.hashicorp.com/terraform/cli/commands/version)
- [Basic CLI features](https://developer.hashicorp.com/terraform/cli/commands)
