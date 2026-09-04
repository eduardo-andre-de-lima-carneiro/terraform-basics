# 1.1 Infraestructura como código

La infraestructura como código (IaC, por sus siglas en inglés) usa un modelo descriptivo y versionado para definir y desplegar infraestructura — redes, máquinas virtuales, balanceadores de carga, bases de datos y la topología que las conecta — en lugar de procesos manuales y clics en la consola. Igual que el mismo código fuente siempre produce el mismo binario, una configuración de IaC produce el mismo entorno cada vez que se despliega.

## El problema que resuelve

Sin infraestructura como código, la configuración real vive solo en una consola y en la cabeza de unas pocas personas. Cada entorno deriva hacia un "copo de nieve" único y no documentado que no se puede reproducir automáticamente, y reconstruirlo o auditarlo se convierte en una conjetura. Terraform y herramientas similares mantienen la configuración prevista en archivos estructurados, de modo que los cambios de infraestructura pasan por la misma revisión e historial de control de versiones que el código de la aplicación.

## Ejemplos prácticos

- **Duplicación multi-región o multi-cuenta.** Aplica la misma configuración para levantar un entorno idéntico en una segunda región o cuenta de nube, en lugar de repetir la configuración manual a mano.
- **Entornos de prueba y revisión efímeros.** Provisiona una pila completa para una pull request o una prueba de carga, y elimínala cuando el trabajo termine, para que los entornos dejen de ser escasos, compartidos y desactualizados.
- **Recuperación ante desastres.** Reconstruye la topología de producción a partir de la configuración en una nueva región tras una interrupción, en lugar de reconstruirla de memoria y tickets de soporte.
- **Landing zones estandarizadas.** Ofrece a cada equipo la misma red base, registro y control de acceso reutilizando un módulo, en lugar de incorporar a cada equipo manualmente.

## Beneficios

- **Consistencia.** La misma configuración siempre produce el mismo entorno, lo que elimina la desviación de configuración (drift) y las sorpresas de "funciona en mi entorno".
- **Idempotencia.** Aplicar una configuración repetidamente converge el entorno al mismo estado, sin importar si el destino empieza vacío o parcialmente construido.
- **Velocidad a escala.** Entornos que antes tomaban días de trabajo manual pueden aprovisionarse, duplicarse o eliminarse en minutos.
- **Revisión y reversión.** Los cambios de infraestructura pasan por la misma pull request, diff e historial de versiones que el código de la aplicación, así un cambio problemático puede revisarse antes de publicarse y revertirse después.
- **Un lenguaje compartido entre equipos.** Desarrolladores y operadores leen los mismos archivos, lo que reduce los traspasos y errores de traducción del aprovisionamiento por ticket.

## Desafíos

- **Una curva de aprendizaje.** Los equipos deben aprender un lenguaje de configuración, un modelo de state y el esquema de recursos de un provider antes de ser productivos.
- **Gestión de state y drift.** Si alguien cambia un recurso fuera de la herramienta, por ejemplo directamente en una consola, el state registrado y la realidad quedan en desacuerdo hasta que se reconcilian.
- **Radio de impacto (blast radius).** Un solo `apply` incorrecto puede cambiar o destruir muchos recursos a la vez; las salvaguardas del Capítulo 3, como revisar primero un plan, existen por este riesgo.
- **Secretos y datos sensibles.** La configuración y el state pueden terminar guardando credenciales u otros valores sensibles si no se manejan deliberadamente.
- **Probar código de infraestructura.** Validar una configuración a fondo normalmente implica aprovisionarla de verdad en algún lugar, lo que cuesta tiempo y dinero que probar código de aplicación no cuesta.

## Práctica

Escribe cada paso manual que darías para crear un recurso pequeño en la consola de tu nube. Esa lista es el valor que ofrece la infraestructura como código: convierte esos pasos en un archivo que puedes revisar, repetir y revertir. Luego elige un elemento de los desafíos anteriores y anota cómo tendría que cambiar el proceso de tu equipo para manejarlo.

## Referencias

- [What is infrastructure as code? (Microsoft Learn, Azure DevOps)](https://learn.microsoft.com/en-us/devops/deliver/what-is-infrastructure-as-code)
- [What is infrastructure as code? (AWS)](https://aws.amazon.com/what-is/iac/)
- [What is Terraform? (HashiCorp)](https://developer.hashicorp.com/terraform/intro)
- [Terraform use cases](https://developer.hashicorp.com/terraform/intro/use-cases)
