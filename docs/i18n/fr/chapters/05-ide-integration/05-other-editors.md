# 5.5 Autres éditeurs

Tout éditeur prenant en charge le Language Server Protocol peut utiliser `terraform-ls`.

- **Sublime Text :** le package LSP plus le package d'aide `LSP-terraform`, installés via Package Control.
- **Emacs :** `lsp-mode` (rattaché à `terraform-mode`) ou `eglot` pointé vers `terraform-ls`, avec `terraform-mode` pour la syntaxe et l'indentation.
- **Zed :** non intégré par défaut — installez l'[extension Terraform](https://github.com/zed-extensions/terraform) communautaire depuis le panneau d'extensions de Zed ; elle exécute ensuite `terraform-ls` pour vous.
- **Helix :** embarque une définition de langage `hcl` (qui couvre aussi les fichiers `.tf`) préconfigurée pour utiliser `terraform-ls` ; installez le binaire sur le `PATH` et il se rattache sans configuration supplémentaire.

Pour les éditeurs sans serveur de langage, vous tirez tout de même parti de `terraform fmt` comme hook d'enregistrement et de `terraform validate` comme commande de build.

## Pratique

Choisissez votre éditeur, installez `terraform-ls` sur le `PATH`, et vérifiez que la complétion fonctionne dans un bloc de ressource. Si ce n'est pas le cas, vérifiez que le dossier a été initialisé avec `terraform init`.

## Références

- [terraform-ls (Terraform language server)](https://github.com/hashicorp/terraform-ls)
- [terraform-ls usage guide](https://github.com/hashicorp/terraform-ls/blob/main/docs/USAGE.md)
- [Helix language support](https://docs.helix-editor.com/lang-support.html)
- [Zed: Terraform language docs](https://zed.dev/docs/languages/terraform)
- [zed-extensions/terraform](https://github.com/zed-extensions/terraform)
