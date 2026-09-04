# 5.3 IDE JetBrains

IntelliJ IDEA, GoLand, PyCharm et les autres IDE JetBrains prennent en charge Terraform via le plugin **Terraform and HCL**, qui peut utiliser `terraform-ls` pour une complétion plus riche.

## Après l'installation

- Activez le plugin dans Settings, Plugins, puis pointez-le vers le binaire `terraform` dans Settings, Tools, Terraform.
- La complétion, la vue structure et le refactoring de renommage fonctionnent à travers un module.
- Définissez `terraform fmt` comme File Watcher ou activez « Reformat on save » pour que le style reste cohérent.
- Des configurations d'exécution peuvent encapsuler `terraform plan` et `terraform apply` si vous les voulez dans le panneau Run.

## Pratique

Créez une variable, référencez-la dans une ressource, puis utilisez « Find usages » et « Rename » pour confirmer que l'IDE suit la référence à travers les fichiers.

## Références

- [Terraform and HCL plugin (JetBrains Marketplace)](https://plugins.jetbrains.com/plugin/7808-terraform-and-hcl)
- [Terraform support in IntelliJ IDEA (help)](https://www.jetbrains.com/help/idea/terraform.html)
- [terraform-ls](https://github.com/hashicorp/terraform-ls)
