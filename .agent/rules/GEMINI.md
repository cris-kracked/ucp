---
trigger: always_on
---

PROJECT RULES & CONTEXT

## 0. DIRETRIZES CRÍTICAS (LEITURA OBRIGATÓRIA)
1.  **Idioma:** Pense e responda **sempre** em Português (pt-BR).
2.  **Contexto de Banco de Dados:** No início de cada nova sessão/chat, você deve buscar OBRIGATORIAMENTE e ler os arquivos `Docs/Atualizações.txt` e `supabase_schema.sql` para garantir que seu conhecimento do banco está sincronizado com a realidade.

3.  **Segurança de Produção:**
    * **Status:** O sistema está EM PRODUÇÃO. Alterações indevidas causam prejuízo real. Tome cuidado ao lidar com arquivos sensiveis, SEMPRE alerte ao usuario que ele deve fazer backup antes de qualquer alteração que vá acrescentar ou reduzir o que há nos arquivos.

    * **ATENÇÃO:** ANTES de alterações Significativas você OBRIGATORIAMENTE deve ler o arquivo Docs/Atualizações.txt e se certificar se alguma vez esse mesmo problema já existiu e o que foi feito, então planejar a melhor forma. 

    * **Proibição:** Jamais decida sozinho alterar políticas, lógica de negócios ou fluxos que já funcionam. São EXPRESSAMENTE PROIBIDAS A CRIAÇÃO DE NOVAS RPC no Supabase SEM A DEVIDA PERMISSÃO E justificativa!

    * **Protocolo:** Se uma alteração for necessária em algo existente, você deve EXPLICAR o risco, justificar a mudança e PERGUNTAR explicitamente: "Posso prosseguir com essa alteração?".

4.  **Workflow de Deploy e Documentação:**
    * **Github:** AGUARDE meu comando explícito para subir atualizações.
    * **Commits:** Devem ser escritos sempre em Português.
    * **Pós-Deploy:** SOMENTE após a atualização no Github, você deve atualizar o arquivo `Docs/Atualizações.txt` seguindo este template:

    ```text
    Atualizações
    Data: [DD/MM/AAAA]
    1. o que foi atualizado:
    2. o que foi implementado: 
    Quais arquivos foram atualizados:
    Quais arquivos foram removidos:
    Quais arquivos foram adicionados:
    ```

---

## 1. Tech Stack & Frameworks
- **Frontend Core:** Vite + React 19.2.0. Use functional components e Hooks exclusivamente.
- **Routing:** React Router DOM 7.9.6.
- **UI Components:** Recharts 3.6.0 (gráficos), Lucide React 0.555.0 (ícones).
- **Backend/Database:** Supabase (PostgreSQL). Use `@supabase/supabase-js` v2.86.0.
- **Integrations:** Evolution API (WhatsApp) e n8n (Workflows).
- **Linguagem:** JavaScript (.jsx) para componentes React. (Não migrar para TS agora).

## 2. Database & Data Fetching
- **Consciência do Schema:** Respeite rigorosamente as tabelas: `users`, `financial`, `transactions`, `documents`. Consulte `Schema atual.txt` antes de qualquer query complexa.
- **Autenticação:**
  - Login via `access_token` na tabela `users`.
  - NÃO crie login por senha.
  - Use `lib/supabase.js`.
- **Segurança (RLS):** Todas as queries devem filtrar por `user_id`.
- **Datas:** Use `timestamp with time zone`. No Dashboard, filtre pelo fuso horário do usuário (Brasil/UTC-3).

## 3. Restrições de Código
- **Estado:** Evite Redux. Use Context API ou Hooks simples (`useDashboardData`).