# 5.3 IDEs da JetBrains

O IntelliJ IDEA, o GoLand, o PyCharm e as outras IDEs da JetBrains têm suporte a Terraform (e OpenTofu) pelo plugin **Terraform and HCL**, publicado pela JetBrains, que usa o `terraform-ls` internamente para autocompletar e diagnósticos que levam em conta os providers.

## Depois de instalar

- Habilite o plugin em Settings, Plugins, depois aponte-o para o binário `terraform` em Settings, Tools, Terraform Tools. A IDE costuma detectar um binário instalado automaticamente; use "Detect and Test" se isso não acontecer.
- Autocompletar, structure view, Find Usages (Ctrl+B) e Rename (Shift+F6) funcionam por todo um módulo, incluindo providers de terceiros do Terraform Registry.
- A formatação tem dois controles independentes: qual formatador roda (o próprio `terraform fmt`, ou o formatador HCL nativo da IDE) é escolhido em Settings, Editor, Code Style, Terraform; se ele roda automaticamente ao salvar é uma opção separada em Settings, Tools, Actions on Save ("Reformat code").
- As run configurations envolvem `terraform init`, `validate`, `plan`, `apply` e `destroy` (ou um comando personalizado), de modo que aparecem no painel Run com seu próprio diretório de trabalho e variáveis de ambiente, em vez de uma chamada de terminal crua.

## Vantagens e desvantagens frente a um editor leve

O plugin da JetBrains troca uma IDE mais pesada por uma análise estática mais profunda: rename entre arquivos, refatoração "introduce variable" e inspeções que sinalizam argumentos obsoletos ou ausentes antes mesmo de rodar `validate`. Se sua equipe já vive em uma IDE da família IntelliJ para outras linguagens, isso é quase de graça; caso contrário, um editor mais leve com `terraform-ls` (capítulos 5.2, 5.4, 5.5) entrega a maior parte do valor do dia a dia sem a instalação extra.

## Prática

Crie uma variável, referencie-a em um resource, depois use "Find usages" e "Rename" para confirmar que a IDE acompanha a referência entre arquivos.

## Referências

- [Terraform and HCL plugin (JetBrains Marketplace)](https://plugins.jetbrains.com/plugin/7808-terraform-and-hcl)
- [Terraform support in IntelliJ IDEA (help)](https://www.jetbrains.com/help/idea/terraform.html)
- [Reformat and rearrange code / Actions on Save (help)](https://www.jetbrains.com/help/idea/reformat-and-rearrange-code.html)
- [terraform-ls](https://github.com/hashicorp/terraform-ls)
