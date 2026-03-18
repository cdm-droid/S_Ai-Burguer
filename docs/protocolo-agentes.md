# 🤝 Protocolo de Agentes — Manus + Claude Code

## Visão Geral

A Saí Burguer opera com dois agentes de IA em colaboração:

- **Manus** — ponto de entrada único, operação cotidiana, integrações em tempo real
- **Claude Code** — engenharia, automações técnicas, desenvolvimento local

O Eden interage exclusivamente com o Manus (via Telegram ou diretamente na plataforma). O Manus orquestra e, quando necessário, aciona o Claude via API ou documenta tarefas para execução local.

O Notion é o cérebro compartilhado: ambos os agentes leem e escrevem aqui.

---

## Responsabilidades do Manus

### Operação Cotidiana (tempo real via Telegram)

- Recebimento e processamento de comprovantes de despesa (OCR)
- Lançamento financeiro no Google Sheets e Notion
- Controle de estoque: entradas (compras), saídas (vendas), conferência (contagem)
- Processamento de relatórios de vendas do Anota AI
- Consultas rápidas: saldo, estoque, indicadores
- Alertas automáticos: estoque crítico, metas, anomalias

### Análise e Relatórios

- Relatório diário de vendas e consumo de insumos
- Fechamento semanal: conciliação de extrato bancário
- Fechamento mensal: DRE simplificado, CMV, impacto de taxas
- Análise de desempenho: Instagram, Meta Ads, Google Meu Negócio

### Gestão de Dados

- Atualização de fichas técnicas no Notion
- Manutenção do cadastro de fornecedores
- Registro de movimentações de estoque

---

## Responsabilidades do Claude Code

### Engenharia e Automações Técnicas

- Aplicação de importação de NF-e direto da Sefaz
- Automações locais e scripts de processamento
- Integrações técnicas com APIs externas (Sólides, etc.)

### Desenvolvimento de Produtos

- Sabrina — atendente IA da Saí Burguer (desenvolvimento e treinamento)
- Painel web de gestão (cozinhasai)

### RH e Jurídico

- Cálculo de jornada 4x3 e folha de pagamento estimada
- Contratos de trabalho por cargo
- Gestão de informações de pessoal no Sólides

### Arquitetura

- Manutenção e evolução da estrutura do Notion
- Auditoria periódica de dados e consistência

---

## Protocolo de Handoff

Quando o Manus identifica uma tarefa de competência do Claude Code:

1. Documenta a tarefa na database **📥 Fila para Claude Code** (Notion)
2. Inclui: contexto completo, dados relevantes, resultado esperado
3. Notifica o Eden pelo Telegram

Quando o Claude Code conclui uma tarefa:

1. Documenta o resultado no Notion
2. Atualiza o status na fila (Pendente → Concluído)
3. O Manus incorpora o resultado na operação

---

## Frentes de Marketing (Manus)

- **Instagram:** análise de engajamento, alcance, crescimento de seguidores
- **Meta Ads:** performance de campanhas, CPC, ROAS, sugestões de otimização
- **Google Meu Negócio:** avaliações, buscas, visualizações de perfil

---

## Regras Gerais

1. O Eden nunca precisa saber qual agente está executando — a interface é sempre o Manus.
2. Dados sensíveis (senhas, tokens) nunca são armazenados no Notion ou no repositório.
3. Toda tarefa de engenharia que exige execução local vai para a fila do Claude Code.
4. O Manus documenta tudo no Notion; o Claude Code lê o Notion para contexto.
