# 2.1 Instalar o Terraform

O Terraform é um único binário. Instale-o com o gerenciador de pacotes da sua plataforma ou baixe-o dos releases oficiais.

## Opções comuns

```bash
# macOS (Homebrew)
brew tap hashicorp/tap
brew install hashicorp/tap/terraform

# Debian and Ubuntu use the official HashiCorp apt repository
# Windows uses winget or Chocolatey
```

## Verifique a instalação

```bash
terraform version
```

O comando imprime a versão da CLI e, quando um diretório de trabalho é inicializado, as versões de provider em uso. Se a versão estiver antiga, atualize antes de continuar para que os exemplos se comportem como descrito.

## Prática

Instale o Terraform, execute `terraform version` e registre a string de versão. Execute `terraform -help` e passe os olhos pela lista de subcomandos que você vai encontrar no Capítulo 3.

## Referências

- [Install Terraform](https://developer.hashicorp.com/terraform/install)
- [Official packaging repository (releases.hashicorp.com)](https://releases.hashicorp.com/terraform/)
- [Command: version](https://developer.hashicorp.com/terraform/cli/commands/version)
- [Basic CLI features](https://developer.hashicorp.com/terraform/cli/commands)
