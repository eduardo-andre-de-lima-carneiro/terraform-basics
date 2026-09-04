# 1.4 El modelo mental de Terraform

Piensa en tres lugares:

1. Configuración: los archivos `.tf` que describen el estado deseado.
2. State: lo que Terraform registró sobre los recursos que gestiona.
3. Infraestructura real: lo que existe de verdad en la plataforma.

El flujo básico es `escribir configuración -> terraform plan -> terraform apply`. `terraform plan` compara los tres lugares y muestra la diferencia; debería ser tu comando de diagnóstico más frecuente. Ejecútalo antes de cada apply.

## Referencias

- [State (Terraform language)](https://developer.hashicorp.com/terraform/language/state)
- [Purpose of Terraform state](https://developer.hashicorp.com/terraform/language/state/purpose)
- [Command: plan](https://developer.hashicorp.com/terraform/cli/commands/plan)
- [Core workflow](https://developer.hashicorp.com/terraform/intro/core-workflow)
