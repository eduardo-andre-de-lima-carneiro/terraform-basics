# 4.4 GitLab CI/CD

GitLab CI/CD exécute Terraform dans des étapes de pipeline et peut aussi héberger le state via sa fonctionnalité de state Terraform gérée.

## Pipeline minimal

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

Le remplacement `entrypoint: [""]` est indispensable : l'image `hashicorp/terraform` exécute le binaire Terraform comme point d'entrée, donc sans cela les lignes `script` ne peuvent pas lancer de shell.

## Bonnes pratiques

- Gardez `apply` comme un job manuel sur la branche par défaut afin qu'un humain le confirme.
- Passez l'artefact `plan.tfplan` enregistré à `apply` pour qu'il exécute exactement ce qui a été relu.
- Un fichier de plan enregistré contient des valeurs de configuration et de state en clair. Gardez l'artefact éphémère (`expire_in`) et restreint au projet.
- Stockez les identifiants comme variables CI/CD masquées et protégées.
- Utilisez le state Terraform géré par GitLab ou un backend externe, jamais le dépôt.

## Références

- [Terraform with GitLab (Infrastructure as Code)](https://docs.gitlab.com/ee/user/infrastructure/iac/)
- [GitLab-managed Terraform state](https://docs.gitlab.com/ee/user/infrastructure/iac/terraform_state.html)
- [CI/CD variables (masked and protected)](https://docs.gitlab.com/ee/ci/variables/)
- [Job artifacts](https://docs.gitlab.com/ee/ci/jobs/job_artifacts.html)
- [hashicorp/terraform image](https://hub.docker.com/r/hashicorp/terraform)
