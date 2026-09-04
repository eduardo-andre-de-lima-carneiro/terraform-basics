# 5.3 IDEs da JetBrains

O IntelliJ IDEA, o GoLand, o PyCharm e as outras IDEs da JetBrains têm suporte a Terraform pelo plugin **Terraform and HCL**, que pode usar o `terraform-ls` para um autocompletar mais rico.

## Depois de instalar

- Habilite o plugin em Settings, Plugins, depois aponte-o para o binário `terraform` em Settings, Tools, Terraform.
- Autocompletar, structure view e refatoração de rename funcionam por todo um módulo.
- Defina o `terraform fmt` como um File Watcher ou habilite "Reformat on save" para que o estilo permaneça consistente.
- As run configurations podem envolver `terraform plan` e `terraform apply` se você quiser tê-los no painel Run.

## Prática

Crie uma variável, referencie-a em um resource, depois use "Find usages" e "Rename" para confirmar que a IDE acompanha a referência entre arquivos.

## Referências

- [Terraform and HCL plugin (JetBrains Marketplace)](https://plugins.jetbrains.com/plugin/7808-terraform-and-hcl)
- [Terraform support in IntelliJ IDEA (help)](https://www.jetbrains.com/help/idea/terraform.html)
- [terraform-ls](https://github.com/hashicorp/terraform-ls)
