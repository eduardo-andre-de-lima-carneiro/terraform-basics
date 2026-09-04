# 4.4 GitLab CI/CD

GitLab CI/CD ejecuta Terraform en etapas de pipeline y también puede alojar el state a través de su función de state de Terraform gestionado.

## Pipeline mínimo

```yaml
stages: [validate, plan, apply]

default:
  image:
    name: hashicorp/terraform:latest
    entrypoint: [""]

validate:
  stage: validate
  script:
    - terraform init
    - terraform validate

plan:
  stage: plan
  script:
    - terraform plan -out plan.tfplan
  artifacts:
    paths: [plan.tfplan]
    expire_in: 1 day

apply:
  stage: apply
  script:
    - terraform apply plan.tfplan
  when: manual
  only: [main]
```

La sobrescritura `entrypoint: [""]` es obligatoria: la imagen `hashicorp/terraform` ejecuta el binario de Terraform como su entrypoint, así que sin ella las líneas de `script` no pueden arrancar una shell.

GitLab ya no distribuye su propia plantilla de pipeline de Terraform ni la imagen de job asociada; el pipeline de arriba usa directamente la imagen `hashicorp/terraform` de HashiCorp, que sigue siendo una forma válida y sin dependencias de ejecutar comandos de Terraform en un job. Para un proyecto más grande, la recomendación actual de GitLab es construir pipelines de validate/plan/apply a partir de [componentes de CI/CD](https://docs.gitlab.com/ee/ci/components/) reutilizables de su catálogo en lugar de escribir cada etapa a mano.

## Buenas prácticas

- Mantén `apply` como un job manual en la rama por defecto para que una persona lo confirme.
- Pasa el artefacto `plan.tfplan` guardado a `apply` para que ejecute exactamente lo que se revisó.
- Un archivo de plan guardado contiene valores de configuración y de state en texto plano. Mantén el artefacto de vida corta (`expire_in`) y restringido al proyecto.
- Almacena las credenciales como variables de CI/CD enmascaradas y protegidas.
- Usa el state de Terraform gestionado por GitLab o un backend externo, nunca el repositorio.

## Referencias

- [Terraform with GitLab (Infrastructure as Code)](https://docs.gitlab.com/ee/user/infrastructure/iac/)
- [GitLab-managed Terraform state](https://docs.gitlab.com/ee/user/infrastructure/iac/terraform_state.html)
- [CI/CD variables (masked and protected)](https://docs.gitlab.com/ee/ci/variables/)
- [Job artifacts](https://docs.gitlab.com/ee/ci/jobs/job_artifacts.html)
- [hashicorp/terraform image](https://hub.docker.com/r/hashicorp/terraform)
- [CI/CD components](https://docs.gitlab.com/ee/ci/components/)
