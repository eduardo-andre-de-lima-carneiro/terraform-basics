# Fundamentos do Terraform

> Aprenda Terraform entendendo o que ele é, praticando o que ele faz e ganhando confiança um pequeno passo de cada vez.

Fundamentos do Terraform é um curso prático e guiado para quem está começando com Terraform, migrando de consoles de nuvem manuais ou scripts de provisionamento, ou buscando um modelo mental mais claro para o dia a dia de infraestrutura como código.

[Comece o curso](menu.md) | [Escolha seu idioma](#idiomas) | [Contribua](../../../CONTRIBUTING.md)

## Por que este curso existe

A documentação do Terraform pode ser tecnicamente precisa e, ainda assim, parecer difícil de acessar. Este projeto transforma as ideias essenciais em um caminho guiado: explicações curtas, comandos reais, resultados visíveis e exercícios que podem ser praticados em um diretório de trabalho temporário.

O objetivo não é memorizar uma lista de comandos. O objetivo é entender o estado da sua infraestrutura, fazer mudanças intencionais e recuperar a calma quando algo dá errado.

## O que você vai aprender

- Como a infraestrutura como código protege e explica o histórico de um ambiente.
- Como a configuração, os providers, os resources, o state e os módulos do Terraform se encaixam.
- Como instalar e configurar o Terraform para projetos pessoais ou de equipe.
- Como revisar um plano antes de aplicá-lo.
- Como organizar módulos, armazenar o state remotamente e colaborar com segurança.
- Como escolher o comando de recuperação certo para uma mudança indesejada.

## Mapa do curso

| Capítulo                                                                                    | Foco                                              | Você vai praticar                                                         |
| ------------------------------------------------------------------------------------------ | -------------------------------------------------- | -------------------------------------------------------------------------- |
| [1. Conceitos básicos](chapters/01-basic-concepts/README.md)                               | As ideias por trás da infraestrutura como código e do Terraform | Pensar em estado desejado, planos e grafos de resources                   |
| [2. Instalação e configuração](chapters/02-installation-configuration/README.md)           | Deixar o Terraform pronto para uso                | Verificar a instalação, providers, credenciais e backends                  |
| [3. Comandos e operações](chapters/03-commands-operations/README.md)                       | Construir um fluxo de trabalho diário confiável   | Init, plan, apply, state, módulos, state remoto e recuperação             |
| [4. Integração com plataformas](chapters/04-platform-integration/README.md)               | Executar o Terraform em plataformas hospedadas e de CI/CD | Execuções remotas, pipelines, permissões, segredos e entrega segura      |
| [5. Integração com IDE e editores](chapters/05-ide-integration/README.md)                 | Usar o Terraform por meio de editores de código e IDEs | Escrita, validação, formatação, navegação e configuração de ferramentas  |

## Uma primeira prática rápida

Assim que o Terraform estiver instalado, crie um diretório de prática descartável:

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

Você acabou de criar uma configuração, inicializar o diretório de trabalho, pré-visualizar a mudança, aplicá-la e removê-la novamente. O Capítulo 1 explica o que aconteceu em cada etapa.

## Como usar a documentação

1. Comece pelo [menu da documentação](menu.md).
2. Leia o Capítulo 1 antes de partir para a memorização de comandos.
3. Conclua as etapas de configuração do Capítulo 2.
4. Percorra o Capítulo 3 em um diretório de trabalho descartável.
5. Explore o Capítulo 4 para a plataforma usada pela sua equipe.
6. Leia o Capítulo 5 para o seu editor de código ou IDE.
7. Use o [glossário](glossary.md) sempre que um termo for desconhecido.

Cada lição é um arquivo Markdown independente, ligado por caminhos relativos para que possa ser lido diretamente no GitHub.

## Idiomas

O curso está disponível em quatro idiomas:

- [English](../../../README.md)
- [Français](../fr/README.md)
- [Português (Brasil)](README.md)
- [Español](../es/README.md)

## Valores do projeto

- **Prático:** os exemplos devem levar a algo que o aluno possa observar.
- **Acessível:** explicar a ideia antes de introduzir o comando.
- **Seguro:** usar diretórios de trabalho descartáveis e tornar explícitas as operações destrutivas.
- **Aberto:** manter a documentação gratuita, reutilizável e fácil de melhorar.

## Contribuindo

Encontrou uma explicação confusa, um exercício ausente ou um link quebrado? Leia o [guia de contribuição](../../../CONTRIBUTING.md) e ajude a tornar melhor a primeira experiência com Terraform do próximo aluno.

## Origem

Este curso nasceu de uma experiência de DevSecOps apoiando equipes que estavam migrando de mudanças manuais na nuvem para infraestrutura como código. A documentação oficial e os sites de referência eram úteis, mas alguns alunos precisavam de um caminho mais guiado e prático para o assunto. O Fundamentos do Terraform foi criado para oferecer esse caminho e para tornar o processo de aprendizado mais fácil de compartilhar.

O projeto é intencionalmente colaborativo. Feedback, correções, exemplos e traduções são bem-vindos.
