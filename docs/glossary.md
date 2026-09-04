# Glossary

- **Configuration:** The `.tf` files that describe the desired infrastructure.
- **Provider:** A plugin that lets Terraform manage a specific platform or API.
- **Resource:** A single infrastructure object managed by a Terraform configuration.
- **Data source:** A read-only lookup of information defined outside the configuration.
- **State:** The file where Terraform records the resources it manages and their last known values.
- **Plan:** A preview of the actions Terraform would take to reach the desired state.
- **Saved plan file:** The output of `terraform plan -out`, applied later so `apply` runs exactly what was reviewed.
- **Apply:** Executing a plan to create, update, or destroy resources.
- **Module:** A reusable group of configuration files called with input variables.
- **Backend:** The place where Terraform stores state, such as local disk or a remote service.
- **Workspace:** A named state instance that lets one configuration manage multiple environments.
- **HCP Terraform:** HashiCorp's managed platform for remote runs, state, and policy (formerly Terraform Cloud).
- **Dependency lock file:** `.terraform.lock.hcl`, which pins provider versions and checksums; commit it to version control.
- **Drift:** A difference between real infrastructure and the recorded state.
- **Variable:** A named input that parameterizes a configuration or module.
- **Output:** A named value a configuration or module exposes after apply.
- **`moved` block:** Configuration that tells Terraform a resource's address changed so it is not destroyed and recreated.

## References

- [Terraform glossary (HashiCorp)](https://developer.hashicorp.com/terraform/docs/glossary)
- [Terraform language documentation](https://developer.hashicorp.com/terraform/language)
- [Terraform CLI documentation](https://developer.hashicorp.com/terraform/cli)
