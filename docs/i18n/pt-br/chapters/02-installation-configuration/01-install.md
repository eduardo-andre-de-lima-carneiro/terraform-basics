# 2.1 Instalar o Terraform

O Terraform é um binário único. Instale-o com o gerenciador de pacotes da sua plataforma, ou baixe o binário diretamente das releases oficiais.

## macOS (Homebrew)

```bash
brew tap hashicorp/tap
brew install hashicorp/tap/terraform
```

## Debian e Ubuntu (repositório apt oficial)

A HashiCorp publica um repositório apt que funciona tanto para Debian quanto para Ubuntu:

```bash
wget -O - https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(grep -oP '(?<=UBUNTU_CODENAME=).*' /etc/os-release || lsb_release -cs) main" | \
  sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update
sudo apt install terraform
```

O primeiro comando baixa a chave GPG da HashiCorp e a guarda para que o apt possa verificar a assinatura do pacote; o segundo adiciona o repositório em si, detectando automaticamente o codinome da sua distribuição, então os mesmos três comandos funcionam tanto no Debian quanto no Ubuntu. Para atualizar depois:

```bash
sudo apt update && sudo apt install --only-upgrade terraform
```

## Windows

O Terraform não tem um instalador oficial para Windows, mas dois gerenciadores de pacotes mantidos pela comunidade acompanham as releases oficiais:

```powershell
# winget (embutido no Windows 11 e no Windows 10 atual)
winget install --exact --id Hashicorp.Terraform

# Chocolatey
choco install terraform
```

Qualquer um dos comandos instala o binário e o coloca no seu `PATH`. Para atualizar, rode `winget upgrade Hashicorp.Terraform` ou `choco upgrade terraform`. Você também pode dispensar os gerenciadores de pacotes: baixe o `windows_amd64.zip` das releases oficiais, descompacte-o e coloque `terraform.exe` em algum lugar do seu `PATH`.

## Qualquer outra plataforma: binário manual

Cada release também é distribuída como um arquivo zip simples:

```bash
curl -O https://releases.hashicorp.com/terraform/<version>/terraform_<version>_linux_amd64.zip
unzip terraform_<version>_linux_amd64.zip
sudo mv terraform /usr/local/bin/
```

Substitua `<version>` e o sufixo de sistema operacional/arquitetura pelos valores da sua plataforma, listados na página de releases.

## Verifique a instalação

```bash
terraform version
```

O comando imprime a versão da CLI e, depois que um diretório de trabalho é inicializado, as versões dos providers em uso. Se a versão estiver desatualizada, atualize antes de continuar para que os exemplos se comportem como descrito.

## Prática

Instale o Terraform com o método para a sua plataforma (apt, winget, Chocolatey, Homebrew ou o binário manual), rode `terraform version` e anote a string de versão. Rode `terraform -help` e passe os olhos pela lista de subcomandos que você vai encontrar no Capítulo 3.

## Referências

- [Install Terraform](https://developer.hashicorp.com/terraform/install)
- [Official packaging repository (releases.hashicorp.com)](https://releases.hashicorp.com/terraform/)
- [HashiCorp apt/yum GPG key](https://apt.releases.hashicorp.com/gpg)
- [Windows Package Manager (winget) documentation](https://learn.microsoft.com/en-us/windows/package-manager/winget/)
- [Hashicorp.Terraform winget manifest (community-maintained)](https://github.com/microsoft/winget-pkgs/tree/master/manifests/h/Hashicorp/Terraform)
- [terraform package on Chocolatey (community-maintained)](https://community.chocolatey.org/packages/terraform)
- [Command: version](https://developer.hashicorp.com/terraform/cli/commands/version)
- [Basic CLI features](https://developer.hashicorp.com/terraform/cli/commands)
