# 1.4 El modelo mental de Terraform

Piensa en tres lugares:

1. Configuración: los archivos `.tf` que describen el estado deseado.
2. State: lo que Terraform registró sobre los recursos que gestiona.
3. Infraestructura real: lo que realmente existe en la plataforma.

El flujo básico es `escribir configuración -> terraform plan -> terraform apply`. `terraform plan` compara los tres lugares y muestra la diferencia; debería ser tu comando de diagnóstico más frecuente. Ejecútalo antes de cada apply.

## Por qué Terraform necesita un tercer lugar

Sería más simple si Terraform solo comparara la configuración con la infraestructura real, pero dos problemas lo impiden. Primero, cierta información no se puede recuperar solo de la plataforma: cuando eliminas un recurso de la configuración, Terraform tiene que saber cómo quitarlo y en qué orden, y una API de nube no expone esa relación. Segundo, consultar todos los atributos de cada recurso en cada comando no escala: en un entorno grande implicaría llamadas constantes a la API, latencia y límites de tasa. El state resuelve ambos problemas: es la caché y el mapa que conecta un bloque de recurso como `aws_instance.web` con un objeto real, como la instancia `i-abcd1234`, sin el cual, como dice HashiCorp, "Terraform is unable to function" (Terraform no puede funcionar).

## Recorriendo el ciclo con un ejemplo práctico

```bash
terraform apply    # la configuración ahora coincide con el state, que coincide con la infraestructura real
# ...editas main.tf, cambiando un argumento...
terraform plan      # compara los tres lugares; muestra exactamente un atributo cambiando
terraform apply     # la infraestructura real y el state se ponen al día con la nueva configuración
```

Si en cambio cambias algo a mano en la consola del provider, solo el state y la infraestructura real quedan desincronizados; el siguiente `terraform plan` actualiza el state desde la plataforma real por defecto y reporta la desviación, normalmente como una propuesta para volver a dejar el recurso como en la configuración.

## Qué vive dónde

| Pregunta | La respuesta vive en | Cómo comprobarlo |
| --- | --- | --- |
| ¿Qué quiero que exista? | Configuración | Leer los archivos `.tf` |
| ¿Qué cree Terraform que existe? | State | `terraform show`, `terraform state list` |
| ¿Qué existe realmente en la plataforma? | Infraestructura real | La consola o API del provider, o un `terraform plan` reciente |
| ¿Qué cambiará si aplico ahora? | La diferencia entre los tres | `terraform plan` |

## Errores comunes

- **Tratar el state como algo desechable.** Eliminar o editar a mano el archivo de state no elimina los recursos reales; solo hace que Terraform los olvide, lo que normalmente lleva a applies fallidos o recursos duplicados.
- **Aplicar sin un plan reciente.** Saltarse `plan`, o aplicar un plan que ya no está vigente, significa confiar en una comparación desactualizada de los tres lugares.
- **Asumir que el state es opcional cuando trabajas solo.** Incluso en solitario, un apply interrumpido puede dejar la configuración, el state y la realidad desalineados; las técnicas de recuperación del Capítulo 3 existen justamente para esto.
- **Olvidar que la "infraestructura real" puede cambiar sin Terraform.** Cualquier cosa que otro ingeniero, otro script o una sesión en la consola cambie no aparecerá hasta que el siguiente `plan` actualice el state.

## Práctica

Aplica una configuración pequeña, y luego cambia a mano ese mismo recurso fuera de Terraform (por ejemplo, edita directamente el contenido de un archivo local en lugar de hacerlo mediante la configuración). Ejecuta `terraform plan` e identifica cuál de los tres lugares se movió, y cuáles dos siguieron de acuerdo.

## Referencias

- [State (Terraform language)](https://developer.hashicorp.com/terraform/language/state)
- [Purpose of Terraform state](https://developer.hashicorp.com/terraform/language/state/purpose)
- [Command: plan](https://developer.hashicorp.com/terraform/cli/commands/plan)
- [Command: refresh](https://developer.hashicorp.com/terraform/cli/commands/refresh)
- [Core workflow](https://developer.hashicorp.com/terraform/intro/core-workflow)
