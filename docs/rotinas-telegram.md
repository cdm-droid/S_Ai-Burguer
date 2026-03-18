# Rotinas Operacionais via Telegram — Saí Burguer

O Telegram é o canal único de interação do Eden com o Manus. As rotinas abaixo definem o fluxo completo de operação cotidiana, com exemplos reais de interação.

---

## Rotinas Diárias

### 1. Lançamento de Compra com OCR

**Quando:** A cada compra de insumos ou despesa operacional.

**O que Eden faz:** Envia foto do recibo, comprovante de Pix ou nota fiscal.

**O que o Manus faz automaticamente:**
1. Extrai por OCR: valor, data, fornecedor e itens (quando discriminados)
2. Se os itens estão discriminados na nota: lista os itens e pede confirmação
3. Se não estão discriminados: pergunta "Quais insumos e quantidades entraram nessa compra?"
4. Após confirmação: registra a despesa em Lançamentos (Sheets) + entrada dos insumos em Entradas de Estoque (Sheets) + atualiza Nível Atual no Notion
5. Confirma tudo ao Eden com resumo

**Exemplo de interação:**
```
Eden: [foto do comprovante de Pix - R$ 223,50 - Carnes & Cortes]

Manus: Comprovante recebido! Identifiquei:
  📅 Data: 26/02/2026
  💰 Valor: R$ 223,50
  🏪 Fornecedor: Carnes & Cortes Ltda
  💳 Forma: Pix

Quais insumos e quantidades entraram nessa compra?

Eden: Blend 120g: 30 unidades, Blend 150g: 24 unidades

Manus: ✅ Registrado!
  LANC-001 | Carnes & Cortes | R$ 223,50 | Pix
  ENT-001 | Blend 120g | 30 un | R$ 3,73/un
  ENT-002 | Blend 150g | 24 un | R$ 4,66/un
  Estoque atualizado no Notion.
```

---

### 2. Baixa de Estoque por Vendas

**Quando:** Ao final de cada dia de operação.

**O que Eden faz:** Envia o relatório de itens vendidos do Anota AI (CSV ou foto).

**O que o Manus faz automaticamente:**
1. Processa os itens vendidos e quantidades
2. Cruza cada item com a ficha técnica correspondente
3. Calcula o consumo de cada insumo
4. Registra as saídas em Saídas de Estoque (Sheets)
5. Subtrai do Nível Atual no Checklist (Notion)
6. Envia resumo com alertas de estoque baixo/crítico

**Exemplo de interação:**
```
Eden: [arquivo vendas_26_02.csv]

Manus: Vendas do dia 26/02 processadas! 🍔

  💰 Faturamento: R$ 1.892,50
  📦 Itens vendidos: 91
  🎯 Ticket médio: R$ 20,80

  Top consumo de insumos:
  • Pão Brioche: 47 un
  • Blend 120g: 32 un
  • Queijo Prato: 35 fatias
  • Bacon: 1,8 kg
  • Maionese: 18 porções

  ⚠️ Alerta: Pão Brioche abaixo do mínimo (3 un restantes)
  ✅ Estoque atualizado no Notion.
```

---

### 3. Contagem Física de Estoque

**Quando:** Diariamente, ao final do expediente ou no início do turno.

**O que Eden faz:** Envia as quantidades contadas dos insumos sensíveis.

**O que o Manus faz automaticamente:**
1. Compara com o estoque teórico (calculado pelas entradas e saídas)
2. Calcula o desvio (perda/quebra)
3. Atualiza o Nível Atual no Notion com o valor real
4. Registra o desvio como "Ajuste" em Saídas de Estoque
5. Alerta se o desvio for relevante

**Exemplo de interação:**
```
Eden: Contagem de hoje:
  Pão Brioche: 8
  Blend 120g: 12
  Blend 150g: 9
  Bacon: 2,1 kg
  Queijo Prato: 18 fatias

Manus: Contagem registrada! 📋

  Desvios identificados:
  • Pão Brioche: teórico 10, real 8 → perda de 2 un
  • Blend 120g: teórico 12, real 12 → ✅ OK
  • Blend 150g: teórico 10, real 9 → perda de 1 un
  • Bacon: teórico 2,3 kg, real 2,1 kg → perda de 0,2 kg
  • Queijo Prato: teórico 18, real 18 → ✅ OK

  ⚠️ Pão Brioche abaixo do mínimo (8 un / mínimo 10 un)
  Estoque atualizado no Notion.
```

---

## Rotinas Semanais

### 4. Conciliação do Extrato Bancário

**Quando:** Toda segunda-feira, referente à semana anterior.

**O que Eden faz:** Envia o extrato bancário do Nubank em CSV.

**O que o Manus faz automaticamente:**
1. Classifica todas as transações por categoria DRE
2. Importa em Lançamentos (Sheets)
3. Identifica transações não classificadas ("A Classificar")
4. Atualiza Dashboard e Fluxo de Caixa
5. Envia relatório resumido com breakdown por categoria

---

### 5. Alerta de Estoque Semanal

**Quando:** Toda sexta-feira.

**O que o Manus faz automaticamente:**
1. Verifica todos os itens do Checklist de Estoque no Notion
2. Lista os itens abaixo do mínimo ou em nível crítico
3. Sugere lista de compras para a semana seguinte
4. Envia alerta ao Eden pelo Telegram

---

## Rotinas Mensais

### 6. Fechamento Mensal — DRE Simplificado

**Quando:** Todo dia 1º do mês seguinte.

**O que o Manus faz automaticamente:**
1. Consolida todas as receitas e despesas do mês
2. Calcula CMV (Custo da Mercadoria Vendida) médio
3. Calcula impacto das taxas de pagamento (iFood, Stone, Pix)
4. Gera DRE simplificado por categoria
5. Compara com o mês anterior
6. Envia relatório ao Eden

**Estrutura do DRE mensal:**
```
RECEITA BRUTA
(-) Deduções (DAS)
= RECEITA LÍQUIDA
(-) CPV (Insumos + Embalagens + Delivery)
= LUCRO BRUTO
(-) Despesas Operacionais
= EBITDA
(-) Despesas Financeiras (taxas)
= RESULTADO LÍQUIDO
```

---

## Consultas Rápidas (qualquer momento)

Eden pode perguntar ao Manus a qualquer momento:

- `"Qual o saldo de hoje?"` → Manus consulta Sheets e responde
- `"Quanto gastei com fornecedores esse mês?"` → Manus filtra Lançamentos e responde
- `"Tem algum insumo crítico?"` → Manus consulta Notion e responde
- `"Qual o CMV do mês?"` → Manus calcula e responde
- `"Quantos pedidos ontem?"` → Manus consulta Vendas e responde

---

*Última revisão: Março/2026*
