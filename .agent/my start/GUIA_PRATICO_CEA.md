# Guia Prático de Uso do Sistema CEA
## Como Usar o Chief Executive Agent na Prática

> **Manual de operação para desenvolvedores e gerentes de projeto**

---

## 📋 ÍNDICE

1. [Instalação e Setup](#instalação-e-setup)
2. [Comandos Básicos](#comandos-básicos)
3. [Fluxos de Trabalho Comuns](#fluxos-de-trabalho-comuns)
4. [Troubleshooting](#troubleshooting)
5. [Melhores Práticas](#melhores-práticas)

---

## 🔧 INSTALAÇÃO E SETUP

### Estrutura de Arquivos Necessária

```
seu-projeto/
├── .agent/
│   ├── agents/
│   │   ├── chief-executive-agent.md       # ← Arquivo que criamos
│   │   ├── orchestrator.md
│   │   ├── project-planner.md
│   │   └── ... (outros 17 agentes)
│   │
│   ├── skills/
│   │   ├── api-patterns/
│   │   ├── frontend-design/
│   │   └── ... (outras 36 skills)
│   │
│   ├── workflows/
│   │   ├── brainstorm.md
│   │   ├── plan.md
│   │   └── ... (outros 9 workflows)
│   │
│   └── ARCHITECTURE.md
│
├── docs/
│   ├── PLAN.md                            # ← Criado pelo project-planner
│   ├── GLOBAL_STATE.md                    # ← Criado pelo CEA
│   │
│   └── contracts/                         # ← Criados pelo CEA
│       ├── HANDOFF_01_PLANNER_TO_DB.md
│       ├── HANDOFF_02_DB_TO_BACKEND.md
│       └── ... (outros contratos)
│
└── [seu código aqui]
```

### Setup Inicial

```bash
# 1. Copiar os agentes customizados para .agent/agents/
cp chief-executive-agent.md .agent/agents/

# 2. Criar diretório de documentação
mkdir -p docs/contracts

# 3. Inicializar GLOBAL_STATE.md
touch docs/GLOBAL_STATE.md

# 4. Verificar disponibilidade de agentes
ls -la .agent/agents/
```

---

## 💻 COMANDOS BÁSICOS

### Como Invocar o CEA

**Na interface do Claude Code ou Claude.ai:**

```
Você: "Use chief-executive-agent para iniciar o projeto X"

CEA: [inicia protocolo de análise]
```

**Ou de forma mais direta:**

```
Você: "CEA, preciso criar uma aplicação de [descrição]"

CEA: [executa CHECKPOINT 0]
```

### Comandos para o CEA

| Comando | Quando Usar | Exemplo |
|---------|-------------|---------|
| `CEA, analise este projeto` | Primeira vez trabalhando em projeto | Entender estrutura existente |
| `CEA, crie um plano para [feature]` | Nova feature complexa | Adicionar sistema de pagamentos |
| `CEA, execute o PLAN.md` | Após plano aprovado | Iniciar desenvolvimento |
| `CEA, valide o checkpoint [N]` | Após conclusão de fase | Verificar qualidade |
| `CEA, status do projeto` | Verificar progresso | Ver o que falta |
| `CEA, crie contratos para [módulo]` | Módulo novo | Garantir handoffs claros |

---

## 🔄 FLUXOS DE TRABALHO COMUNS

### Fluxo 1: Iniciar Projeto do Zero

```
┌──────────────────────────────────────────────────────────┐
│ 1. VOCÊ INICIA                                           │
└─────────────────────┬────────────────────────────────────┘
                      │
                      ▼
┌──────────────────────────────────────────────────────────┐
│ Você: "CEA, quero criar [descrição do projeto]"          │
└─────────────────────┬────────────────────────────────────┘
                      │
                      ▼
┌──────────────────────────────────────────────────────────┐
│ 2. CEA ANALISA (CHECKPOINT 0)                            │
│ - Identifica tipo de projeto                             │
│ - Estima complexidade                                    │
│ - Decide se precisa clarificação                         │
└─────────────────────┬────────────────────────────────────┘
                      │
          ┌───────────┴───────────┐
          │                       │
     [CLARO]                  [VAGO]
          │                       │
          ▼                       ▼
┌─────────────────┐    ┌──────────────────────┐
│ 3. PULA PARA 4  │    │ 3. CEA FAZ PERGUNTAS │
└─────────────────┘    │ /brainstorm workflow │
                       └──────────┬───────────┘
                                  │
                       Você responde
                                  │
                                  ▼
┌──────────────────────────────────────────────────────────┐
│ 4. CEA INVOCA project-planner                            │
│ "Crie PLAN.md detalhado"                                 │
└─────────────────────┬────────────────────────────────────┘
                      │
                      ▼
┌──────────────────────────────────────────────────────────┐
│ 5. project-planner CRIA PLAN.md                          │
│ - Breakdown de tarefas                                   │
│ - Estrutura de diretórios                                │
│ - Estimativas de tempo                                   │
└─────────────────────┬────────────────────────────────────┘
                      │
                      ▼
┌──────────────────────────────────────────────────────────┐
│ 6. CEA APRESENTA PLANO                                   │
│ "Aqui está o plano. Aprovar?"                            │
└─────────────────────┬────────────────────────────────────┘
                      │
          ┌───────────┴───────────┐
          │                       │
    [APROVADO]                [AJUSTAR]
          │                       │
          ▼                       ▼
┌─────────────────┐    ┌──────────────────────┐
│ 7. CEA CRIA     │    │ 7. project-planner   │
│ CONTRATOS       │    │ AJUSTA PLAN.md       │
└────────┬────────┘    └──────────┬───────────┘
         │                        │
         ▼                        ▼
         └───────────┬────────────┘
                     │
                     ▼
┌──────────────────────────────────────────────────────────┐
│ 8. CEA INICIALIZA GLOBAL_STATE.md                        │
└─────────────────────┬────────────────────────────────────┘
                      │
                      ▼
┌──────────────────────────────────────────────────────────┐
│ 9. CEA INVOCA PRIMEIRO AGENTE (geralmente database)      │
│ Com contexto + contrato                                  │
└─────────────────────┬────────────────────────────────────┘
                      │
                      ▼
┌──────────────────────────────────────────────────────────┐
│ 10. EXECUÇÃO SEQUENCIAL                                  │
│ Agente → CEA valida → Próximo agente                     │
└──────────────────────────────────────────────────────────┘
```

**Exemplo Prático:**

```
Você:
"CEA, quero criar uma API REST para gerenciar tarefas com autenticação JWT"

CEA:
"🔍 ANÁLISE INICIAL
- Tipo: WEB_BACKEND (API)
- Complexidade: MÉDIA (4-5 agentes)
- Tech Stack sugerido: Node.js + Express + PostgreSQL + JWT

Requisição está clara. Prosseguindo.

📋 Invocando project-planner para criar PLAN.md..."

[project-planner executa]

CEA:
"✅ PLAN.md criado com 4 fases:
1. Database: Schema de Users + Tasks
2. Backend: Auth endpoints + Task CRUD
3. Testing: Unit + Integration tests
4. Security: JWT audit + Penetration test

Estimativa: 2-3 semanas

Aprovar este plano?"

Você:
"Aprovado"

CEA:
"✅ Plano aprovado.
📄 Criando 4 contratos de handoff...
✅ Contratos criados.
📊 Inicializando GLOBAL_STATE.md...
✅ Estado global inicializado.

🚀 Invocando database-architect para Fase 1..."

[database-architect trabalha]

CEA:
"✅ database-architect concluiu.
✅ CHECKPOINT 1 aprovado.
📄 HANDOFF assinado.

🚀 Invocando backend-specialist para Fase 2..."

[continua...]
```

---

### Fluxo 2: Adicionar Feature a Projeto Existente

```
Você:
"CEA, preciso adicionar sistema de comentários aos posts"

CEA:
"🔍 Analisando projeto existente...
[Lê GLOBAL_STATE.md]
[Lê ARCHITECTURE.md]

📊 ANÁLISE:
- Projeto: Blog Platform
- Módulo afetado: Posts
- Novos arquivos: comments/ (backend), CommentSection (frontend)
- Agentes necessários: 3
  1. database-architect (nova tabela Comments)
  2. backend-specialist (API de comentários)
  3. frontend-specialist (UI de comentários)

Complexidade: BAIXA-MÉDIA
Tempo estimado: 3-5 dias

Prosseguir?"

Você:
"Sim"

CEA:
"🚀 Invocando database-architect..."
[continua execução sequencial]
```

---

### Fluxo 3: Debug de Problema

```
Você:
"CEA, a autenticação JWT está retornando 401 mesmo com token válido"

CEA:
"🔍 ANÁLISE DE PROBLEMA:
- Tipo: DEBUG
- Módulo: Auth
- Agente ideal: debugger

🔎 Invocando debugger para investigação..."

debugger:
"📋 INVESTIGAÇÃO SISTEMÁTICA:
Fase 1: Reprodução
- [x] Consegui reproduzir o erro
- Token está válido mas sendo rejeitado

Fase 2: Isolamento
- [x] Testei endpoint isoladamente
- [x] Token é válido quando decodificado
- [x] Problema está no middleware de verificação

Fase 3: Análise
- 🎯 CAUSA RAIZ ENCONTRADA:
  Middleware está verificando com secret errado
  Env var JWT_SECRET não está sendo carregada
  
Fase 4: Solução
- Adicionar dotenv ao topo do servidor
- Verificar .env tem JWT_SECRET

Recomendo invocar backend-specialist para implementar fix."

CEA:
"✅ Causa raiz identificada.
🚀 Invocando backend-specialist para correção..."

backend-specialist:
"✅ Fix implementado:
- Adicionado require('dotenv').config() no topo
- Verificado .env.example tem JWT_SECRET
- Testado localmente - funcionando

Recomendo invocar test-engineer para adicionar teste de regressão."

CEA:
"✅ Fix validado.
🚀 Invocando test-engineer..."

test-engineer:
"✅ Teste de regressão adicionado:
- tests/auth/jwt-validation.test.ts
- Testa cenário de token válido
- Testa cenário de secret inválido

Todos os testes passando."

CEA:
"✅ PROBLEMA RESOLVIDO.
✅ Fix implementado.
✅ Teste de regressão adicionado.
✅ Validação completa.

Problema: 401 com token válido
Causa: dotenv não carregado
Solução: Adicionar dotenv.config()
Prevenção: Teste de regressão criado"
```

---

### Fluxo 4: Deploy para Produção

```
Você:
"CEA, estou pronto para fazer deploy para produção"

CEA:
"⚠️ CHECKPOINT DE PRODUÇÃO INICIADO

Executando validação completa..."

[CEA executa sequência de validações]

CEA:
"📊 RELATÓRIO DE PRONTIDÃO:

✅ CHECKPOINT 1: Código
  - Build: SUCCESS
  - Lint: PASSED
  - Types: OK

✅ CHECKPOINT 2: Testes
  - Unit: 92% cobertura (target: 80%+) ✅
  - Integration: PASSED
  - E2E: PASSED

⚠️ CHECKPOINT 3: Segurança
  - Security Audit: 1 MEDIUM issue encontrado
  - Pentest: NÃO EXECUTADO

❌ BLOQUEADOR ENCONTRADO:
  Issue #47: Rate limiting não implementado (MEDIUM)

🛑 DEPLOY BLOQUEADO.

Recomendação:
1. Invocar security-auditor para corrigir issue #47
2. Invocar penetration-tester para pentest
3. Re-executar CHECKPOINT 3

Prosseguir com correções?"

Você:
"Sim, corrija"

CEA:
"🚀 Invocando security-auditor..."

[security-auditor corrige]

CEA:
"✅ Issue #47 corrigido.
🚀 Invocando penetration-tester..."

[penetration-tester executa]

CEA:
"✅ Pentest concluído - 0 issues críticos.

🔄 Re-executando CHECKPOINT 3..."

CEA:
"✅ CHECKPOINT 3: Segurança
  - Security Audit: PASSED (0 issues)
  - Pentest: PASSED

✅ CHECKPOINT 4: Performance
  - Lighthouse: 94 (target: 90+) ✅
  - Core Web Vitals: PASSED

✅ CHECKPOINT 5: Documentação
  - README: COMPLETO
  - API Docs: COMPLETO
  - Deployment Guide: COMPLETO

✅ TODOS OS CHECKPOINTS APROVADOS.

🚀 Invocando devops-engineer para deploy..."

devops-engineer:
"📦 Deploy iniciado...
✅ Build bem-sucedido
✅ Deploy para production
✅ Smoke tests PASSED
✅ Monitoring ativo

🎉 DEPLOY CONCLUÍDO COM SUCESSO

URL: https://seu-app.com
Status: ONLINE
Uptime: 100%"

CEA:
"🎉 DEPLOY PARA PRODUÇÃO CONCLUÍDO.

✅ Todos os checkpoints aprovados
✅ Deploy bem-sucedido
✅ Aplicação online
✅ Monitoring ativo

Projeto movido para status: PRODUCTION"
```

---

## 🆘 TROUBLESHOOTING

### Problema 1: CEA não está seguindo contratos

**Sintoma:**
```
Agente X está modificando arquivos do domínio de Agente Y
```

**Solução:**
```
1. Pare a execução
2. "CEA, validar HANDOFF entre [Agente X] e [Agente Y]"
3. CEA verifica contrato
4. CEA reforça limites
5. Re-invoca agente com limites claros
```

**Prevenção:**
```
Sempre criar contratos ANTES de invocar agentes
```

---

### Problema 2: Agente ficou travado/confuso

**Sintoma:**
```
Agente está dando voltas sem progredir
```

**Solução:**
```
Você: "CEA, o [agente] está travado"

CEA: "🔍 Analisando estado do [agente]...
      
      📊 DIAGNÓSTICO:
      - Tarefa muito vaga
      - Falta contexto necessário
      
      🔄 AÇÃO CORRETIVA:
      - Interrompendo [agente]
      - Clarificando tarefa
      - Re-invocando com contexto melhor"
```

---

### Problema 3: Quero pular um checkpoint

**Sintoma:**
```
Você: "CEA, posso pular os testes e fazer deploy direto?"

CEA: "🛑 NEGATIVO.

Checkpoint de testes é obrigatório para deploy em produção.

Alternativas:
1. Deploy para STAGING (não requer todos os checkpoints)
2. Executar testes agora e depois deploy
3. Ativar flag EMERGENCY_DEPLOY (não recomendado)

Qual opção prefere?"
```

**Nunca pule checkpoints de segurança ou testes em produção.**

---

### Problema 4: Dois agentes entraram em conflito

**Sintoma:**
```
backend-specialist e frontend-specialist modificaram o mesmo arquivo
```

**Solução:**
```
Você: "CEA, conflito no arquivo X entre backend e frontend"

CEA: "🔍 Analisando conflito...
      
      📊 DIAGNÓSTICO:
      - Arquivo: src/types/api.ts
      - backend-specialist: Mudou interface do endpoint
      - frontend-specialist: Já estava usando interface antiga
      - CAUSA: Handoff não foi seguido
      
      🔄 AÇÃO CORRETIVA:
      1. Reverter mudanças de frontend-specialist
      2. Re-invocar frontend-specialist APÓS backend concluir
      3. Criar contrato explícito
      
      Executar correção?"
```

---

## 🌟 MELHORES PRÁTICAS

### 1. Sempre Leia o GLOBAL_STATE.md Antes de Solicitar Trabalho

```bash
# BOM
cat docs/GLOBAL_STATE.md
# [verifica status]
"CEA, adicionar feature X"

# RUIM
"CEA, adicionar feature X"
# [sem saber estado atual do projeto]
```

### 2. Seja Específico em Solicitações

```bash
# BOM
"CEA, adicionar autenticação JWT com refresh tokens no backend Node.js"

# RUIM
"CEA, adicionar login"
```

### 3. Aprove Planos Antes de Execução

```bash
# BOM
CEA: "Aqui está o plano. Aprovar?"
Você: [lê plano] "Aprovado" ou "Ajustar X e Y"

# RUIM
Você: "Só faça"
# [sem revisar plano]
```

### 4. Confie nos Checkpoints

```bash
# BOM
CEA: "Checkpoint 3 falhou - 2 testes quebraram"
Você: "OK, corrija e re-valide"

# RUIM
Você: "Ignora os testes, deploy mesmo assim"
```

### 5. Mantenha Contratos Atualizados

```bash
# BOM
[Requisito mudou]
"CEA, atualizar contrato DB-to-Backend com novo campo"

# RUIM
[Requisito mudou]
[Não atualiza contrato]
[Desconexão entre agentes]
```

### 6. Use Workflows Apropriados

```bash
# Para ideação vaga
/brainstorm

# Para planejamento detalhado
/plan

# Para execução coordenada
CEA ou /orchestrate

# Para debugging
"CEA, debug [problema]"
```

### 7. Documente Decisões Arquiteturais

```bash
# BOM
"CEA, documentar decisão: escolhemos PostgreSQL porque X, Y, Z"

# RUIM
[Escolhe tech stack]
[Não documenta razão]
[6 meses depois ninguém lembra por quê]
```

---

## 📊 MONITORAMENTO DE PROGRESSO

### Como Ver Status do Projeto

```bash
# Opção 1: Perguntar ao CEA
"CEA, status do projeto"

CEA: "📊 STATUS GERAL
- Fase atual: Sprint 5 (Frontend)
- Progresso: 65%
- Agentes ativos: frontend-specialist
- Contratos assinados: 8/12
- Checkpoints aprovados: 3/4
- Bloqueadores: 0
- ETA: 3 semanas"

# Opção 2: Ler GLOBAL_STATE.md
cat docs/GLOBAL_STATE.md

# Opção 3: Verificar contratos
ls docs/contracts/
cat docs/contracts/HANDOFF_XX_*.md
```

---

## 🎓 DICAS AVANÇADAS

### 1. Execução Paralela (Quando Possível)

```
CEA pode executar agentes em paralelo SE não há dependências:

Exemplo:
- frontend-specialist (UI) + mobile-developer (App)
  → Podem trabalhar em paralelo se API já existe

- backend-specialist + frontend-specialist
  → NÃO podem trabalhar em paralelo (dependência)
```

### 2. Hotfix em Produção

```
Você: "CEA, hotfix urgente: [problema] em produção"

CEA: "🚨 MODO HOTFIX ATIVADO

Protocolo acelerado:
1. debugger → identifica causa (15min)
2. [agente responsável] → implementa fix (30min)
3. test-engineer → teste de regressão rápido (15min)
4. devops-engineer → deploy imediato

Checkpoints completos SERÃO PULADOS.
Auditoria completa SERÁ FEITA após fix.

Confirmar protocolo de emergência?"
```

### 3. Pair Programming com CEA

```
Você: "CEA, vou trabalhar no módulo X. Fique de olho."

CEA: "✅ Modo observador ativo.

Monitorarei:
- Se você violar domínios de outros agentes
- Se você quebrar contratos
- Se você criar débitos técnicos

Alertarei quando necessário."

[Você trabalha]

CEA: "⚠️ ALERTA: Você está prestes a modificar src/types/api.ts
Este arquivo é domínio de backend-specialist.
Recomendo criar uma interface nova ao invés de modificar."
```

---

## 📚 RECURSOS ADICIONAIS

### Documentos de Referência

1. **chief-executive-agent.md** - Guia completo do CEA
2. **HANDOFF_CONTRACTS_SYSTEM.md** - Sistema de contratos
3. **IMAGINE_PROJECT_BOOTSTRAP.md** - Exemplo real (IMAgine)
4. **ARCHITECTURE.md** - Arquitetura do sistema de agentes

### Scripts Úteis

```bash
# Validar contrato
python .agent/scripts/validate_contract.py docs/contracts/HANDOFF_XX.md

# Verificar estado global
cat docs/GLOBAL_STATE.md | grep "Status:"

# Listar agentes disponíveis
ls .agent/agents/

# Listar skills disponíveis
ls .agent/skills/
```

---

## 🎯 RESUMO EXECUTIVO

**O QUE É O CEA:**
- Diretor executivo dos agentes
- Orquestra, não executa
- Valida qualidade rigorosamente

**QUANDO USAR O CEA:**
- Projetos complexos (6+ agentes)
- Quando qualidade é crítica
- Quando desconexões devem ser evitadas
- Para projetos de longa duração

**QUANDO NÃO USAR O CEA:**
- Tarefas triviais (<2 agentes)
- Protótipos rápidos e sujos
- Quando processo é mais lento que valor agregado

**BENEFÍCIO PRINCIPAL:**
Qualidade consistente, sem retrabalho, com rastreabilidade completa.

---

**DÚVIDAS?**

Pergunte ao CEA:
```
"CEA, como faço [X]?"
"CEA, qual agente para [Y]?"
"CEA, explique [Z]"
```

O CEA está aqui para ajudar. Use-o. 🚀
