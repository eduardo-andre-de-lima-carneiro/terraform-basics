# 1.2 Por qué Terraform

Terraform es una herramienta ampliamente usada para infraestructura como código. Lee configuración declarativa, habla con las plataformas a través de providers y mantiene un archivo de state para saber qué es lo que ya gestiona.

## Qué la hace útil

- Un solo flujo de trabajo (`init`, `plan`, `apply`) funciona en muchos providers.
- Un plan muestra los cambios exactos antes de que ocurra nada.
- El state permite que Terraform actualice y elimine solo lo que creó.
- Los módulos empaquetan configuración para reutilizarla entre equipos y entornos.

## Terraform frente a otras herramientas

Terraform es una de varias herramientas de automatización de infraestructura, y cada una optimiza para un trabajo distinto. La siguiente tabla compara las herramientas que probablemente te encuentres en un equipo que ya tiene algo de automatización en marcha.

| Herramienta | Categoría | Enfoque | Lenguaje de configuración | Modelo de ejecución | Alcance | Seguimiento de state |
| --- | --- | --- | --- | --- | --- | --- |
| **Terraform** | Aprovisionamiento (IaC) | Declarativo, estado deseado | HCL (o JSON) | Sin agente; tú o un pipeline ejecutan `plan`/`apply` | Muchos providers: plataformas de nube, SaaS, on-prem, Kubernetes | Un archivo de state explícito registra cada recurso gestionado |
| **Ansible** | Gestión de configuración | Principalmente playbooks procedurales, con módulos idempotentes | YAML | Sin agente; envía módulos por SSH/WinRM desde un nodo de control | Configurar y actualizar máquinas que ya existen; también puede llamar APIs de nube para aprovisionar | Sin archivo de state dedicado; cada ejecución inspecciona el sistema real |
| **Chef / Puppet** | Gestión de configuración | Recursos declarativos dentro de un DSL (Ruby para Chef, DSL propio de Puppet) | Ruby (Chef) / DSL de Puppet | Basado en agente, clásicamente un modelo pull desde un servidor central | Configurar y actualizar máquinas que ya existen | Sin archivo de state de infraestructura; los agentes reconcilian la máquina local en cada ejecución |
| **AWS CloudFormation** | Aprovisionamiento (IaC) | Declarativo, estado deseado | Plantillas JSON o YAML | Sin agente; un servicio administrado de AWS ejecuta el despliegue | Solo AWS | El state es el stack de CloudFormation, gestionado por AWS |
| **Pulumi** | Aprovisionamiento (IaC) | Estado deseado, expresado con código de propósito general | TypeScript, Python, Go, C#, Java o YAML | Sin agente; un motor de CLI ejecuta tu programa y aplica el diff | Muchos providers, un alcance similar al de Terraform | Archivo de state, guardado localmente o en Pulumi Cloud |

## Cómo leer la comparación

- **Aprovisionamiento frente a gestión de configuración.** Terraform, CloudFormation y Pulumi crean y cambian la infraestructura en sí: la VM, la base de datos, la red. Ansible, Chef y Puppet configuran principalmente software en una máquina que ya existe. HashiCorp es explícito al decir que "Terraform is not a configuration management tool" (Terraform no es una herramienta de gestión de configuración), y las dos categorías suelen ser complementarias en lugar de competir: Terraform aprovisiona un servidor y, después, un paso de bootstrapping (o un playbook de Ansible posterior) configura el software que corre en él.
- **Multi-nube frente a nube única.** Terraform, Ansible y Pulumi funcionan en muchos providers con un solo flujo de trabajo. CloudFormation es declarativo y confiable, pero solo gestiona AWS.
- **Lenguaje específico de dominio frente a código de propósito general.** El HCL de Terraform y el JSON/YAML de CloudFormation son deliberadamente acotados, lo que mantiene un plan fácil de leer y revisar en una pull request. Pulumi cambia esa acotación por todo el poder de un lenguaje de propósito general: bucles, pruebas y gestores de paquetes que quizá ya uses, a costa de una superficie más grande sobre la que razonar.
- **El state como fuente de verdad.** Terraform, CloudFormation y Pulumi comparan un state registrado con la realidad antes de cambiar cualquier cosa, lo que es lo que hace posible un "plan". Ansible y las herramientas clásicas de gestión de configuración generalmente no mantienen ese tipo de archivo de state de infraestructura; inspeccionan el destino en cada ejecución y confían en que cada módulo sea idempotente.

Nada de esto convierte a Terraform en la herramienta correcta para cada trabajo. Un equipo que ya invirtió en Ansible para gestión de configuración no necesita reemplazarlo; Terraform normalmente se ubica debajo, aprovisionando las máquinas que Ansible luego configura.

## Práctica

Piensa en dos plataformas que use tu equipo, como un proveedor de nube y un servicio de DNS. Con Terraform, ambas se gestionan con los mismos comandos y el mismo formato de archivo. Anota dónde esa consistencia le ahorraría tiempo a tu equipo hoy, e identifica una tarea de tu stack actual en la que una herramienta de gestión de configuración todavía sería mejor que Terraform.

## Referencias

- [What is Terraform? (HashiCorp)](https://developer.hashicorp.com/terraform/intro)
- [Terraform use cases](https://developer.hashicorp.com/terraform/intro/use-cases)
- [Terraform Registry: providers](https://registry.terraform.io/browse/providers)
- [Terraform workflow overview](https://developer.hashicorp.com/terraform/intro/core-workflow)
- [Terraform vs. other software (comparison index)](https://developer.hashicorp.com/terraform/intro/vs)
- [Terraform vs. Chef, Puppet, and other configuration management tools](https://developer.hashicorp.com/terraform/intro/vs/chef-puppet)
- [Terraform vs. CloudFormation](https://developer.hashicorp.com/terraform/intro/vs/cloudformation)
- [How Ansible works (Red Hat)](https://www.redhat.com/en/ansible-collaborative/how-ansible-works)
- [How Pulumi works](https://www.pulumi.com/docs/iac/concepts/how-pulumi-works/)
- [AWS CloudFormation](https://aws.amazon.com/cloudformation/)
