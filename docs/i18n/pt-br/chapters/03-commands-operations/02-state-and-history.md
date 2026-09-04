# 3.2 State e Histórico de Mudanças

O state é como o Terraform lembra o que gerencia. Ele mapeia cada bloco de resource para um objeto real e armazena os últimos atributos conhecidos desse objeto.

## Inspecione o state

```bash
terraform state list
terraform show
terraform state show <resource address>
```

O `state list` nomeia cada resource rastreado, o `show` imprime todos os valores registrados e o `state show` foca em um único. O `state list` aceita um padrão de endereço de resource para filtrar, o que importa quando um state tem milhares de resources espalhados por muitos módulos — por exemplo, `terraform state list module.notes` lista apenas os resources desse módulo.

## Inspecionando um plano salvo da mesma forma

O `terraform show` não se limita ao state — apontado para um arquivo salvo com `plan -out`, ele o renderiza no mesmo formato legível para humanos, e o `terraform show -json` produz uma versão legível por máquina do state ou de um arquivo de plano, para uso por ferramentas externas. Como o `-json` imprime valores sensíveis em texto puro, trate essa saída com o mesmo cuidado que o próprio arquivo de state.

## Avançado: lendo e escrevendo o arquivo de state bruto

- O `terraform state pull` imprime o state atual como JSON bruto na saída padrão — útil para scripts ou para arquivar um snapshot, mas é somente leitura e seguro.
- O `terraform state push` envia um arquivo de state local para o backend configurado, sobrescrevendo o que estiver lá. **Isso é destrutivo**: substitui todo o state remoto desta configuração, não apenas um resource, e uma divergência aceita silenciosamente pode corromper a próxima execução de um colega. Por padrão, o Terraform recusa o push se o destino tiver um lineage diferente ou um número de série mais novo; passe `-force` para ignorar essa verificação apenas se você tiver certeza de que a cópia de destino é a que deve ser descartada, e mantenha uma cópia do arquivo que está prestes a sobrescrever (ou o state atual do destino, obtido antes com `pull`) para poder desfazer o push.

Use `state pull`/`state push` apenas quando não houver outra forma de corrigir o state (por exemplo, consertar manualmente um JSON corrompido offline); para mudanças do dia a dia, prefira `terraform state list` / `show` / `mv` / `rm` e `import`, já que cada um deles mexe em apenas um resource por vez e é revisável.

## Onde o histórico vive

O Terraform não mantém um log completo de mudanças próprio. O histórico revisável é o seu histórico de controle de versão dos arquivos `.tf`, mais o histórico de execuções da plataforma que os aplica. Faça commit das mudanças de configuração em passos pequenos e descritos para que o "porquê" seja recuperável.

## Prática

Aplique uma configuração pequena, execute `terraform state list`, depois altere um atributo e execute `terraform plan`. Observe como o plano explica a diferença entre o state registrado e o novo estado desejado.

## Referências

- [Manipulating state](https://developer.hashicorp.com/terraform/cli/state)
- [Command: state list](https://developer.hashicorp.com/terraform/cli/commands/state/list)
- [Command: show](https://developer.hashicorp.com/terraform/cli/commands/show)
- [Inspecting state](https://developer.hashicorp.com/terraform/cli/state/inspect)
- [Command: state pull](https://developer.hashicorp.com/terraform/cli/commands/state/pull)
- [Command: state push](https://developer.hashicorp.com/terraform/cli/commands/state/push)
