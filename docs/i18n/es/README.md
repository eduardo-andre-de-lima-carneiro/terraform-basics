# Fundamentos de Terraform

> Aprende Terraform entendiendo qué es, practicando lo que hace y ganando confianza paso a paso.

Fundamentos de Terraform es un curso práctico y guiado para personas que empiezan con Terraform, que vienen de consolas de nube manuales o de scripts de aprovisionamiento, o que buscan un modelo mental más claro para el día a día de la infraestructura como código.

[Empieza el curso](menu.md) | [Elige tu idioma](#idiomas) | [Contribuye](../../../CONTRIBUTING.md)

## Por qué existe este curso

La documentación de Terraform puede ser técnicamente precisa y aun así resultar difícil de abordar. Este proyecto convierte las ideas esenciales en un camino guiado: explicaciones breves, comandos reales, resultados visibles y ejercicios que se pueden practicar en un directorio de trabajo temporal.

El objetivo no es memorizar una lista de comandos. El objetivo es entender el estado de tu infraestructura, hacer cambios intencionados y recuperarte con calma cuando algo sale mal.

## Qué vas a aprender

- Cómo la infraestructura como código protege y explica el historial de un entorno.
- Cómo encajan entre sí la configuración, los providers, los recursos, el state y los módulos de Terraform.
- Cómo instalar y configurar Terraform para proyectos personales o de equipo.
- Cómo revisar un plan antes de aplicarlo.
- Cómo organizar módulos, almacenar el state de forma remota y colaborar con seguridad.
- Cómo elegir el comando de recuperación adecuado ante un cambio no deseado.

## Mapa del curso

| Capítulo                                                                                    | Enfoque                                              | Vas a practicar                                                           |
| ------------------------------------------------------------------------------------------ | -------------------------------------------------- | -------------------------------------------------------------------------- |
| [1. Conceptos básicos](chapters/01-basic-concepts/README.md)                               | Las ideas detrás de la infraestructura como código y Terraform | Pensar en estado deseado, planes y grafos de recursos            |
| [2. Instalación y configuración](chapters/02-installation-configuration/README.md)         | Dejar Terraform listo para usar                    | Comprobar la instalación, los providers, las credenciales y los backends   |
| [3. Comandos y operaciones](chapters/03-commands-operations/README.md)                     | Construir un flujo de trabajo diario fiable        | Init, plan, apply, state, módulos, state remoto y recuperación             |
| [4. Integración con plataformas](chapters/04-platform-integration/README.md)               | Ejecutar Terraform en plataformas gestionadas y de CI/CD | Ejecuciones remotas, pipelines, permisos, secretos y entrega segura   |
| [5. Integración con IDE y editores](chapters/05-ide-integration/README.md)                 | Usar Terraform a través de editores de código e IDE | Redacción, validación, formato, navegación y configuración de herramientas |

## Una primera práctica rápida

Una vez instalado Terraform, crea un directorio de práctica desechable:

```bash
mkdir terraform-practice
cd terraform-practice
cat > main.tf <<'EOF'
resource "local_file" "notes" {
  content  = "My first Terraform file\n"
  filename = "${path.module}/notes.txt"
}
EOF
terraform init
terraform plan
terraform apply
terraform destroy   # clean up: the whole directory is disposable
```

Acabas de crear una configuración, inicializar el directorio de trabajo, previsualizar el cambio, aplicarlo y volver a eliminarlo. El Capítulo 1 explica qué ocurrió en cada etapa.

## Cómo usar la documentación

1. Empieza por el [menú de documentación](menu.md).
2. Lee el Capítulo 1 antes de lanzarte a memorizar comandos.
3. Completa los pasos de configuración del Capítulo 2.
4. Trabaja el Capítulo 3 en un directorio de trabajo desechable.
5. Explora el Capítulo 4 para la plataforma que usa tu equipo.
6. Lee el Capítulo 5 para tu editor de código o IDE.
7. Usa el [glosario](glossary.md) siempre que un término te resulte desconocido.

Cada lección es un archivo Markdown independiente, enlazado con rutas relativas para que se pueda leer directamente en GitHub.

## Idiomas

El curso está disponible en cuatro idiomas:

- [English](../../../README.md)
- [Français](../fr/README.md)
- [Português (Brasil)](../pt-br/README.md)
- [Español](README.md)

## Valores del proyecto

- **Práctico:** los ejemplos deben conducir a algo que el aprendiz pueda observar.
- **Accesible:** explicar la idea antes de introducir el comando.
- **Seguro:** usar directorios de trabajo desechables y hacer explícitas las operaciones destructivas.
- **Abierto:** mantener la documentación gratuita, reutilizable y fácil de mejorar.

## Contribuir

¿Encontraste una explicación confusa, un ejercicio que falta o un enlace roto? Lee la [guía de contribución](../../../CONTRIBUTING.md) y ayuda a mejorar la primera experiencia con Terraform del próximo aprendiz.

## Origen

Este curso surgió de una experiencia de DevSecOps apoyando a equipos que estaban migrando de cambios manuales en la nube a infraestructura como código. La documentación oficial y los sitios de referencia eran útiles, pero algunas personas necesitaban una ruta más guiada y práctica para entrar en el tema. Fundamentos de Terraform se creó para ofrecer esa ruta y para hacer que el proceso de aprendizaje sea más fácil de compartir.

El proyecto es intencionadamente colaborativo. Los comentarios, las correcciones, los ejemplos y las traducciones son bienvenidos.
