# 🍔 Saí Burguer — Sistema de Gestão Multi-Agente

> Repositório central do sistema de gestão operacional, financeiro e de estoque da **Saí Burguer** — hamburgeria artesanal de Boa Vista/RR, gerida por Eden Gonçalves.

---

## Visão Geral: O Megazord

O sistema opera com dois agentes de IA em colaboração permanente:

| Agente | Papel | Canal |
|---|---|---|
| **Manus** | Operação cotidiana, integrações em tempo real, análise e relatórios | Telegram + Plataforma Manus |
| **Claude Code** | Engenharia, automações técnicas, desenvolvimento local | Terminal + IDE |

O Eden interage exclusivamente com o **Manus** (via Telegram). O Manus orquestra e, quando necessário, documenta tarefas para execução pelo Claude Code. O **Notion** é o cérebro compartilhado — ambos os agentes leem e escrevem lá.

```
Eden (Telegram)
      │
      ▼
   MANUS ──────────────────────────────────────────────────────────┐
      │                                                             │
      ├── Google Sheets (motor financeiro)                         │
      ├── Notion (base de dados estruturada)                       │
      └── Anota AI (vendas / PDV)                                  │
                                                                    │
                                                             CLAUDE CODE
                                                                    │
                                                    ├── NF-e Sefaz (importação)
                                                    ├── Sabrina (atendente IA)
                                                    ├── Painel Web (cozinhasai)
                                                    └── RH / Jurídico (Sólides)
```

---

## Estado Atual do Sistema

### ✅ Operacional
- **Planilha Mestre** (Google Sheets): 8 abas — Insumos (453 itens), Fichas Técnicas, Lançamentos, Vendas, Entradas de Estoque, Saídas de Estoque, Fluxo de Caixa, Dashboard
- **Notion**: Fichas Técnicas (46 produtos), Checklist de Estoque, Cadastro de Fornecedores, Estoque, Movimentações
- **Extrato Nubank** (Fev/2026): 305 lançamentos importados e classificados por plano de contas DRE
- **Baixa de estoque por vendas**: cruzamento automático Anota AI × Fichas Técnicas
- **Importação de NF-e direto da Sefaz** (Claude Code): pipeline Python completo, operacional
- **Bridge GitHub** (`/bridge`): canal assíncrono Manus ↔ Claude Code via `queue.json` / `responses.json`

### 🔄 Em Desenvolvimento (Claude Code)
- Sabrina — atendente IA da Saí Burguer
- Painel web de gestão (cozinhasai)

### ⏳ Pendente
- Integração Sólides (RH)
- Preenchimento do Cadastro de Fornecedores

---

## Estrutura do Repositório

```
/docs               → Documentação de arquitetura, protocolos e processos
/data               → Dados exportados (fichas técnicas, relatórios)
/integracoes        → Scripts e configs de integração por sistema
/agentes            → Código e prompts dos agentes (Manus e Sabrina)
/bridge             → Canal de comunicação Manus ↔ Claude Code (queue.json / responses.json)
```

---

## Links Importantes

- **Planilha Mestre**: [Google Sheets](https://docs.google.com/spreadsheets/d/1LlmzHtiXez4hRyuZHUHDyyoKYmYLvtXZTTPyx4iqPMY/edit)
- **Notion — Saí Burguer Gestão**: [Notion](https://www.notion.so/313f5c09515a81f68f90fd397fa4bec5)
- **Painel Web**: [cozinhasai.manus.space](https://cozinhasai-ai7i9kdk.manus.space)

---

*Última atualização: 2026-03-18*
