# 5.6 Fluxos de Trabalho Assistidos por IA

Os assistentes de editor podem rascunhar blocos de resource, explicar um erro ou sugerir uma estrutura de variáveis. Eles são úteis para um primeiro rascunho e para providers desconhecidos.

## Use-os com segurança

- Trate o HCL gerado como uma proposta. Execute `terraform validate` e leia o `terraform plan` antes de confiar nele.
- Nunca cole credenciais reais, arquivos de state ou identificadores privados em um prompt.
- Confirme os nomes dos argumentos na documentação do provider; os assistentes inventam atributos plausíveis, porém errados. Um modo de falha comum: a sugestão é HCL válido e até corresponde a um argumento que existiu em uma versão mais antiga do provider, mas que foi renomeado ou removido desde então — passa por uma leitura rápida, falha no `terraform validate` ou, pior, muda silenciosamente no `plan` porque o atributo simplesmente é ignorado.
- Tenha atenção redobrada com qualquer coisa que exclua ou substitua resources.
- Algumas extensões de editor agora expõem os esquemas dos providers diretamente ao assistente, em vez de depender dos dados de treinamento dele — por exemplo, a extensão HashiCorp Terraform do VS Code traz um Terraform MCP server opcional (`terraform.mcp.server.enable`) que permite a um assistente conectado consultar o Terraform Registry. Isso reduz o modo de falha de "atributo inventado" acima, mas não elimina a necessidade de ler o plano.

## Onde eles mais ajudam

- Explicar um erro de validação ou de plano em linguagem simples.
- Converter uma configuração de console em um primeiro rascunho de bloco de resource.
- Sugerir a estrutura de blocos `for_each` ou `dynamic` para eliminar repetição.

## Prática

Peça a um assistente para gerar um resource `local_file`, depois verifique cada argumento na documentação do provider e confirme que o plano faz exatamente o que você esperava.

## Referências

- [Terraform Registry (authoritative provider schemas)](https://registry.terraform.io/)
- [Style and validation: `terraform validate`](https://developer.hashicorp.com/terraform/cli/commands/validate)
- [`for_each` and `dynamic` blocks](https://developer.hashicorp.com/terraform/language/meta-arguments/for_each)
- [Terraform style guide](https://developer.hashicorp.com/terraform/language/style)
- [Terraform MCP server](https://github.com/hashicorp/terraform-mcp-server)
