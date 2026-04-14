# PROJECT RULES — Compilado Completo
## Regras Específicas do Projeto + Protocolo de Evolução (PEA)

> **Idioma obrigatório:** Pense e responda **sempre** em Português (pt-BR).
> **Status do sistema:** EM PRODUÇÃO. Alterações indevidas causam prejuízo real.
> **Você não chuta. Você lê, entende, discute, planeja, executa e documenta.**

---

## 📋 ÍNDICE

1. [Diretrizes Críticas do Projeto](#1-diretrizes-críticas-do-projeto-leitura-obrigatória)
2. [Tech Stack](#2-tech-stack--frameworks)
3. [Banco de Dados & Autenticação](#3-banco-de-dados--autenticação)
4. [Restrições de Código](#4-restrições-de-código)
5. [Enhancement Log — Documentação Viva](#5-enhancement-log--documentação-viva)
6. [Fase 0 — Leitura do Projeto](#6-fase-0--leitura-e-compreensão-do-projeto)
7. [Fase 1 — Brainstorm e Confirmação](#7-fase-1--brainstorm-e-confirmação-da-ideia)
8. [Fase 2 — Planejamento](#8-fase-2--planejamento)
9. [Fase 3 — Execução Controlada](#9-fase-3--execução-controlada)
10. [Fase 4 — Verificação e Qualidade](#10-fase-4--verificação-e-qualidade)
11. [Fase 5 — Documentação](#11-fase-5--documentação)
12. [Fase 6 — GitHub](#12-fase-6--github)
13. [Regras de Ouro](#13-regras-de-ouro)

---

## 1. DIRETRIZES CRÍTICAS DO PROJETO (LEITURA OBRIGATÓRIA)

> Estas regras têm **prioridade absoluta** sobre qualquer protocolo genérico.
> Em caso de conflito, as regras deste bloco prevalecem.

### 1.1 Sincronização de Contexto — Toda Nova Sessão

**OBRIGATÓRIO ao iniciar qualquer sessão ou chat:**

```
1. Leia `Docs/Atualizações.txt`       → histórico completo do que já foi feito
2. Leia `supabase_schema.sql`         → estado atual real do banco de dados
3. Leia `Docs/Schema atual.txt`       → referência de tabelas e campos
4. Se existir: leia `docs/ENHANCEMENT_LOG.md` → contexto da sessão anterior
```

> Sem esta leitura você não tem como saber o que já foi tentado, o que foi resolvido ou o que está em produção. **Não pule.**

---

### 1.2 Segurança de Produção

O sistema está **em funcionamento com usuários reais**. Qualquer erro causa prejuízo direto.

**ANTES de qualquer alteração significativa:**
- Leia `Docs/Atualizações.txt` e verifique se o problema já existiu antes e o que foi feito
- Analise o impacto sobre o que já está funcionando
- Identifique zonas de risco

**PROIBIÇÕES absolutas:**
- ❌ Alterar políticas, lógica de negócios ou fluxos existentes sem permissão explícita
- ❌ Decidir sozinho sobre qualquer mudança que quebre comportamento atual
- ❌ Fazer alterações em produção sem confirmação do usuário

**Protocolo obrigatório para alterações em algo existente:**
```
1. EXPLIQUE o risco da alteração
2. JUSTIFIQUE a necessidade da mudança
3. PERGUNTE explicitamente: "Posso prosseguir com essa alteração?"
4. Aguarde resposta antes de executar qualquer coisa
```

---

### 1.3 Workflow de GitHub e Documentação

**GitHub:**
- ❌ NUNCA suba atualizações sem comando explícito do usuário
- Aguarde: "pode subir", "faz o commit", "atualiza o GitHub" ou similar
- Commits escritos **sempre em Português**

**Documentação pós-deploy** (somente após atualização no GitHub):

Atualize `Docs/Atualizações.txt` seguindo **exatamente** este template:

```text
Atualizações
Data: [DD/MM/AAAA]
1. o que foi atualizado:
2. o que foi implementado:
Quais arquivos foram atualizados:
Quais arquivos foram removidos:
Quais arquivos foram adicionados:
```

> Este template é fixo. Não invente campos. Não altere a estrutura.

---

## 2. TECH STACK & FRAMEWORKS

> Stack **fixa**. Não sugira migrações fora do escopo da tarefa.

| Camada | Tecnologia | Versão |
|--------|-----------|--------|
| Frontend Core | Vite + React | React 19.2.0 |
| Componentes | Functional components + Hooks | — |
| Routing | React Router DOM | 7.9.6 |
| Gráficos | Recharts | 3.6.0 |
| Ícones | Lucide React | 0.555.0 |
| Backend/DB | Supabase (PostgreSQL) | supabase-js 2.86.0 |
| WhatsApp | Evolution API | — |
| Workflows | n8n | — |
| Linguagem | JavaScript (.jsx) | — |

**Restrição de linguagem:** Não migrar para TypeScript agora. Todos os componentes React em `.jsx`.

---

## 3. BANCO DE DADOS & AUTENTICAÇÃO

### 3.1 Tabelas Existentes (não criar sem aprovação)

```
users | financial | transactions | documents
```

Consulte `Docs/Schema atual.txt` antes de qualquer query complexa ou ao criar novas queries. Respeite rigorosamente a estrutura existente.

### 3.2 Autenticação

- Login via `access_token` na tabela `users`
- **NÃO criar login por senha** — esta decisão é permanente
- Usar sempre `lib/supabase.js` para conexão

### 3.3 Segurança de Dados (RLS)

- **Todas as queries** devem filtrar por `user_id` — sem exceção
- Nunca expor dados de um usuário para outro

### 3.4 Datas e Fusos Horários

- Usar `timestamp with time zone` em todos os campos de data
- No Dashboard, filtrar pelo fuso horário do usuário (Brasil / UTC-3)

---

## 4. RESTRIÇÕES DE CÓDIGO

- **Estado global:** Evite Redux. Use Context API ou Hooks simples (ex: `useDashboardData`)
- **Componentes:** Functional components e Hooks exclusivamente — sem class components
- **Queries:** Sempre via `lib/supabase.js`, nunca acesso direto
- **RLS:** Todo acesso a dados deve passar pelo filtro `user_id`

---

## 5. ENHANCEMENT LOG — DOCUMENTAÇÃO VIVA

> O `docs/ENHANCEMENT_LOG.md` é criado no início de cada sessão de melhoria, atualizado a cada Gate e lido por cada agente antes de iniciar qualquer trabalho.
> **Complementa** (não substitui) o `Docs/Atualizações.txt`, que é o histórico oficial de produção.

### Relação entre os documentos

| Documento | Propósito | Quando atualizar |
|-----------|-----------|-----------------|
| `Docs/Atualizações.txt` | Histórico oficial de produção | Somente após deploy no GitHub |
| `docs/ENHANCEMENT_LOG.md` | Memória de contexto da sessão atual | A cada Gate aprovado |
| `supabase_schema.sql` | Schema real do banco | Ao alterar estrutura do BD |
| `Docs/Schema atual.txt` | Referência de tabelas para queries | Ao alterar estrutura do BD |

### Criação do Enhancement Log (início da Fase 0)

```bash
Write docs/ENHANCEMENT_LOG.md
```

Estrutura inicial:

```markdown
# Enhancement Log
## Projeto: [Nome do projeto]
## Melhoria: [Título — preenchido no Gate 1]
## Iniciado em: [DD/MM/AAAA HH:mm]
## Status: 🔄 EM ANDAMENTO

> Lido por todo agente antes de iniciar trabalho. Atualizado em cada Gate.
> Última atualização: [DD/MM/AAAA HH:mm]

---

## ⏳ GATES

| Gate | Fase | Status | Aprovado em |
|------|------|--------|-------------|
| GATE 0 | Leitura e sincronização | ⏳ Pendente | — |
| GATE 1 | Brainstorm e escopo | ⏳ Pendente | — |
| GATE 2 | Plano aprovado | ⏳ Pendente | — |
| GATE 3 | Execução concluída | ⏳ Pendente | — |
| GATE 4 | Verificação aprovada | ⏳ Pendente | — |
| GATE 5 | Documentação completa | ⏳ Pendente | — |
| GATE 6 | GitHub atualizado | ⏳ Pendente | — |

---

<!-- GATE 0 -->
<!-- GATE 1 -->
<!-- GATE 2 -->
<!-- GATE 3 -->
<!-- GATE 4 -->
<!-- GATE 5 -->
<!-- GATE 6 -->
```

### Protocolo de Leitura Obrigatória para Agentes

Todo agente invocado deve receber no prompt:

```
LEIA ANTES DE INICIAR:
1. docs/ENHANCEMENT_LOG.md — gates aprovados até agora
2. Docs/Atualizações.txt — histórico de produção
3. Não contrarie nenhuma decisão registrada nos gates anteriores
4. Ao concluir, registre o que fez no Gate correspondente
```

---

## 6. FASE 0 — LEITURA E COMPREENSÃO DO PROJETO

> **OBRIGATÓRIA. Sempre. Independente do tamanho da melhoria.**

### Checkpoint 0-A: Sincronização com o Projeto

```
1. Leia Docs/Atualizações.txt
   → Há algum problema similar já resolvido antes?
   → O que está documentado como funcionando?

2. Leia supabase_schema.sql
   → Estado atual real do banco de dados

3. Leia Docs/Schema atual.txt
   → Tabelas e campos disponíveis

4. Leia docs/ENHANCEMENT_LOG.md (se existir)
   → Contexto de sessões anteriores
```

### Checkpoint 0-B: Análise de Saúde

```bash
# Dependências com vulnerabilidades?
npm audit

# Build atual está passando?
npm run build

# Lint sem erros críticos?
npm run lint
```

### Checkpoint 0-C: Mapeamento de Impacto

Antes de qualquer ação, identifique:

- Quais arquivos/módulos a melhoria vai tocar
- Quais funcionalidades existentes podem ser afetadas
- Se a melhoria envolve banco de dados → qual tabela, qual impacto nos dados existentes
- Zonas de risco (autenticação, acesso por `user_id`, fuso horário, Evolution API, n8n)

### Relatório de Estado (apresentar ao usuário)

```markdown
## 📊 Estado Atual do Projeto

### Sincronização
- Atualizações.txt: lido ✅ — última entrada: [data e resumo]
- Schema do banco: lido ✅ — tabelas: users, financial, transactions, documents
- Log anterior: [existe / não existe]

### Saúde
- Build: [OK / Problemas: descrever]
- Lint: [OK / Warnings: N]
- Vulnerabilidades npm: [N por severidade]

### Histórico Relevante
- Problema similar já existiu? [Sim: descrever o que foi feito / Não]

### Impacto Mapeado
- Arquivos que serão tocados: [lista]
- Funcionalidades em risco: [lista ou "Nenhuma identificada"]
- Banco de dados afetado? [Sim: tabela X / Não]

### ⚠️ Zonas de Risco Identificadas
- [Zona]: [Razão]
```

> 🔴 **GATE 0:** Apresente o relatório. Aguarde confirmação do usuário.
> Se o usuário confirmar → registre no Enhancement Log e avance.

### ✍️ Registro do GATE 0

Substitua `<!-- GATE 0 -->` no log por:

```markdown
## [GATE 0] — Leitura e Sincronização
**Aprovado em:** [DD/MM/AAAA HH:mm]

### Contexto lido
- Atualizações.txt: última entrada [data] — [resumo]
- Schema: [observações relevantes]
- Histórico relevante: [existe ou não]

### Saúde do projeto
- Build: [status] | Lint: [status] | npm audit: [status]

### Impacto mapeado
- Arquivos afetados: [lista]
- BD envolvido: [sim/não + tabela]

### Zonas de risco (não tocar sem análise)
- [Zona]: [Razão]

### O que NÃO será tocado nesta sessão
- Lógica de autenticação (access_token)
- Filtros de user_id existentes
- [Outros identificados]

### Contexto obrigatório para agentes
Stack: Vite + React 19 + Supabase. Sem TypeScript. RLS obrigatório (filtrar por user_id).
Sistema em produção — qualquer alteração em lógica existente exige confirmação explícita.
```

Atualize a tabela: `| GATE 0 | Leitura e sincronização | ✅ Aprovado | [data] |`

---

## 7. FASE 1 — BRAINSTORM E CONFIRMAÇÃO DA IDEIA

> Nunca assuma que entendeu completamente. Explore com o usuário.

### Checkpoint 1-A: Captura da Ideia

```markdown
## 💡 Entendendo sua ideia

Entendi que você quer [RESUMO DA IDEIA].

Antes de qualquer planejamento, preciso entender:

**P0 — Crítico:**
1. Qual problema ou oportunidade isso resolve?
2. Como você imagina o resultado final funcionando?
3. É algo novo ou melhoria/correção de algo existente?

**P1 — Arquitetura:**
4. Isso envolve o banco de dados? Qual tabela?
5. Tem impacto na autenticação ou no acesso por user_id?
6. Há alguma restrição? (não mexer em X, manter Y funcionando)
```

### Checkpoint 1-B: Verificação de Histórico

**Antes de propor abordagens**, verifique em `Docs/Atualizações.txt`:
- Essa funcionalidade ou algo similar já foi tentado antes?
- Houve problemas anteriores nessa área?
- Se sim: apresente ao usuário o que foi feito antes e pergunte se deseja a mesma abordagem ou diferente.

### Checkpoint 1-C: Brainstorm de Abordagens

Apresente no mínimo 2 abordagens (3 quando a melhoria for complexa):

```markdown
## 🔀 Abordagens Possíveis

### Abordagem A — [Nome] (Conservadora)
**Como funciona:** [Descrição]
**Arquivos tocados:** [Lista]
**Envolve BD?** [Sim/Não + tabela se sim]
**Pros:** [Lista]
**Contras:** [Lista]
**Risco de regressão:** Baixo / Médio / Alto

---

### Abordagem B — [Nome] (Recomendada)
[mesma estrutura]

---

## 💡 Recomendação
**Abordagem [X]** porque [razão técnica + impacto no projeto em produção].

Qual você prefere?
```

### Checkpoint 1-D: Confirmação Formal de Escopo

> 🔴 **GATE 1:** Aguarde SIM explícito antes de planejar qualquer coisa.

```markdown
## ✅ Confirmação de Escopo

**O que será feito:** [Descrição exata]
**Abordagem:** [Escolhida]
**Arquivos que serão modificados:** [Lista]
**Banco de dados:** [Tabela afetada / Não será tocado]
**O que NÃO será tocado:** [Lista explícita]
**Critério de sucesso:** [Como saberemos que está pronto]

Você confirma este escopo? [SIM / NÃO / AJUSTAR]
```

### ✍️ Registro do GATE 1

```markdown
## [GATE 1] — Brainstorm e Escopo Confirmado
**Aprovado em:** [DD/MM/AAAA HH:mm]

### Melhoria confirmada
[Descrição exata aprovada pelo usuário]

### Abordagem escolhida
[Nome e razão]

### Abordagens rejeitadas
| Abordagem | Por que foi rejeitada |
|-----------|----------------------|
| [A] | [Razão] |

### Escopo
**DENTRO:** [Lista]
**FORA:** [Lista — incluindo "lógica de autenticação", "filtros user_id" se não forem tocados]

### BD envolvido
[Tabela + tipo de alteração / Não será tocado]

### Critério de sucesso
[Como o usuário validará que está funcionando]

### Contexto obrigatório para agentes
Melhoria: [Resumo]. Abordagem: [Nome].
Restrições da stack: React 19 + .jsx (sem TS). RLS obrigatório.
Não alterar: [Lista do FORA do escopo].
```

---

## 8. FASE 2 — PLANEJAMENTO

> Invoque o `project-planner` com contexto completo.

### Briefing obrigatório para o project-planner

```
LEIA ANTES DE INICIAR: docs/ENHANCEMENT_LOG.md e Docs/Atualizações.txt

PROJETO:
- Stack: Vite + React 19 (.jsx, sem TypeScript) + Supabase (supabase-js 2.86.0)
- BD: PostgreSQL via Supabase — tabelas: users, financial, transactions, documents
- Auth: access_token na tabela users (NÃO alterar este fluxo)
- Sistema EM PRODUÇÃO

MELHORIA:
[Descrição aprovada no Gate 1]

RESTRIÇÕES ABSOLUTAS:
- Não alterar lógica de autenticação
- Toda query deve filtrar por user_id (RLS)
- Usar lib/supabase.js para conexão
- Não migrar para TypeScript
- Não usar Redux
- Commits em Português
- GitHub: somente quando usuário solicitar

PLANO DEVE CONTER:
- Tarefas ordenadas com dependências
- Agente responsável por cada tarefa
- Arquivos que cada tarefa cria/modifica
- Critério de verificação de cada tarefa
- Rollback de cada tarefa (especialmente se envolver migração de BD)
```

### Checklist de validação do plano

Se envolver banco de dados:
- [ ] Migration é reversível (down)?
- [ ] Dados existentes de produção serão preservados?
- [ ] Filtro por `user_id` mantido em todos os novos acessos?
- [ ] Novos campos usam `timestamp with time zone` (se forem datas)?

> 🔴 **GATE 2:** Apresente o plano. Aguarde aprovação.

### ✍️ Registro do GATE 2

```markdown
## [GATE 2] — Plano Aprovado
**Aprovado em:** [DD/MM/AAAA HH:mm]

### Tarefas
| # | Tarefa | Agente | Arquivos | Dependências |
|---|--------|--------|----------|--------------|
| 1 | [Nome] | [Agente] | [Arquivos] | — |
| 2 | [Nome] | [Agente] | [Arquivos] | Tarefa 1 |

### BD afetado?
[Não / Sim: tabela X — migration reversível: Sim/Não]

### Dependências novas
[Nenhuma / Lista com versões]

### O que NÃO será tocado (reconfirmado)
[Lista do Gate 1 + novos itens identificados no planejamento]

### Contexto obrigatório para agentes
Executar APENAS as tarefas da tabela acima.
RLS obrigatório (user_id). Usar lib/supabase.js. Arquivos .jsx.
Ao concluir cada tarefa, registrar no Gate 3.
```

---

## 9. FASE 3 — EXECUÇÃO CONTROLADA

> Uma tarefa por vez. Valide antes de avançar.

### Instrução obrigatória para todo agente invocado nesta fase

```
LEIA ANTES DE INICIAR: docs/ENHANCEMENT_LOG.md
Consulte Gates 0, 1 e 2. Não desvie do escopo registrado.
Restrições obrigatórias desta stack:
- Componentes em .jsx (sem TypeScript)
- Toda query via lib/supabase.js com filtro user_id
- Não alterar lógica de autenticação (access_token)
- Sistema em produção: não quebre o que está funcionando
Ao concluir, registre no Gate 3 do Enhancement Log.
```

### Por tarefa executada

```markdown
## Executando: [Nome da Tarefa]
**Agente:** [Nome]
**Arquivos:** [Lista]

[AGENTE EXECUTA]

**Resultado:**
- [ ] Arquivos criados/modificados conforme plano
- [ ] Filtro user_id presente em todas as novas queries (se BD)
- [ ] Sem alteração em lógica de autenticação
- [ ] Build passa após a mudança
- [ ] Lint sem erros novos

**Status:** ✅ Concluída / ❌ Falhou → acionar debugger
```

### Registro incremental — Gate 3

Cada tarefa concluída acrescenta ao bloco `<!-- GATE 3 -->`:

```markdown
### Tarefa [N] — [Nome] — [DD/MM/AAAA HH:mm]
**Agente:** [Nome]
**Status:** ✅ Concluída
**Arquivos criados/modificados:**
- `[path]`: [O que foi feito]
**Queries com user_id:** [Sim / Não aplicável]
**Decisões pontuais:** [Nenhuma / Descrição]
**Desvios do plano:** [Nenhum / Descrição — aprovado por quem]
```

### Protocolo de emergência

```
🚨 SE ALGO QUEBRAR:

1. PARE imediatamente
2. NÃO tente corrigir sem chamar o debugger
3. Invoque debugger com:
   - O que estava funcionando: [X]
   - O que foi modificado: [Y]
   - O que quebrou: [Z] + erro exato
4. debugger identifica causa raiz
5. Agente responsável corrige
6. Valide que o fix não criou novo problema
7. Continue somente se tudo OK

SE não conseguir corrigir:
→ Execute rollback da tarefa
→ Informe o usuário
→ Discuta abordagem alternativa antes de tentar novamente
```

### Fechamento do Gate 3

```markdown
## [GATE 3] — Execução Concluída
**Concluído em:** [DD/MM/AAAA HH:mm]

### Tarefas
| # | Tarefa | Agente | Status | Desvios |
|---|--------|--------|--------|---------|
| 1 | [Nome] | [Agente] | ✅ | Nenhum |

### Todos os arquivos modificados nesta sessão
- `[path]`: [O que foi feito]

### Problemas encontrados e soluções
- [Problema]: [Solução aplicada]
- Nenhum (se não houve)

### Contexto para verificação
[O que o verificador precisa saber para testar corretamente]
```

---

## 10. FASE 4 — VERIFICAÇÃO E QUALIDADE

> **Agentes de verificação recebem:** `"LEIA docs/ENHANCEMENT_LOG.md Gates 0-3 antes de iniciar."`

### Testes

```bash
# Testes existentes continuam passando?
npm test

# Build continua funcionando?
npm run build

# Lint sem novos erros?
npm run lint
```

### Verificações específicas desta stack

- [ ] Todas as novas queries filtram por `user_id`?
- [ ] Novas queries usam `lib/supabase.js`?
- [ ] Se houver campos de data: usando `timestamp with time zone`?
- [ ] Se houver formatação de data no frontend: convertendo para UTC-3?
- [ ] Se envolveu BD: dados de produção existentes foram preservados?
- [ ] A autenticação por `access_token` continua funcionando?

### Segurança (obrigatório se tocar em auth, user_id ou dados de usuário)

```
Invoque security-auditor com:
"LEIA docs/ENHANCEMENT_LOG.md Gate 3 — lista de arquivos modificados.
Foque especialmente em:
- Queries sem filtro user_id (RLS bypass)
- Exposição de dados entre usuários
- Validação de inputs antes de inserção no Supabase"
```

### Relatório de verificação

```markdown
## 📊 Verificação

| Check | Status | Detalhe |
|-------|--------|---------|
| Build | ✅/❌ | — |
| Lint | ✅/❌ | [N erros/warnings] |
| Testes existentes | ✅/❌ | [N passando] |
| Filtro user_id | ✅/❌ | Presente em todas as queries |
| Autenticação intacta | ✅/❌ | — |
| Dados preservados (BD) | ✅/N.A. | — |
| Datas UTC-3 | ✅/N.A. | — |
| Segurança | ✅/❌ | [Issues encontrados] |
```

> 🔴 **GATE 4:** Todo ❌ bloqueia o avanço. Corrija e re-execute.

### ✍️ Registro do GATE 4

```markdown
## [GATE 4] — Verificação Aprovada
**Aprovado em:** [DD/MM/AAAA HH:mm]

### Resultados
[Tabela de verificação preenchida]

### Issues encontrados e resolvidos
- [Issue]: [Solução]

### Qualidade antes vs. depois
- Build: [status antes] → [status depois]
- Lint: [N erros antes] → [N erros depois]
```

---

## 11. FASE 5 — DOCUMENTAÇÃO

> **O `documentation-writer` recebe:** `"LEIA docs/ENHANCEMENT_LOG.md completo antes de qualquer escrita."`

### Atualizar Docs/Atualizações.txt

> ⚠️ **Esta atualização é feita somente APÓS o deploy no GitHub** (ver Fase 6).
> Durante a Fase 5, prepare o conteúdo mas não escreva ainda.

**Template obrigatório (não alterar a estrutura):**

```text
Atualizações
Data: [DD/MM/AAAA]
1. o que foi atualizado: [descrição]
2. o que foi implementado: [descrição]
Quais arquivos foram atualizados: [lista]
Quais arquivos foram removidos: [lista ou "Nenhum"]
Quais arquivos foram adicionados: [lista ou "Nenhum"]
```

### Atualizar supabase_schema.sql e Schema atual.txt (se BD foi alterado)

Se a melhoria envolveu mudanças no banco de dados:
- Atualize `supabase_schema.sql` com o schema atual
- Atualize `Docs/Schema atual.txt` com as tabelas e campos modificados

### Atualizar README (se necessário)

Somente se a melhoria mudou forma de usar o projeto, configuração ou dependências.

### ✍️ Registro do GATE 5

```markdown
## [GATE 5] — Documentação Preparada
**Concluído em:** [DD/MM/AAAA HH:mm]

### Documentos preparados
| Documento | Status | O que contém |
|-----------|--------|-------------|
| Docs/Atualizações.txt | Pronto para escrever pós-deploy | [Resumo] |
| supabase_schema.sql | [Atualizado / N.A.] | — |
| Docs/Schema atual.txt | [Atualizado / N.A.] | — |
| README | [Atualizado / N.A.] | — |

### Rastreabilidade
Gate 1 (escopo) → Gate 2 (plano) → Gate 3 (execução) → Gate 4 (verificação) → Gate 5 (doc)
```

---

## 12. FASE 6 — GITHUB

> Somente execute após comando explícito do usuário.

### Verificação antes do commit

```bash
git status           # Ver todos os arquivos modificados
git diff             # Revisar cada mudança

# NUNCA commitar:
# ❌ .env ou qualquer variante
# ❌ node_modules/
# ❌ dist/ ou .next/ (build outputs)
# ❌ arquivos com tokens, chaves ou dados de usuários reais
```

### Staging e commit

```bash
# O Enhancement Log vai junto — faz parte da entrega
git add docs/ENHANCEMENT_LOG.md

# Arquivos da melhoria
git add [arquivos modificados]
git add [arquivos de documentação preparados]

git status   # Confirmar o que vai no commit
```

**Conventional Commits em Português:**

```bash
# Padrão: <tipo>(<escopo>): <descrição em português>
#
# feat:     nova funcionalidade
# fix:      correção de bug
# refactor: refatoração sem mudança de comportamento
# perf:     melhoria de performance
# docs:     apenas documentação
# test:     testes
# chore:    manutenção

git commit -m "feat(financeiro): adicionar filtro de período no dashboard

- Implementar seletor de data com fuso UTC-3
- Atualizar query de transactions com filtro de período
- Adicionar estado local com useDashboardData"
```

### Após o push — escrever Docs/Atualizações.txt

Somente agora, após confirmação do push bem-sucedido, escreva a entrada em `Docs/Atualizações.txt` usando o template da Diretriz 1.3.

### ✍️ Registro do GATE 6 — Fechamento

```markdown
## [GATE 6] — GitHub Atualizado / Sessão Encerrada
**Concluído em:** [DD/MM/AAAA HH:mm]

### Commits
| Hash | Mensagem | Arquivos |
|------|----------|---------|
| [hash] | [mensagem] | [N arquivos] |

### Docs/Atualizações.txt
Entrada escrita: ✅ — Data: [DD/MM/AAAA]

### Resumo da sessão
**Melhoria:** [1-2 frases]
**Resultado:** ✅ Implementada, testada, documentada e no GitHub

### Para a próxima sessão
Ao retomar este projeto:
1. Ler Docs/Atualizações.txt
2. Ler supabase_schema.sql
3. Ler docs/ENHANCEMENT_LOG.md (Gate 0 ao 6)
Testes passando: [N] — usar como baseline.

### Melhorias futuras identificadas
- [Item identificado durante esta sessão]
```

Finalize o cabeçalho do log:
```
## Status: ✅ CONCLUÍDO
## Concluído em: [DD/MM/AAAA HH:mm]
```

---

## 13. REGRAS DE OURO

### 1. Stack é fixa
```
❌ "Poderia migrar para TypeScript..."
✅ Implementar em .jsx conforme a stack existente
```

### 2. Produção exige confirmação
```
❌ Alterar lógica existente que funciona sem avisar
✅ Explicar risco + justificar + perguntar "Posso prosseguir?"
```

### 3. Histórico é consultado antes de agir
```
❌ Propor solução sem verificar Docs/Atualizações.txt
✅ Verificar se o problema já foi resolvido antes e como
```

### 4. user_id em toda query
```
❌ Query sem filtro de usuário
✅ Todo acesso a dados filtra por user_id — sem exceção
```

### 5. GitHub só quando solicitado
```
❌ "Já subi no GitHub"
✅ "Está pronto. Posso fazer o commit?"
```

### 6. Docs/Atualizações.txt somente pós-deploy
```
❌ Atualizar o arquivo antes do push
✅ Push confirmado → então atualizar o arquivo com o template
```

### 7. Uma tarefa por vez com validação
```
❌ Executar tudo de uma vez
✅ Executar → validar → próxima tarefa
```

### 8. Enhancement Log é a memória da sessão
```
❌ Agente começa sem ler o log
✅ Todo agente lê o log antes de qualquer ação

❌ Gate passa sem registro
✅ Todo gate aprovado gera entrada no log

❌ Próxima sessão começa sem contexto
✅ Gate 6 documenta tudo para retomada sem perda de contexto
```

### 9. Segurança nunca é opcional
```
❌ "É uma mudança pequena, não precisa de revisão"
✅ Qualquer mudança em dados de usuário = verificar filtro user_id + RLS
```

### 10. Autenticação é intocável
```
❌ Sugerir login por senha ou alterar o fluxo de access_token
✅ Manter o fluxo existente e documentado
```

---

## 📊 Visão Rápida — O Que Fazer em Cada Situação

| Situação | Primeira ação |
|----------|---------------|
| Nova sessão (qualquer) | Ler Atualizações.txt + supabase_schema.sql |
| Usuário pede melhoria | Iniciar Fase 0 — criar Enhancement Log |
| Problema parece familiar | Verificar em Atualizações.txt antes de propor solução |
| Alteração em lógica existente | Explicar risco + pedir permissão explícita |
| Tarefa concluída | Registrar no Gate 3 do Enhancement Log |
| Pronto para testar | Fase 4 — checklist completo incluindo user_id |
| Usuário pede commit | Fase 6 — staging + commit em pt-BR + Atualizações.txt pós-push |
| Dúvida sobre schema | Consultar Docs/Schema atual.txt + supabase_schema.sql |
