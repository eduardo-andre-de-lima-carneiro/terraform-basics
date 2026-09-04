# 4.1 Fundamentos de la integración

Ejecutar Terraform en una plataforma añade servicios de colaboración y entrega alrededor de una configuración. Los comandos siguen siendo los de siempre; la plataforma aporta identidad, permisos, revisión de planes, automatización y visibilidad de los cambios.

## El flujo común

1. Almacena la configuración en un repositorio de control de versiones.
2. Configura un backend remoto o un workspace de la plataforma para el state.
3. En un pull request, ejecuta `terraform plan` automáticamente y publica el resultado.
4. Exige la revisión tanto del diff de código como del plan.
5. Ejecuta `terraform apply` solo después del merge, tras una aprobación o un entorno protegido.
6. Mantén las credenciales de los providers en el gestor de secretos de la plataforma.

## Elige dónde ocurre la ejecución

Las ejecuciones pueden correr en un job de CI genérico o en una plataforma de Terraform dedicada. Un job genérico es flexible; una plataforma dedicada añade almacenamiento de state, bloqueo, historial de ejecuciones y comprobaciones de políticas sin scripts adicionales.

| Aspecto | Job de CI genérico (capítulos 4.3-4.5) | Plataforma de Terraform dedicada (capítulo 4.2) |
| --- | --- | --- |
| Almacenamiento de state y bloqueo | Tú configuras un backend | Incluido, sin necesidad de bloque de backend |
| Historial de ejecuciones y diffs de plan | Lo que conserven los logs del sistema de CI | Se conserva por workspace, es buscable |
| Comprobaciones de políticas (Sentinel/OPA) | Requiere una herramienta o script aparte | Nativas, pueden bloquear un plan o un apply |
| Esfuerzo de configuración | Bajo: reutilizas el pipeline que ya tienes | Mayor: hay que aprender una plataforma, org y workspace nuevos |

Ninguna opción es estrictamente mejor; un equipo pequeño que ya vive en GitHub o GitLab suele empezar con un job genérico y adoptar una plataforma dedicada cuando el bloqueo del state o las comprobaciones de políticas se vuelven una necesidad real.

## Qué hay que configurar

Como mínimo, hay que acordar la rama por defecto, la protección de ramas, plan en el PR, apply en el merge, quién puede aprobar un apply, dónde vive el state y cómo se inyectan los secretos. Estas políticas son parte del proceso de entrega, no un adorno opcional.

## Referencias

- [Running Terraform in automation](https://developer.hashicorp.com/terraform/tutorials/automation/automate-terraform)
- [Terraform automation tutorials](https://developer.hashicorp.com/terraform/tutorials/automation)
- [Backend configuration](https://developer.hashicorp.com/terraform/language/backend)
- [Terraform recommended practices](https://developer.hashicorp.com/terraform/cloud-docs/recommended-practices)
