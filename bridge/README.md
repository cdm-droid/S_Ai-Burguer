# Bridge — Canal de Comunicação Manus ↔ Claude Code

Este diretório é o canal assíncrono de comunicação entre os dois agentes.
Funciona via Git: qualquer agente que escrever faz commit + push; qualquer agente que ler faz pull primeiro.

---

## Por que Git e não Notion?

O Git funciona independente de a máquina do Eden estar ligada ou não. O Manus pode enfileirar tarefas a qualquer hora. Quando Eden ligar o computador e abrir sessão com o Claude Code, ele faz `git pull`, lê a fila e processa tudo.

---

## Arquivos

| Arquivo | Quem escreve | Quem lê |
|---------|-------------|---------|
| `queue.json` | **Manus** | Claude Code |
| `responses.json` | **Claude Code** | Manus |

---

## Como o Manus enfileira uma tarefa

1. Fazer `git pull` para ter a versão mais recente
2. Abrir `queue.json` e adicionar um item no array `tarefas`:

```json
{
  "id": "TASK-001",
  "de": "manus",
  "para": "claude",
  "criado_em": "2026-03-18T14:30:00-04:00",
  "prioridade": "normal",
  "tipo": "automacao",
  "titulo": "Rodar pipeline NF-e",
  "contexto": "Há NF-es novas desde ontem. Último NSU conhecido: 550.",
  "dados": {
    "ultimo_nsu": 550
  },
  "resultado_esperado": "NF-es lançadas no Notion financeiro e estoque. E-mail de resumo enviado.",
  "status": "pendente"
}
```

3. Commit: `git commit -m "task: TASK-001 — Rodar pipeline NF-e"`
4. Push: `git push`

---

## Como o Claude Code responde

Após processar, Claude atualiza `status` em `queue.json` para `"concluido"` e adiciona item em `responses.json`:

```json
{
  "task_id": "TASK-001",
  "respondido_em": "2026-03-18T16:00:00-04:00",
  "status": "concluido",
  "resumo": "12 NF-es processadas, 0 duplicatas, 47 itens lançados no estoque.",
  "detalhes": "NSUs processados: 551–562. Último NSU salvo: 562. E-mail enviado para cdm@cardumecozinha.com.",
  "proximos_passos": null
}
```

Commit + push com mensagem: `git commit -m "response: TASK-001 — concluido"`

---

## Tipos de tarefa reconhecidos pelo Claude Code

| Tipo | Exemplos |
|------|---------|
| `automacao` | Rodar pipeline NF-e, conciliação financeira |
| `engenharia` | Criar/corrigir script, ajustar estrutura |
| `analise` | Gerar relatório, auditar dados no Notion |
| `correcao` | Corrigir lançamento errado, ajustar IDs |
| `outro` | Qualquer coisa fora das categorias acima |

---

## Prioridades

| Prioridade | Quando usar |
|-----------|-------------|
| `alta` | Erro em produção, dado crítico incorreto |
| `normal` | Tarefa de rotina, prazo de horas |
| `baixa` | Melhoria, não urgente |

---

*Protocolo estabelecido em 2026-03-18.*
