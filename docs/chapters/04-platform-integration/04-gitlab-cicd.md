# 4.4 GitLab CI/CD

GitLab CI/CD runs Terraform in pipeline stages and can also host the state through its managed Terraform state feature.

## Minimal pipeline

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

The `entrypoint: [""]` override is required: the `hashicorp/terraform` image runs the Terraform binary as its entrypoint, so without it the `script` lines cannot start a shell.

## Good practices

- Keep `apply` a manual job on the default branch so a human confirms it.
- Pass the saved `plan.tfplan` artifact to `apply` so it runs exactly what was reviewed.
- A saved plan file holds configuration and state values in clear text. Keep the artifact short-lived (`expire_in`) and restricted to the project.
- Store credentials as masked, protected CI/CD variables.
- Use GitLab-managed Terraform state or an external backend, never the repository.

## References

- [Terraform with GitLab (Infrastructure as Code)](https://docs.gitlab.com/ee/user/infrastructure/iac/)
- [GitLab-managed Terraform state](https://docs.gitlab.com/ee/user/infrastructure/iac/terraform_state.html)
- [CI/CD variables (masked and protected)](https://docs.gitlab.com/ee/ci/variables/)
- [Job artifacts](https://docs.gitlab.com/ee/ci/jobs/job_artifacts.html)
- [hashicorp/terraform image](https://hub.docker.com/r/hashicorp/terraform)
