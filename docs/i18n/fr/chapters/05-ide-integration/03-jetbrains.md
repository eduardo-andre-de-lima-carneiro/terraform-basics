# 5.3 IDE JetBrains

IntelliJ IDEA, GoLand, PyCharm et les autres IDE JetBrains prennent en charge Terraform (et OpenTofu) via le plugin **Terraform and HCL**, publié par JetBrains, qui utilise `terraform-ls` en interne pour une complétion et des diagnostics tenant compte des providers.

## Après l'installation

- Activez le plugin dans Settings, Plugins, puis pointez-le vers le binaire `terraform` dans Settings, Tools, Terraform Tools. L'IDE détecte généralement un binaire installé automatiquement ; utilisez « Detect and Test » si ce n'est pas le cas.
- La complétion, la vue structure, Find Usages (Ctrl+B) et Rename (Shift+F6) fonctionnent à travers un module, y compris pour les providers tiers du Terraform Registry.
- Le formatage a deux réglages indépendants : quel formateur s'exécute (`terraform fmt` lui-même, ou le formateur HCL propre à l'IDE) se choisit dans Settings, Editor, Code Style, Terraform ; le fait qu'il s'exécute automatiquement à l'enregistrement est un interrupteur séparé dans Settings, Tools, Actions on Save (« Reformat code »).
- Les configurations d'exécution encapsulent `terraform init`, `validate`, `plan`, `apply` et `destroy` (ou une commande personnalisée) pour qu'elles apparaissent dans le panneau Run avec leur propre répertoire de travail et leurs variables d'environnement, plutôt qu'un simple appel de terminal.

## Compromis face à un éditeur léger

Le plugin JetBrains échange un IDE plus lourd contre une analyse statique plus poussée : renommage inter-fichiers, refactoring « introduce variable » et inspections qui signalent les arguments obsolètes ou manquants avant même d'exécuter `validate`. Si votre équipe vit déjà dans un IDE de la famille IntelliJ pour d'autres langages, c'est quasiment gratuit ; sinon, un éditeur plus léger avec `terraform-ls` (chapitres 5.2, 5.4, 5.5) apporte l'essentiel de la valeur au quotidien sans installation supplémentaire.

## Pratique

Créez une variable, référencez-la dans une ressource, puis utilisez « Find usages » et « Rename » pour confirmer que l'IDE suit la référence à travers les fichiers.

## Références

- [Terraform and HCL plugin (JetBrains Marketplace)](https://plugins.jetbrains.com/plugin/7808-terraform-and-hcl)
- [Terraform support in IntelliJ IDEA (help)](https://www.jetbrains.com/help/idea/terraform.html)
- [Reformat and rearrange code / Actions on Save (help)](https://www.jetbrains.com/help/idea/reformat-and-rearrange-code.html)
- [terraform-ls](https://github.com/hashicorp/terraform-ls)
