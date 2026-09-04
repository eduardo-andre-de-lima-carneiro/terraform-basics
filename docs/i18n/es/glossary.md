# Glosario

- **Configuración:** Los archivos `.tf` que describen la infraestructura deseada.
- **Provider:** Un plugin que permite a Terraform gestionar una plataforma o API concreta.
- **Recurso:** Un único objeto de infraestructura gestionado por una configuración de Terraform.
- **Data source:** Una consulta de solo lectura de información definida fuera de la configuración.
- **State:** El archivo donde Terraform registra los recursos que gestiona y sus últimos valores conocidos.
- **Plan:** Una vista previa de las acciones que Terraform tomaría para alcanzar el estado deseado.
- **Archivo de plan guardado:** La salida de `terraform plan -out`, que se aplica más tarde para que `apply` ejecute exactamente lo que se revisó.
- **Apply:** Ejecutar un plan para crear, actualizar o destruir recursos.
- **Módulo:** Un grupo reutilizable de archivos de configuración que se invoca con variables de entrada.
- **Backend:** El lugar donde Terraform almacena el state, como el disco local o un servicio remoto.
- **Workspace:** Una instancia de state con nombre que permite que una configuración gestione varios entornos.
- **HCP Terraform:** La plataforma gestionada de HashiCorp para ejecuciones remotas, state y políticas (antes Terraform Cloud).
- **Archivo de bloqueo de dependencias:** `.terraform.lock.hcl`, que fija las versiones de los providers y sus checksums; versiónalo en el control de versiones.
- **Drift (desviación):** Una diferencia entre la infraestructura real y el state registrado.
- **Variable:** Una entrada con nombre que parametriza una configuración o un módulo.
- **Output:** Un valor con nombre que una configuración o un módulo expone después del apply.
- **Bloque `moved`:** Configuración que le indica a Terraform que la dirección de un recurso cambió, para que no se destruya y se vuelva a crear.

## Referencias

- [Terraform glossary (HashiCorp)](https://developer.hashicorp.com/terraform/docs/glossary)
- [Terraform language documentation](https://developer.hashicorp.com/terraform/language)
- [Terraform CLI documentation](https://developer.hashicorp.com/terraform/cli)
