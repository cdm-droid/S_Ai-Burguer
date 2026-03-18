# Arquitetura do Sistema — Saí Burguer

## Visão Geral

O sistema de gestão da Saí Burguer é uma arquitetura multi-agente distribuída, onde cada componente tem uma responsabilidade clara e os dados fluem de forma estruturada entre as ferramentas.

## Componentes

### Camada de Entrada
- **Telegram**: canal principal de interação do Eden com o Manus. Recebe fotos, CSVs, mensagens de texto e comandos.
- **Anota AI**: sistema de PDV (ponto de venda) e cardápio digital. Exporta relatórios de vendas em CSV.
- **Nubank**: extrato bancário exportado em CSV para conciliação financeira.

### Camada de Processamento (Agentes)
- **Manus**: agente de operação em tempo real. Recebe inputs do Eden, processa (OCR, cálculos, cruzamentos) e escreve nos bancos de dados.
- **Claude Code**: agente de engenharia. Desenvolve automações locais, integrações técnicas e produtos de IA.

### Camada de Dados
- **Google Sheets (Planilha Mestre)**: motor de cálculos financeiros. Contém 8 abas: Insumos, Fichas Técnicas, Lançamentos, Vendas, Entradas de Estoque, Saídas de Estoque, Fluxo de Caixa e Dashboard.
- **Notion**: base de dados estruturada. Contém Fichas Técnicas, Checklist de Estoque, Cadastro de Fornecedores, Estoque e Movimentações.

## Fluxo de Dados

### Fluxo de Compra (Entrada de Estoque)
```
Eden envia foto do recibo/Pix
        │
        ▼
Manus (OCR) → extrai: valor, data, fornecedor, itens
        │
        ├── Lança em Lançamentos (Sheets) com ID sequencial
        ├── Registra em Entradas de Estoque (Sheets)
        └── Atualiza Nível Atual no Checklist (Notion)
```

### Fluxo de Vendas (Saída de Estoque)
```
Eden envia CSV de vendas do Anota AI
        │
        ▼
Manus processa itens vendidos
        │
        ├── Cruza com Fichas Técnicas → calcula consumo por insumo
        ├── Registra em Saídas de Estoque (Sheets)
        └── Subtrai do Nível Atual no Checklist (Notion)
```

### Fluxo de Conciliação Financeira
```
Eden envia extrato bancário (CSV Nubank)
        │
        ▼
Manus categoriza por plano de contas DRE
        │
        ├── Importa em Lançamentos (Sheets)
        └── Atualiza Dashboard e Fluxo de Caixa (Sheets)
```

## Tecnologias

| Componente | Tecnologia |
|---|---|
| Agente principal | Manus (plataforma) |
| Agente de engenharia | Claude Code (Anthropic) |
| Banco de dados estruturado | Notion (via MCP) |
| Motor financeiro | Google Sheets (via API v4) |
| PDV / Cardápio | Anota AI |
| Banco | Nubank (extrato CSV) |
| Pagamentos | Stone, iFood, Pix |
| RH | Sólides (planejado) |
| NF-e | Sefaz (em desenvolvimento) |

## Identificadores e Chaves

- **Lançamentos**: `LANC-XXX` (sequencial, gerado pelo Manus)
- **Entradas de Estoque**: `ENT-XXX` (sequencial, vinculado ao LANC)
- **Saídas de Estoque**: `SAI-XXX` (sequencial, vinculado ao produto vendido)
- **Fichas Técnicas**: nome do produto (chave natural)
- **Insumos**: nome padronizado (chave natural)
