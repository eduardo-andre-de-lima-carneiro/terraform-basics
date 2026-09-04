# 1.3 El modelo declarativo de Terraform

Describes el resultado que quieres, no los pasos para llegar a él. Terraform compara tu configuración con el state actual y con la plataforma real, y luego decide qué acciones son necesarias.

## Declarativo frente a imperativo

Un script imperativo dice "crea esto, luego conecta aquello". Una configuración declarativa dice "estos recursos deben existir con estos ajustes". Si un recurso ya coincide, Terraform no hace nada. Si divergió, Terraform propone una corrección.

```bash
# Imperativo: especificas cada paso, en orden, y volver a ejecutarlo a ciegas puede fallar o duplicar trabajo
aws ec2 create-vpc ...
aws ec2 create-subnet ...
aws ec2 create-security-group ...
```

```hcl
# Declarativo: especificas el estado final; Terraform calcula los pasos y su orden
resource "aws_vpc" "main" { cidr_block = "10.0.0.0/16" }
resource "aws_subnet" "app" { vpc_id = aws_vpc.main.id, cidr_block = "10.0.1.0/24" }
```

El bloque declarativo nunca dice "crea primero la VPC". Terraform lo deduce porque la configuración de la subred hace referencia a `aws_vpc.main.id`.

## Los providers hacen el trabajo

Terraform en sí mismo no conoce ninguna API de nube. Cada provider traduce los bloques de recursos en llamadas a la API y reporta los resultados de vuelta al state.

## Cómo decide Terraform el orden

Cada referencia entre recursos —`aws_vpc.main.id` en el ejemplo anterior— se convierte en una arista de un grafo de dependencias. Terraform construye ese grafo a partir de tu configuración, verifica que no tenga ciclos, y lo recorre de modo que un recurso solo se crea, actualiza o destruye una vez que todo de lo que depende ya terminó. Los recursos que no dependen entre sí pueden ejecutarse al mismo tiempo, hasta 10 en paralelo por defecto, por lo que un modelo declarativo tiende a aplicarse más rápido que un script imperativo equivalente a medida que tu configuración crece. Puedes inspeccionar el grafo tú mismo con `terraform graph`, y forzar una arista explícita con `depends_on` cuando existe una dependencia que no aparece como referencia a un atributo.

Destruir recursos recorre un grafo relacionado pero separado, porque el orden seguro para eliminar cosas suele ser el inverso del orden usado para crearlas.

## Dónde el modelo declarativo se tensiona

El modelo no está libre de compromisos:

- **Todavía existen pasos verdaderamente imperativos.** Una migración de base de datos, una importación de datos de una sola vez, o una llamada a una API sensible al orden no encajan bien en "este recurso debe existir". La guía de HashiCorp sobre los provisioners —la vía de escape para ejecutar un script durante la creación o destrucción— es tratarlos como último recurso: primero revisa si existe una forma nativa del provider de hacer lo mismo, porque Terraform no puede modelar lo que realmente hace un provisioner ni puede saber si tuvo éxito de la forma en que rastrea un recurso.
- **La plataforma real puede no coincidir con el grafo.** Las API de nube a veces son de consistencia eventual, así que un recurso puede reportar éxito antes de estar completamente disponible en otro lugar, lo que ocasionalmente aparece como un error de dependencia aunque la configuración fuera correcta.
- **Declarativo no significa automático.** Tú sigues eligiendo los recursos, los argumentos y los límites de los módulos; Terraform solo automatiza el orden y la comparación de diferencias, no el diseño.

## Práctica

Lee un bloque de recurso corto y describe, en lenguaje sencillo, el estado final que declara. Luego predice qué haría Terraform si ese recurso ya existiera sin cambios. Por último, ejecuta `terraform graph` en una configuración pequeña con dos recursos relacionados e identifica la arista que corresponde a la referencia entre ellos.

## Referencias

- [How Terraform works](https://developer.hashicorp.com/terraform/intro#how-does-terraform-work)
- [Providers (Terraform language)](https://developer.hashicorp.com/terraform/language/providers)
- [Resources overview](https://developer.hashicorp.com/terraform/language/resources)
- [Resource behavior and the dependency graph](https://developer.hashicorp.com/terraform/language/resources/behavior)
- [The dependency graph (internals)](https://developer.hashicorp.com/terraform/internals/graph)
- [Command: graph](https://developer.hashicorp.com/terraform/cli/commands/graph)
- [The `depends_on` meta-argument](https://developer.hashicorp.com/terraform/language/meta-arguments/depends_on)
- [Provisioners: a last resort](https://developer.hashicorp.com/terraform/language/resources/provisioners/syntax)
