# 5.5 Autres éditeurs

Tout éditeur prenant en charge le Language Server Protocol peut utiliser `terraform-ls`.

- **Sublime Text :** le package LSP plus une configuration d'aide `LSP-terraform`.
- **Emacs :** `lsp-mode` ou `eglot` pointé vers `terraform-ls`, avec `terraform-mode` pour la syntaxe.
- **Zed :** intègre une prise en charge du langage Terraform qui utilise `terraform-ls` automatiquement.
- **Helix :** configuration intégrée pour `terraform-ls` ; installez le binaire et il se rattache.

Pour les éditeurs sans serveur de langage, vous tirez tout de même parti de `terraform fmt` comme hook d'enregistrement et de `terraform validate` comme commande de build.

## Pratique

Choisissez votre éditeur, installez `terraform-ls` sur le `PATH`, et vérifiez que la complétion fonctionne dans un bloc de ressource. Si ce n'est pas le cas, vérifiez que le dossier a été initialisé avec `terraform init`.

## Références

- [terraform-ls (Terraform language server)](https://github.com/hashicorp/terraform-ls)
- [terraform-ls usage guide](https://github.com/hashicorp/terraform-ls/blob/main/docs/USAGE.md)
- [Helix language support](https://docs.helix-editor.com/lang-support.html)
- [Zed languages](https://zed.dev/docs/languages)
