# 1.1 Infraestrutura como código

Infraestrutura como código (IaC) usa um modelo descritivo e versionado para definir e implantar infraestrutura — redes, máquinas virtuais, balanceadores de carga, bancos de dados e a topologia que os conecta — em vez de processos manuais e cliques no console. Assim como o mesmo código-fonte sempre gera o mesmo binário, uma configuração de IaC gera o mesmo ambiente toda vez que é implantada.

## O problema que ela resolve

Sem infraestrutura como código, a configuração real vive apenas em um console e na cabeça de poucas pessoas. Cada ambiente vai divergindo até virar um "floco de neve" único e sem documentação, impossível de reproduzir automaticamente, e reconstruí-lo ou auditá-lo vira um chute. O Terraform e ferramentas semelhantes mantêm a configuração pretendida em arquivos estruturados, então as mudanças de infraestrutura passam pela mesma revisão e pelo mesmo histórico de controle de versão que o código da aplicação.

## Exemplos práticos

- **Duplicação multi-região ou multi-conta.** Aplique a mesma configuração para levantar um ambiente idêntico em outra região ou conta de nuvem, em vez de repetir a configuração manual à mão.
- **Ambientes de teste e revisão efêmeros.** Provisione uma stack completa para um pull request ou um teste de carga, e destrua-a quando o trabalho terminar, para que os ambientes deixem de ser escassos, compartilhados e desatualizados.
- **Recuperação de desastres.** Reconstrua a topologia de produção a partir da configuração em uma nova região depois de uma interrupção, em vez de reconstruí-la de memória e de chamados de suporte.
- **Landing zones padronizadas.** Ofereça a cada equipe a mesma rede base, registro (logging) e controle de acesso reutilizando um módulo, em vez de integrar cada equipe manualmente.

## Benefícios

- **Consistência.** A mesma configuração sempre gera o mesmo ambiente, o que elimina divergência de configuração (drift) e surpresas do tipo "funciona no meu ambiente".
- **Idempotência.** Aplicar uma configuração repetidamente converge o ambiente para o mesmo estado, independentemente de o destino começar vazio ou parcialmente construído.
- **Velocidade em escala.** Ambientes que antes levavam dias de trabalho manual podem ser provisionados, duplicados ou destruídos em minutos.
- **Revisão e reversão.** As mudanças de infraestrutura passam pelo mesmo pull request, diff e histórico de versões que o código da aplicação, então uma mudança ruim pode ser revisada antes de ir para produção e revertida depois.
- **Uma linguagem compartilhada entre equipes.** Desenvolvedores e operadores leem os mesmos arquivos, o que reduz as transferências e os erros de tradução do provisionamento por chamado.

## Desafios

- **Uma curva de aprendizado.** As equipes precisam aprender uma linguagem de configuração, um modelo de state e o schema de recursos de um provider antes de serem produtivas.
- **Gestão de state e drift.** Se alguém muda um recurso fora da ferramenta, por exemplo direto no console, o state registrado e a realidade ficam divergentes até serem reconciliados.
- **Raio de impacto (blast radius).** Um único `apply` errado pode mudar ou destruir muitos recursos de uma vez; as salvaguardas do Capítulo 3, como revisar um plano antes, existem por causa desse risco.
- **Segredos e dados sensíveis.** A configuração e o state podem acabar guardando credenciais ou outros valores sensíveis se não forem tratados deliberadamente.
- **Testar código de infraestrutura.** Validar uma configuração a fundo normalmente significa provisioná-la de verdade em algum lugar, o que custa tempo e dinheiro que testar código de aplicação não custa.

## Prática

Anote cada passo manual que você daria para criar um recurso pequeno no console da sua nuvem. Essa lista é o valor que a infraestrutura como código oferece: ela transforma esses passos em um arquivo que você pode revisar, repetir e reverter. Depois, escolha um item da lista de desafios acima e anote como o processo da sua equipe precisaria mudar para lidar com ele.

## Referências

- [What is infrastructure as code? (Microsoft Learn, Azure DevOps)](https://learn.microsoft.com/en-us/devops/deliver/what-is-infrastructure-as-code)
- [What is infrastructure as code? (AWS)](https://aws.amazon.com/what-is/iac/)
- [What is Terraform? (HashiCorp)](https://developer.hashicorp.com/terraform/intro)
- [Terraform use cases](https://developer.hashicorp.com/terraform/intro/use-cases)
