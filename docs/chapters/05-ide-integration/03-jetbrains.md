# 5.3 JetBrains IDEs

IntelliJ IDEA, GoLand, PyCharm, and the other JetBrains IDEs support Terraform (and OpenTofu) through the **Terraform and HCL** plugin, published by JetBrains, which uses `terraform-ls` under the hood for provider-aware completion and diagnostics.

## After installing

- Enable the plugin in Settings, Plugins, then point it at the `terraform` binary in Settings, Tools, Terraform Tools. The IDE usually detects an installed binary automatically; use "Detect and Test" if it does not.
- Completion, structure view, Find Usages (Ctrl+B), and Rename (Shift+F6) work across a module, including third-party providers from the Terraform Registry.
- Formatting has two independent controls: which formatter runs (`terraform fmt` itself, or the IDE's own HCL formatter) is chosen in Settings, Editor, Code Style, Terraform; whether it runs automatically on save is a separate toggle in Settings, Tools, Actions on Save ("Reformat code").
- Run configurations wrap `terraform init`, `validate`, `plan`, `apply`, and `destroy` (or a custom command) so they appear in the Run panel with their own working directory and environment variables, instead of a raw terminal invocation.

## Trade-offs versus a lightweight editor

The JetBrains plugin trades a heavier IDE for deeper static analysis: cross-file rename, "introduce variable" refactoring, and inspections that flag deprecated or missing arguments before you even run `validate`. If your team already lives in an IntelliJ-family IDE for other languages, this is close to free; if not, a lighter editor with `terraform-ls` (Chapters 5.2, 5.4, 5.5) gets you most of the day-to-day value without the extra install.

## Practice

Create a variable, reference it in a resource, then use "Find usages" and "Rename" to confirm the IDE tracks the reference across files.

## References

- [Terraform and HCL plugin (JetBrains Marketplace)](https://plugins.jetbrains.com/plugin/7808-terraform-and-hcl)
- [Terraform support in IntelliJ IDEA (help)](https://www.jetbrains.com/help/idea/terraform.html)
- [Reformat and rearrange code / Actions on Save (help)](https://www.jetbrains.com/help/idea/reformat-and-rearrange-code.html)
- [terraform-ls](https://github.com/hashicorp/terraform-ls)
