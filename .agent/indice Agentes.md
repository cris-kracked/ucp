# Arquitetura do Antigravity Kit

> Conjunto Abrangente de Ferramentas para Expansão de Capacidades de Agentes de IA

---

## 📋 Visão Geral

O Antigravity Kit é um sistema modular composto por:

* **20 Agentes Especialistas** - Personas de IA baseadas em funções
* **36 Skills (Habilidades)** - Módulos de conhecimento de domínios específicos
* **11 Workflows (Fluxos de Trabalho)** - Procedimentos acionados por comandos (slash commands)

---

## 🏗️ Estrutura de Diretórios

```plaintext
.agent/
├── ARCHITECTURE.md          # Este arquivo
├── agents/                  # 20 Agentes Especialistas
├── skills/                  # 36 Habilidades
├── workflows/               # 11 Comandos de Barra
├── rules/                   # Regras Globais
└── scripts/                 # Scripts de Validação Mestre

```

---

## 🤖 Agentes (20)

Personas de IA especialistas para diferentes domínios.

| Agente | Foco | Skills Utilizadas |
| --- | --- | --- |
| `orchestrator` | Coordenação multi-agente | parallel-agents, behavioral-modes |
| `project-planner` | Descoberta, planejamento | brainstorming, plan-writing, architecture |
| `frontend-specialist` | UI/UX Web | frontend-design, react-best-practices, tailwind-patterns |
| `backend-specialist` | API, lógica de negócio | api-patterns, nodejs-best-practices, database-design |
| `database-architect` | Esquema, SQL | database-design, prisma-expert |
| `mobile-developer` | iOS, Android, RN | mobile-design |
| `game-developer` | Lógica e mecânica de jogos | game-development |
| `devops-engineer` | CI/CD, Docker | deployment-procedures, docker-expert |
| `security-auditor` | Conformidade de segurança | vulnerability-scanner, red-team-tactics |
| `penetration-tester` | Segurança ofensiva | red-team-tactics |
| `test-engineer` | Estratégias de teste | testing-patterns, tdd-workflow, webapp-testing |
| `debugger` | Análise de causa raiz | systematic-debugging |
| `performance-optimizer` | Velocidade, Web Vitals | performance-profiling |
| `seo-specialist` | Ranking, visibilidade | seo-fundamentals, geo-fundamentals |
| `documentation-writer` | Manuais, documentação | documentation-templates |
| `product-manager` | Requisitos, user stories | plan-writing, brainstorming |
| `product-owner` | Estratégia, backlog, MVP | plan-writing, brainstorming |
| `qa-automation-engineer` | Testes E2E, pipelines CI | webapp-testing, testing-patterns |
| `code-archaeologist` | Código legado, refatoração | clean-code, code-review-checklist |
| `explorer-agent` | Análise da base de código | - |

---

## 🧩 Skills / Habilidades (36)

Domínios de conhecimento modulares que os agentes podem carregar sob demanda, com base no contexto da tarefa.

### Frontend & UI

| Skill | Descrição |
| --- | --- |
| `react-best-practices` | Otimização de React e Next.js (Vercel - 57 regras) |
| `web-design-guidelines` | Auditoria de UI Web - 100+ regras de acessibilidade, UX e performance |
| `tailwind-patterns` | Utilitários de Tailwind CSS v4 |
| `frontend-design` | Padrões de UI/UX, sistemas de design |
| `ui-ux-pro-max` | 50 estilos, 21 paletas, 50 fontes |

### Backend & API

| Skill | Descrição |
| --- | --- |
| `api-patterns` | REST, GraphQL, tRPC |
| `nestjs-expert` | Módulos NestJS, DI, decoradores |
| `nodejs-best-practices` | Assincronismo Node.js, módulos |
| `python-patterns` | Padrões Python, FastAPI |

### Banco de Dados

| Skill | Descrição |
| --- | --- |
| `database-design` | Design de esquema, otimização |
| `prisma-expert` | ORM Prisma, migrações |

### TypeScript/JavaScript

| Skill | Descrição |
| --- | --- |
| `typescript-expert` | Programação em nível de tipo, performance |

### Nuvem & Infraestrutura

| Skill | Descrição |
| --- | --- |
| `docker-expert` | Conteinerização, Compose |
| `deployment-procedures` | CI/CD, fluxos de implantação |
| `server-management` | Gerenciamento de infra |

### Testes & Qualidade

| Skill | Descrição |
| --- | --- |
| `testing-patterns` | Jest, Vitest, estratégias |
| `webapp-testing` | E2E, Playwright |
| `tdd-workflow` | Desenvolvimento orientado a testes |
| `code-review-checklist` | Padrões de revisão de código |
| `lint-and-validate` | Linting, validação |

### Segurança

| Skill | Descrição |
| --- | --- |
| `vulnerability-scanner` | Auditoria de segurança, OWASP |
| `red-team-tactics` | Segurança ofensiva |

### Arquitetura & Planejamento

| Skill | Descrição |
| --- | --- |
| `app-builder` | Estruturação (scaffold) Full-stack |
| `architecture` | Padrões de design de sistema |
| `plan-writing` | Planejamento e quebra de tarefas |
| `brainstorming` | Questionamento socrático |

### Mobile

| Skill | Descrição |
| --- | --- |
| `mobile-design` | Padrões de UI/UX para mobile |

### Desenvolvimento de Jogos

| Skill | Descrição |
| --- | --- |
| `game-development` | Lógica e mecânica de jogos |

### SEO & Crescimento

| Skill | Descrição |
| --- | --- |
| `seo-fundamentals` | SEO, E-E-A-T, Core Web Vitals |
| `geo-fundamentals` | Otimização para IA Generativa (GAO) |

### Shell/CLI

| Skill | Descrição |
| --- | --- |
| `bash-linux` | Comandos Linux, scripts |
| `powershell-windows` | Windows PowerShell |

### Outros

| Skill | Descrição |
| --- | --- |
| `clean-code` | Padrões de código (Global) |
| `behavioral-modes` | Personas de agentes |
| `parallel-agents` | Padrões multi-agente |
| `mcp-builder` | Protocolo de Contexto de Modelo |
| `documentation-templates` | Formatos de documentação |
| `i18n-localization` | Internacionalização |
| `performance-profiling` | Web Vitals, otimização |
| `systematic-debugging` | Resolução de problemas (Troubleshooting) |

---

## 🔄 Workflows / Fluxos (11)

Procedimentos de comando de barra. Invoque com `/comando`.

| Comando | Descrição |
| --- | --- |
| `/brainstorm` | Descoberta socrática |
| `/create` | Criar novas funcionalidades |
| `/debug` | Depurar problemas |
| `/deploy` | Implantar aplicação |
| `/enhance` | Melhorar código existente |
| `/orchestrate` | Coordenação multi-agente |
| `/plan` | Quebra de tarefas |
| `/preview` | Visualizar alterações |
| `/status` | Verificar status do projeto |
| `/test` | Executar testes |
| `/ui-ux-pro-max` | Design com 50 estilos |

---

## 🎯 Protocolo de Carregamento de Skills

```plaintext
Solicitação do Usuário → Correspondência de Descrição da Skill → Carregar SKILL.md
                                                                    ↓
                                                            Ler references/
                                                                    ↓
                                                            Ler scripts/

```

### Estrutura de uma Skill

```plaintext
skill-name/
├── SKILL.md           # (Obrigatório) Metadados e instruções
├── scripts/           # (Opcional) Scripts Python/Bash
├── references/        # (Opcional) Templates, documentação
└── assets/            # (Opcional) Imagens, logotipos

```

### Skills Aprimoradas (com scripts/referências)

| Skill | Arquivos | Cobertura |
| --- | --- | --- |
| `ui-ux-pro-max` | 27 | 50 estilos, 21 paletas, 50 fontes |
| `app-builder` | 20 | Estruturação Full-stack |

---

## ⚙️ Scripts (2)

Scripts de validação mestre que orquestram os scripts de nível de habilidade.

### Scripts Mestres

| Script | Finalidade | Quando usar |
| --- | --- | --- |
| `checklist.py` | Validação baseada em prioridade (Core) | Desenvolvimento, pre-commit |
| `verify_all.py` | Verificação abrangente (Todos os testes) | Pré-implantação, releases |

### Uso

```bash
# Validação rápida durante o desenvolvimento
python .agent/scripts/checklist.py .

# Verificação completa antes da implantação
python .agent/scripts/verify_all.py . --url http://localhost:3000

```

### O Que Eles Verificam

**checklist.py** (Verificações principais):

* Segurança (vulnerabilidades, segredos)
* Qualidade do Código (lint, tipos)
* Validação de Esquema
* Suíte de Testes
* Auditoria de UX
* Verificação de SEO

**verify_all.py** (Suíte completa):

* Tudo do checklist.py MAIS:
* Lighthouse (Core Web Vitals)
* Playwright E2E
* Análise de Bundle
* Auditoria Mobile
* Verificação de i18n

Para detalhes, veja `scripts/README.md`.

---

## 📊 Estatísticas

| Métrica | Valor |
| --- | --- |
| **Total de Agentes** | 20 |
| **Total de Skills** | 36 |
| **Total de Workflows** | 11 |
| **Total de Scripts** | 2 (mestre) + 18 (nível de skill) |
| **Cobertura** | ~90% desenvolvimento web/mobile |

---

## 🔗 Referência Rápida

| Necessidade | Agente | Skills |
| --- | --- | --- |
| App Web | `frontend-specialist` | react-best-practices, frontend-design |
| API | `backend-specialist` | api-patterns, nodejs-best-practices |
| Mobile | `mobile-developer` | mobile-design |
| Banco Dados | `database-architect` | database-design, prisma-expert |
| Segurança | `security-auditor` | vulnerability-scanner |
| Testes | `test-engineer` | testing-patterns, webapp-testing |
| Debug | `debugger` | systematic-debugging |
| Plano | `project-planner` | brainstorming, plan-writing |

