# Sistema de Contratos de Handoff
## Prevenção de Desconexões Entre Módulos

> **Propósito:** Garantir que cada agente sabe exatamente o que recebe, o que deve fazer, e o que entregar para o próximo.

---

## 📋 ÍNDICE

1. [Filosofia dos Contratos](#filosofia-dos-contratos)
2. [Template Universal](#template-universal-de-contrato)
3. [Contratos por Tipo de Projeto](#contratos-por-tipo-de-projeto)
4. [Sistema de Validação](#sistema-de-validação-de-contratos)
5. [Exemplos Práticos](#exemplos-práticos)

---

## 🎯 FILOSOFIA DOS CONTRATOS

### Por Que Contratos São Essenciais?

```
SEM CONTRATO:
database-architect cria schema
backend-specialist não sabe que schema existe
backend-specialist cria schema diferente
→ CONFLITO, RETRABALHO, DESCONEXÃO

COM CONTRATO:
database-architect cria schema
database-architect documenta schema no contrato
backend-specialist lê contrato
backend-specialist usa schema documentado
→ HARMONIA, EFICIÊNCIA, CONEXÃO
```

### Princípios Core

1. **Explícito > Implícito**
   - "O schema está em prisma/schema.prisma"
   - ❌ NÃO: "Criei o schema"

2. **Verificável > Confiável**
   - "API endpoint responde com status 200 em /api/health"
   - ❌ NÃO: "A API está funcionando"

3. **Limitado > Aberto**
   - "Modifique APENAS arquivos em src/components/auth/"
   - ❌ NÃO: "Faça o que for necessário"

4. **Sequencial > Paralelo (quando há dependência)**
   - "Aguarde database-architect concluir ANTES de iniciar"
   - ❌ NÃO: "Pode começar quando quiser"

---

## 📄 TEMPLATE UNIVERSAL DE CONTRATO

```markdown
# HANDOFF CONTRACT: [Agente Origem] → [Agente Destino]

## 🎯 Contexto Geral
**Projeto:** [Nome do Projeto]
**Fase Atual:** [Planning/Development/Testing/Production]
**Objetivo desta Etapa:** [Descrição breve do que está sendo construído]

---

## 📦 ENTREGÁVEIS DO [Agente Origem]

### Arquivos Criados/Modificados
- [ ] `caminho/para/arquivo1.ext` - [Descrição do que contém]
- [ ] `caminho/para/arquivo2.ext` - [Descrição do que contém]

### Estruturas Definidas
- [ ] Schema de dados: [Nome da tabela/modelo]
  - Campos: [lista de campos]
  - Relacionamentos: [lista de relações]
  
- [ ] API Endpoints: [Lista de endpoints]
  - `GET /api/rota` - [O que retorna]
  - `POST /api/rota` - [O que espera receber]

### Decisões Arquiteturais Tomadas
- [ ] Tecnologia escolhida: [Ex: PostgreSQL, Redis, etc]
- [ ] Pattern utilizado: [Ex: REST, Repository Pattern, etc]
- [ ] Justificativa: [Por que esta escolha]

### Validação Concluída
- [ ] Lint passou sem erros
- [ ] Testes do próprio agente passaram
- [ ] Build/Compile bem-sucedido

---

## 🎯 RESPONSABILIDADES DO [Agente Destino]

### O QUE DEVE FAZER ✅

1. **Consumir/Utilizar:**
   - [ ] Usar o schema definido em `arquivo.prisma`
   - [ ] Consumir endpoints da API
   - [ ] Seguir padrões estabelecidos

2. **Implementar:**
   - [ ] [Feature específica 1]
   - [ ] [Feature específica 2]
   - [ ] [Feature específica 3]

3. **Validar:**
   - [ ] Testar integração com trabalho anterior
   - [ ] Garantir compatibilidade
   - [ ] Verificar performance

### O QUE NÃO DEVE FAZER ❌

1. **NÃO Modificar:**
   - ❌ Schema do banco de dados (responsabilidade: database-architect)
   - ❌ Lógica de negócio da API (responsabilidade: backend-specialist)
   - ❌ Configurações de infraestrutura (responsabilidade: devops-engineer)

2. **NÃO Criar:**
   - ❌ Novos endpoints sem consultar backend-specialist
   - ❌ Novas tabelas sem consultar database-architect
   - ❌ Novas dependências sem verificar package.json

3. **NÃO Assumir:**
   - ❌ Que pode mudar comportamento de outros módulos
   - ❌ Que outras partes do sistema se adaptarão automaticamente
   - ❌ Que pode pular validações

---

## ✅ CRITÉRIOS DE ACEITAÇÃO

### Para Considerar Esta Etapa COMPLETA:

- [ ] Todos os itens de "O QUE DEVE FAZER" foram concluídos
- [ ] Nenhum item de "O QUE NÃO DEVE FAZER" foi violado
- [ ] Código compila/executa sem erros
- [ ] Testes (se aplicável) estão passando
- [ ] Integração com trabalho anterior foi validada
- [ ] Próximo handoff está documentado

### Métricas de Qualidade

- **Cobertura de Testes:** [X%] (se aplicável)
- **Performance:** [Métricas específicas]
- **Compatibilidade:** [Lista de browsers/dispositivos]

---

## 🔄 PRÓXIMO HANDOFF

**Após conclusão → [Próximo Agente]**

### O que o próximo agente receberá:
- [ ] [Lista de arquivos/estruturas]
- [ ] [Estado atual do sistema]
- [ ] [Pontos de atenção]

### Bloqueadores conhecidos:
- [ ] [Nenhum] ou [Lista de issues]

---

## 📞 COMUNICAÇÃO DE PROBLEMAS

### Se Encontrar Problemas:

1. **Documente o problema:** [Descrição clara]
2. **Identifique o agente responsável:** [Nome do agente]
3. **Notifique o CEA:** Para re-routing
4. **NÃO tente corrigir sozinho** se estiver fora do seu domínio

### Canais:
- Atualizar `GLOBAL_STATE.md` com status "BLOQUEADO"
- Marcar issue como "NEEDS_ATTENTION"
- Reportar ao Chief Executive Agent

---

## 📊 STATUS

**Data de Início:** [YYYY-MM-DD HH:mm]
**Data de Conclusão:** [YYYY-MM-DD HH:mm] ou [EM ANDAMENTO]
**Status Atual:** [PENDING/IN_PROGRESS/COMPLETED/BLOCKED]
**Responsável:** [Nome do Agente Atual]

---

## 🔐 ASSINATURAS (Validações)

- [ ] **[Agente Origem]** - Entregáveis validados e documentados
- [ ] **CEA** - Contrato revisado e aprovado
- [ ] **[Agente Destino]** - Contrato recebido e compreendido
- [ ] **[Agente Destino]** - Trabalho concluído e validado
- [ ] **CEA** - Validação final aprovada

---

**NOTA IMPORTANTE:** Este contrato é um DOCUMENTO VIVO. Atualize conforme necessário, mas SEMPRE notifique o CEA de mudanças.
```

---

## 🗂️ CONTRATOS POR TIPO DE PROJETO

### 1. Contrato: Database → Backend

```markdown
# HANDOFF: database-architect → backend-specialist

## Entregáveis do database-architect
- [x] `prisma/schema.prisma` - Schema completo com:
  - Tabela Users (id, email, password_hash, created_at)
  - Tabela Posts (id, user_id, title, content, published_at)
  - Relacionamento: User hasMany Posts

## Responsabilidades do backend-specialist
✅ DEVE:
- Usar Prisma Client para queries
- Implementar CRUD para Users e Posts
- Respeitar relações definidas no schema

❌ NÃO DEVE:
- Modificar schema.prisma (voltar para database-architect se necessário)
- Criar queries SQL diretas (usar Prisma)

## Critérios de Aceitação
- [ ] API endpoints criados para Users e Posts
- [ ] Testes de integração com banco passando
- [ ] Relacionamentos funcionando corretamente
```

### 2. Contrato: Backend → Frontend

```markdown
# HANDOFF: backend-specialist → frontend-specialist

## Entregáveis do backend-specialist
- [x] `src/api/users/route.ts` - CRUD de usuários
  - GET /api/users - Lista usuários (200 OK)
  - POST /api/users - Cria usuário (201 Created)
  - GET /api/users/[id] - Busca usuário (200 OK ou 404)
  - PUT /api/users/[id] - Atualiza usuário (200 OK)
  - DELETE /api/users/[id] - Deleta usuário (204 No Content)

## Responsabilidades do frontend-specialist
✅ DEVE:
- Consumir endpoints documentados
- Tratar todos os status codes
- Implementar loading states
- Implementar error states

❌ NÃO DEVE:
- Modificar lógica de API
- Fazer queries diretas ao banco
- Criar novos endpoints sem consultar backend

## Critérios de Aceitação
- [ ] UI consome API corretamente
- [ ] Todos os status codes tratados
- [ ] Loading e error states implementados
```

### 3. Contrato: Frontend → Test Engineer

```markdown
# HANDOFF: frontend-specialist → test-engineer

## Entregáveis do frontend-specialist
- [x] `src/components/UserForm.tsx` - Formulário de usuário
- [x] `src/components/UserList.tsx` - Lista de usuários
- [x] `src/hooks/useUsers.ts` - Hook para gerenciar usuários

## Responsabilidades do test-engineer
✅ DEVE:
- Criar testes unitários para cada componente
- Criar testes de integração para fluxo completo
- Mockar API calls
- Testar edge cases (erro, loading, empty state)

❌ NÃO DEVE:
- Modificar componentes (apenas adicionar test-ids se necessário)
- Modificar lógica de negócio
- Criar novos componentes

## Critérios de Aceitação
- [ ] Testes unitários com 80%+ cobertura
- [ ] Testes de integração cobrindo fluxo CRUD
- [ ] Todos os testes passando
```

### 4. Contrato: Test Engineer → Security Auditor

```markdown
# HANDOFF: test-engineer → security-auditor

## Entregáveis do test-engineer
- [x] Suíte de testes completa
- [x] Testes passando (95% cobertura)
- [x] Código validado funcionalmente

## Responsabilidades do security-auditor
✅ DEVE:
- Auditar autenticação e autorização
- Verificar sanitização de inputs
- Checar vulnerabilidades OWASP Top 10
- Validar tratamento de dados sensíveis
- Verificar HTTPS/TLS
- Auditar dependencies (npm audit)

❌ NÃO DEVE:
- Refatorar código (apenas apontar issues)
- Adicionar features
- Modificar testes

## Critérios de Aceitação
- [ ] Relatório de segurança gerado
- [ ] Issues de severidade HIGH corrigidos
- [ ] Issues de severidade MEDIUM documentados
- [ ] Plano de correção para LOW criado
```

### 5. Contrato: Security Auditor → DevOps Engineer

```markdown
# HANDOFF: security-auditor → devops-engineer

## Entregáveis do security-auditor
- [x] Security audit report (0 HIGH, 2 MEDIUM, 5 LOW)
- [x] Issues MEDIUM corrigidos
- [x] Plano de correção para LOW documentado
- [x] Certificação de segurança aprovada

## Responsabilidades do devops-engineer
✅ DEVE:
- Configurar CI/CD pipeline
- Configurar variáveis de ambiente (secrets)
- Implementar HTTPS
- Configurar monitoring
- Criar scripts de deploy
- Documentar rollback strategy

❌ NÃO DEVE:
- Modificar código da aplicação
- Alterar lógica de negócio
- Fazer deploy sem aprovação do CEA

## Critérios de Aceitação
- [ ] Pipeline CI/CD funcionando
- [ ] Deploy para staging bem-sucedido
- [ ] Monitoring configurado
- [ ] Rollback testado
- [ ] Documentação de deploy completa
```

---

## ✅ SISTEMA DE VALIDAÇÃO DE CONTRATOS

### Checklist de Validação (CEA)

Antes de aprovar um handoff:

```bash
☐ Contrato está completo? (todas as seções preenchidas)
☐ Entregáveis estão claramente listados?
☐ Responsabilidades estão explícitas (DEVE/NÃO DEVE)?
☐ Critérios de aceitação são mensuráveis?
☐ Próximo handoff está identificado?
☐ Bloqueadores estão documentados?
☐ Assinatura do agente origem presente?
```

### Script de Validação Automática

```python
# validate_contract.py

def validate_contract(contract_file):
    """Valida se um contrato está completo"""
    required_sections = [
        "Contexto Geral",
        "ENTREGÁVEIS DO",
        "RESPONSABILIDADES DO",
        "O QUE DEVE FAZER",
        "O QUE NÃO DEVE FAZER",
        "CRITÉRIOS DE ACEITAÇÃO",
        "PRÓXIMO HANDOFF",
        "STATUS"
    ]
    
    with open(contract_file, 'r') as f:
        content = f.read()
    
    missing = []
    for section in required_sections:
        if section not in content:
            missing.append(section)
    
    if missing:
        print(f"❌ Contrato INVÁLIDO. Seções faltando: {missing}")
        return False
    else:
        print("✅ Contrato VÁLIDO")
        return True

# Uso
validate_contract("HANDOFF_DB_TO_BACKEND.md")
```

---

## 📚 EXEMPLOS PRÁTICOS

### Exemplo 1: Projeto Web Fullstack

**Sequência de Contratos:**
```
1. project-planner → database-architect
2. database-architect → backend-specialist
3. backend-specialist → frontend-specialist
4. frontend-specialist → test-engineer
5. test-engineer → security-auditor
6. security-auditor → performance-optimizer
7. performance-optimizer → documentation-writer
8. documentation-writer → devops-engineer
```

**Cada handoff tem seu contrato.**

### Exemplo 2: Projeto Mobile

**Sequência de Contratos:**
```
1. project-planner → backend-specialist (API)
2. backend-specialist → mobile-developer
3. mobile-developer → test-engineer
4. test-engineer → security-auditor
5. security-auditor → devops-engineer
```

### Exemplo 3: Bug Fix

**Sequência de Contratos:**
```
1. debugger → [agente responsável]
2. [agente responsável] → test-engineer
3. test-engineer → CEA (validação final)
```

---

## 🔄 ATUALIZAÇÃO DE CONTRATOS

### Quando Atualizar

- Mudança de requisitos
- Descoberta de bloqueador
- Problema encontrado durante execução
- Necessidade de agente adicional

### Como Atualizar

1. Marcar contrato como "UPDATED"
2. Adicionar seção "CHANGELOG" no topo
3. Documentar o que mudou e por quê
4. Notificar CEA
5. CEA notifica agentes afetados

**Template de Changelog:**
```markdown
## CHANGELOG

### [YYYY-MM-DD HH:mm] - Atualização X
**Razão:** [Por que foi necessário atualizar]
**Mudanças:**
- Adicionado: [Item]
- Removido: [Item]
- Modificado: [Item]
**Impacto:** [Quais agentes são afetados]
**Status:** [Aprovado pelo CEA? SIM/NÃO]
```

---

## 🎯 BENEFÍCIOS DO SISTEMA DE CONTRATOS

### Antes (Sem Contratos)
```
❌ Agentes trabalhando em silos
❌ Retrabalho constante
❌ Conflitos de arquivos
❌ Decisões arquiteturais conflitantes
❌ Tempo perdido com confusão
❌ Bugs de integração
```

### Depois (Com Contratos)
```
✅ Comunicação clara entre agentes
✅ Trabalho sequencial coordenado
✅ Sem conflitos de arquivos
✅ Decisões arquiteturais consistentes
✅ Eficiência máxima
✅ Integração suave
```

---

## 📊 MÉTRICAS DE SUCESSO

**Contrato bem-sucedido quando:**
- ✅ Agente destino não teve dúvidas
- ✅ Agente destino não modificou trabalho do anterior
- ✅ Nenhum retrabalho foi necessário
- ✅ Integração foi suave
- ✅ Handoff aconteceu no prazo

**Contrato falhou quando:**
- ❌ Agente destino ficou confuso
- ❌ Agente destino teve que refazer trabalho anterior
- ❌ Houve conflito de arquivos
- ❌ Integração teve problemas
- ❌ Prazo foi excedido devido a confusão

---

## 🚀 COMO COMEÇAR

### Para o CEA:

1. Para cada novo projeto, crie pasta `contracts/`
2. Para cada handoff previsto, crie um arquivo:
   - `HANDOFF_01_PLANNER_TO_DB.md`
   - `HANDOFF_02_DB_TO_BACKEND.md`
   - `HANDOFF_03_BACKEND_TO_FRONTEND.md`
   - etc.

3. Preencha template para cada um
4. Apresente ao agente antes de invocar
5. Valide após agente concluir
6. Assine o contrato

### Para os Agentes:

1. Antes de iniciar trabalho, LEIA seu contrato
2. Durante execução, SIGA o contrato
3. Após concluir, ATUALIZE o contrato com status
4. DOCUMENTE qualquer desvio necessário
5. ASSINE o contrato quando concluir

---

**LEMBRE-SE:** Contratos não são burocracia. São GARANTIA de qualidade e eficiência.

**Um projeto sem contratos é como um prédio sem plantas: pode até ficar de pé, mas com muita sorte e retrabalho.**
