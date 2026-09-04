# 5.3 IDE de JetBrains

IntelliJ IDEA, GoLand, PyCharm y los demás IDE de JetBrains admiten Terraform a través del plugin **Terraform and HCL**, que puede usar `terraform-ls` para un autocompletado más rico.

## Después de instalar

- Habilita el plugin en Settings, Plugins, y luego apúntalo al binario `terraform` en Settings, Tools, Terraform.
- El autocompletado, la vista de estructura y el refactor de renombrado funcionan en todo un módulo.
- Configura `terraform fmt` como File Watcher o habilita "Reformat on save" para que el estilo se mantenga consistente.
- Las run configurations pueden envolver `terraform plan` y `terraform apply` si los quieres en el panel Run.

## Práctica

Crea una variable, referénciala en un recurso y luego usa "Find usages" y "Rename" para confirmar que el IDE sigue la referencia entre archivos.

## Referencias

- [Terraform and HCL plugin (JetBrains Marketplace)](https://plugins.jetbrains.com/plugin/7808-terraform-and-hcl)
- [Terraform support in IntelliJ IDEA (help)](https://www.jetbrains.com/help/idea/terraform.html)
- [terraform-ls](https://github.com/hashicorp/terraform-ls)
