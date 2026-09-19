# Projeto Dashboard Setup — regras permanentes

Estas regras são obrigatórias em qualquer alteração futura do dashboard, especialmente quando o usuário fornecer um novo layout HTML.

## Regra principal

Alterar o layout nunca autoriza alterar, apagar ou voltar para uma estrutura antiga de dados. Preserve a estrutura atualmente validada no `index.html` até o usuário fornecer explicitamente uma nova BASE_RESUMO.

## Fonte estrutural vigente

A fonte aprovada é `BASE_RESUMO(7).xlsx`, planilha `Base_resumo`.

Campos utilizados:

- `Prefixos_Setup`: identificador da equipe
- `STATUS`: situação de mobilização
- `Coordenador`
- `Supervisor`
- `Fiscal`
- `Processo`
- `Turno`
- `META PREMIAÇÃO`: meta mensal oficial
- `DIA TRAB CT`: divisor da meta diária

## Condições obrigatórias

1. Incluir somente equipes cujo STATUS seja `MOBILIZADA`.
2. Excluir integralmente equipes `DESMOBILIZADA` de filtros, rankings, totais, metas e indicadores.
3. Usar as lideranças exatamente como estão na `Base_resumo`.
4. Meta mensal da equipe = `META PREMIAÇÃO`.
5. Meta diária da equipe = `META PREMIAÇÃO / DIA TRAB CT`, quando o divisor for maior que zero.
6. Normalizar chaves de equipe apenas para comparação (espaços e caixa), preservando acentos e nomes na exibição.
7. Um novo HTML enviado pelo usuário deve ser tratado somente como novo layout/funcionalidade. Reaplicar nele estas regras e os dados estruturais vigentes.
8. Só substituir a estrutura quando o usuário fornecer explicitamente uma nova versão da BASE_RESUMO.

## Totais de controle da versão vigente

- 372 registros estruturais
- 347 equipes mobilizadas incluídas
- 25 equipes desmobilizadas excluídas
- Soma da META PREMIAÇÃO mensal: R$ 12.148.838,24
- Soma da meta diária: R$ 474.448,98

Qualquer alteração que não produza esses totais deve ser interrompida e investigada, salvo quando houver uma nova base explicitamente aprovada pelo usuário.

## Arquitetura de publicação

- Publicar como um único `index.html`.
- Não usar `_dash/part*.txt`, loaders divididos, `atob`, base64, gzip ou `DecompressionStream`.
- Manter uma cópia de faturamento embutida como fallback para que o dashboard abra mesmo se a consulta remota falhar.
- A base de faturamento pode ser atualizada pelo fluxo aprovado do painel/Supabase; isso não altera a estrutura da BASE_RESUMO.
- Projeto Supabase oficial: `mcjlknvrqgmveixyqkyo` (`Dashboard Setup`).
- A tabela `dashboard_faturamento` é singleton: deve existir somente o registro `id = 1`.
- A cada nova publicação, sobrescrever o registro `id = 1` com todos os dados da nova planilha.
- A chave pública possui apenas SELECT e UPDATE; INSERT e DELETE permanecem bloqueados para impedir versões extras.
- Não criar outro registro como alternativa quando uma atualização falhar.
- Antes de publicar, validar sintaxe, abertura da página, filtros, totais e funcionamento em tela móvel.
