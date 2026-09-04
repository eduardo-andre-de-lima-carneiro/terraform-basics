# 5.3 IDE de JetBrains

IntelliJ IDEA, GoLand, PyCharm y los demás IDE de JetBrains admiten Terraform (y OpenTofu) a través del plugin **Terraform and HCL**, publicado por JetBrains, que usa `terraform-ls` internamente para el autocompletado y los diagnósticos conscientes de los providers.

## Después de instalar

- Habilita el plugin en Settings, Plugins, y luego apúntalo al binario `terraform` en Settings, Tools, Terraform Tools. El IDE suele detectar un binario instalado automáticamente; usa "Detect and Test" si no lo hace.
- El autocompletado, la vista de estructura, Find Usages (Ctrl+B) y Rename (Shift+F6) funcionan en todo un módulo, incluidos providers de terceros del Terraform Registry.
- El formato tiene dos controles independientes: qué formateador se ejecuta (`terraform fmt` en sí, o el formateador HCL propio del IDE) se elige en Settings, Editor, Code Style, Terraform; si se ejecuta automáticamente al guardar es un interruptor aparte en Settings, Tools, Actions on Save ("Reformat code").
- Las run configurations envuelven `terraform init`, `validate`, `plan`, `apply` y `destroy` (o un comando personalizado) para que aparezcan en el panel Run con su propio directorio de trabajo y variables de entorno, en lugar de una invocación de terminal sin más.

## Compensaciones frente a un editor ligero

El plugin de JetBrains cambia un IDE más pesado por un análisis estático más profundo: renombrado entre archivos, refactor "introduce variable" e inspecciones que señalan argumentos obsoletos o faltantes incluso antes de ejecutar `validate`. Si tu equipo ya vive en un IDE de la familia IntelliJ para otros lenguajes, esto es casi gratis; si no, un editor más ligero con `terraform-ls` (capítulos 5.2, 5.4, 5.5) te da la mayor parte del valor del día a día sin la instalación adicional.

## Práctica

Crea una variable, referénciala en un recurso y luego usa "Find usages" y "Rename" para confirmar que el IDE sigue la referencia entre archivos.

## Referencias

- [Terraform and HCL plugin (JetBrains Marketplace)](https://plugins.jetbrains.com/plugin/7808-terraform-and-hcl)
- [Terraform support in IntelliJ IDEA (help)](https://www.jetbrains.com/help/idea/terraform.html)
- [Reformat and rearrange code / Actions on Save (help)](https://www.jetbrains.com/help/idea/reformat-and-rearrange-code.html)
- [terraform-ls](https://github.com/hashicorp/terraform-ls)
