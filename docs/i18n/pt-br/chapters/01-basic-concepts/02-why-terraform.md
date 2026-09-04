# 1.2 Por que Terraform

O Terraform é uma ferramenta amplamente usada para infraestrutura como código. Ele lê configuração declarativa, conversa com as plataformas por meio de providers e mantém um arquivo de state para saber o que já gerencia.

## O que o torna útil

- Um único fluxo de trabalho (`init`, `plan`, `apply`) funciona em muitos providers.
- Um plano mostra exatamente as mudanças antes de qualquer coisa acontecer.
- O state permite que o Terraform atualize e remova apenas o que ele mesmo criou.
- Módulos empacotam configuração para reutilização entre equipes e ambientes.

## Terraform comparado a outras ferramentas

O Terraform é uma entre várias ferramentas de automação de infraestrutura, e cada uma otimiza para um trabalho diferente. A tabela compara as ferramentas que você provavelmente vai encontrar em uma equipe que já tem alguma automação em uso.

| Ferramenta | Categoria | Abordagem | Linguagem de configuração | Modelo de execução | Escopo | Rastreamento de state |
| --- | --- | --- | --- | --- | --- | --- |
| **Terraform** | Provisionamento (IaC) | Declarativo, estado desejado | HCL (ou JSON) | Sem agente; você ou um pipeline executam `plan`/`apply` | Muitos providers: plataformas de nuvem, SaaS, on-prem, Kubernetes | Um arquivo de state explícito registra cada resource gerenciado |
| **Ansible** | Gestão de configuração | Playbooks majoritariamente procedurais, com módulos idempotentes | YAML | Sem agente; envia módulos via SSH/WinRM a partir de um nó de controle | Configurar e atualizar máquinas que já existem; também pode chamar APIs de nuvem para provisionar | Sem arquivo de state dedicado; cada execução inspeciona o sistema real |
| **Chef / Puppet** | Gestão de configuração | Recursos declarativos dentro de uma DSL (Ruby no Chef, DSL própria do Puppet) | Ruby (Chef) / DSL do Puppet | Baseado em agente, classicamente um modelo pull a partir de um servidor central | Configurar e atualizar máquinas que já existem | Sem arquivo de state de infraestrutura; os agentes reconciliam a máquina local a cada execução |
| **AWS CloudFormation** | Provisionamento (IaC) | Declarativo, estado desejado | Templates JSON ou YAML | Sem agente; um serviço gerenciado da AWS executa o deployment | Somente AWS | O state é a stack do CloudFormation, gerenciada pela AWS |
| **Pulumi** | Provisionamento (IaC) | Estado desejado, expresso com código de propósito geral | TypeScript, Python, Go, C#, Java ou YAML | Sem agente; um engine de CLI executa seu programa e aplica o diff | Muitos providers, escopo semelhante ao do Terraform | Arquivo de state, guardado localmente ou no Pulumi Cloud |

## Como ler a comparação

- **Provisionamento versus gestão de configuração.** Terraform, CloudFormation e Pulumi criam e alteram a infraestrutura em si: a VM, o banco de dados, a rede. Ansible, Chef e Puppet configuram principalmente software em uma máquina que já existe. A HashiCorp é explícita ao dizer que "Terraform is not a configuration management tool" (o Terraform não é uma ferramenta de gestão de configuração), e as duas categorias costumam ser complementares em vez de concorrentes: o Terraform provisiona um servidor e, depois, uma etapa de bootstrapping (ou um playbook do Ansible subsequente) configura o software que roda nele.
- **Multi-nuvem versus nuvem única.** Terraform, Ansible e Pulumi funcionam em muitos providers com um único fluxo de trabalho. O CloudFormation é declarativo e confiável, mas só gerencia a AWS.
- **Linguagem específica de domínio versus código de propósito geral.** O HCL do Terraform e o JSON/YAML do CloudFormation são deliberadamente restritos, o que mantém um plano fácil de ler e revisar em um pull request. O Pulumi troca essa restrição pelo poder completo de uma linguagem de propósito geral: loops, testes e gerenciadores de pacotes que você talvez já use, ao custo de uma superfície maior para raciocinar.
- **O state como fonte da verdade.** Terraform, CloudFormation e Pulumi comparam um state registrado com a realidade antes de mudar qualquer coisa, o que é o que torna um "plan" possível. O Ansible e as ferramentas clássicas de gestão de configuração geralmente não mantêm esse tipo de arquivo de state de infraestrutura; elas inspecionam o destino a cada execução e confiam na idempotência de cada módulo.

Nada disso torna o Terraform a ferramenta certa para todo trabalho. Uma equipe que já investiu em Ansible para gestão de configuração não precisa substituí-lo; o Terraform normalmente fica por baixo, provisionando as máquinas que o Ansible depois configura.

## Prática

Pense em duas plataformas que sua equipe usa, como um provedor de nuvem e um serviço de DNS. Com o Terraform, ambas são gerenciadas com os mesmos comandos e o mesmo formato de arquivo. Anote onde essa consistência economizaria tempo da sua equipe hoje, e identifique uma tarefa do seu stack atual em que uma ferramenta de gestão de configuração ainda seria melhor que o Terraform.

## Referências

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
