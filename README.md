# Terraform Basics

> Learn Terraform by understanding what it is, practicing what it does, and building confidence one small step at a time.

Terraform Basics is a practical, guided course for people who are new to Terraform, moving from manual cloud consoles or provisioning scripts, or looking for a clearer mental model of everyday infrastructure as code.

[Start the course](menu.md) | [Choose your language](#languages) | [Contribute](CONTRIBUTING.md)

## Why this course exists

Terraform documentation can be technically accurate and still feel difficult to enter. This project turns the essential ideas into a guided path: short explanations, real commands, visible results, and exercises that can be practiced in a temporary working directory.

The goal is not to memorize a list of commands. The goal is to understand the state of your infrastructure, make intentional changes, and recover calmly when something goes wrong.

## What you will learn

- How infrastructure as code protects and explains the history of an environment.
- How Terraform's configuration, providers, resources, state, and modules fit together.
- How to install and configure Terraform for personal or team projects.
- How to review a plan before applying it.
- How to organize modules, store state remotely, and collaborate safely.
- How to choose the right recovery command for an unwanted change.

## Course map

| Chapter                                                                                    | Focus                                              | You will practice                                                           |
| ------------------------------------------------------------------------------------------ | -------------------------------------------------- | -------------------------------------------------------------------------- |
| [1. Basic concepts](docs/chapters/01-basic-concepts/README.md)                             | The ideas behind infrastructure as code and Terraform | Thinking in desired state, plans, and resource graphs                    |
| [2. Installation and configuration](docs/chapters/02-installation-configuration/README.md) | Getting Terraform ready to use                     | Checking the installation, providers, credentials, and backends            |
| [3. Commands and operations](docs/chapters/03-commands-operations/README.md)               | Building a dependable daily workflow               | Init, plan, apply, state, modules, remote state, and recovery              |
| [4. Platform integration](docs/chapters/04-platform-integration/README.md)                 | Running Terraform on hosted and CI/CD platforms    | Remote runs, pipelines, permissions, secrets, and secure delivery         |
| [5. IDE and editor integration](docs/chapters/05-ide-integration/README.md)                | Using Terraform through code editors and IDEs      | Authoring, validation, formatting, navigation, and tool configuration     |

## A quick first practice

Once Terraform is installed, create a disposable practice directory:

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

You have just created a configuration, initialized the working directory, previewed the change, applied it, and removed it again. Chapter 1 explains what happened at each stage.

## How to use the documentation

1. Begin with the [documentation menu](menu.md).
2. Read Chapter 1 before diving into command memorization.
3. Complete the setup steps in Chapter 2.
4. Work through Chapter 3 in a disposable working directory.
5. Explore Chapter 4 for the platform used by your team.
6. Read Chapter 5 for your code editor or IDE.
7. Use the [glossary](docs/glossary.md) whenever a term is unfamiliar.

Every lesson is a standalone Markdown file, linked with relative paths so it can be read directly on GitHub.

## Languages

The course is available in four languages:

- [English](menu.md)
- [Français](docs/i18n/fr/README.md)
- [Português (Brasil)](docs/i18n/pt-br/README.md)
- [Español](docs/i18n/es/README.md)

## Project values

- **Practical:** examples should lead to something the learner can observe.
- **Approachable:** explain the idea before introducing the command.
- **Safe:** use disposable working directories and make destructive operations explicit.
- **Open:** keep the documentation free, reusable, and easy to improve.

## Contributing

Found a confusing explanation, a missing exercise, or a broken link? Read the [contribution guide](CONTRIBUTING.md) and help make the next learner's first Terraform experience better.

## Origin

This course grew out of a DevSecOps experience supporting teams that were moving from manual cloud changes to infrastructure as code. Official documentation and reference sites were useful, but some learners needed a more guided and practical route into the subject. Terraform Basics was created to provide that route and to make the learning process easier to share.

The project is intentionally collaborative. Feedback, corrections, examples, and translations are welcome.
