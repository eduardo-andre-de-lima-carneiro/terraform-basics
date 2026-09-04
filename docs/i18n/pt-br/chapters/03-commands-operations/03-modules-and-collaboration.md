# 3.3 Módulos e Colaboração

Um módulo é uma pasta de arquivos `.tf` feita para ser reutilizada. A pasta em que você executa os comandos é o módulo raiz; ela pode chamar módulos filhos.

## Chame um módulo

```hcl
module "notes" {
  source  = "./modules/notes"
  content = "shared example"
}
```

As entradas são passadas como argumentos, os resultados são lidos dos valores de `output` do módulo, e o `terraform init` instala quaisquer fontes de módulo remotas.

## Fixando a versão de um módulo

Um `source` pode apontar para um caminho local (como acima, sem versionamento — sempre usa o que está no disco), ou para uma fonte remota como o Terraform Registry público, que aceita uma restrição `version` separada:

```hcl
module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "6.0.1"   # or a range, e.g. "~> 6.0"
}
```

Isso é apenas sintaxe ilustrativa — não faça `apply` de um módulo de nuvem real nos exercícios deste curso; use-a para reconhecer o padrão quando encontrá-lo em um projeto real. Fontes de caminho local sempre usam o código atual e ignoram `version`; apenas fontes de registry, Git e outras fontes remotas são versionadas. Depois de alterar uma restrição `version`, execute `terraform init -upgrade` para que o Terraform resolva novamente e baixe a versão recém-permitida em vez de manter a que já estava instalada.

## Movendo um resource para dentro de um módulo

Mover um resource altera o endereço dele (por exemplo, `local_file.notes` vira `module.notes.local_file.notes`). Sem ajuda, o `terraform plan` mostraria esse resource sendo destruído e recriado. Diga ao Terraform que é o mesmo objeto com um bloco `moved`:

```hcl
moved {
  from = local_file.notes
  to   = module.notes.local_file.notes
}
```

Agora o `terraform plan` reporta nenhuma mudança. (O `terraform state mv` faz a mesma coisa como um comando pontual, em vez de configuração versionada.)

## Colabore com segurança

- Mantenha os módulos pequenos e focados em um único propósito.
- Fixe as versões de módulos e providers para que os colegas obtenham o mesmo resultado.
- Revise os planos nos pull requests, não apenas o diff da configuração.
- Compartilhe o state por um backend remoto para que as execuções não colidam.

## Prática

Mova um resource para uma pasta `./modules/notes`, exponha uma entrada e um output, adicione o bloco `moved` correspondente e execute `terraform plan`. Confirme que ele reporta nenhuma mudança.

## Referências

- [Modules overview](https://developer.hashicorp.com/terraform/language/modules)
- [Module blocks](https://developer.hashicorp.com/terraform/language/modules/syntax)
- [Module sources](https://developer.hashicorp.com/terraform/language/modules/sources)
- [Version constraints](https://developer.hashicorp.com/terraform/language/expressions/version-constraints)
- [Refactoring with `moved` blocks](https://developer.hashicorp.com/terraform/language/modules/develop/refactoring)
- [Command: state mv](https://developer.hashicorp.com/terraform/cli/commands/state/mv)
- [Command: init](https://developer.hashicorp.com/terraform/cli/commands/init)
- [Module creation tutorial](https://developer.hashicorp.com/terraform/tutorials/modules/module-create)
