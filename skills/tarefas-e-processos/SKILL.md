---
name: tarefas-e-processos
description: >-
  Imans tasks, approvals and process drafts: list, create, assign, update, complete or archive tasks,
  check pending approvals and process runs, and draft a new business process for Process Studio.
  Use when the user says "cria uma tarefa", "atribui para o Thiago", "minhas tarefas", "o que está
  pendente de aprovação?", "conclui/arquiva a tarefa", "como está o processo X?", "desenha um
  processo de devolução", "create a task", "pending approvals", "draft a process". Do NOT use for
  sales or finance analysis.
---

# Tarefas e processos

Responda no idioma do usuário (padrão: português do Brasil).

## Ler

- Tarefas: `list_tasks`, `get_task`, `list_task_projects`, `list_task_templates`.
- Aprovações pendentes: `list_approvals`.
- Processos e execuções: `list_processes`, `get_process`, `list_process_runs`, `get_process_run`.

## Escrever — sempre prévia, depois envio

Antes de qualquer escrita, confira o modo da conexão (`discover` sem seção). `read_only` não escreve.

**Tarefas** (receita: `discover(section="workflows", key="tasks.manage_tasks")`):

1. Para atribuir a alguém pelo nome, leia `get_workspace_administration(object="members")` e use o
   `members[].user_id` como `assignee_id` — nunca `members[].key`. Só membros `enabled`. Nomes
   repetidos: pergunte qual (e-mail, papel). Não adivinhe ids.
2. Criar: `preview_create_task` → mostre o efeito ao usuário → `create_task`. Guarde a referência da
   operação; depois de timeout use `get_task_operation` em vez de criar de novo.
3. Alterar: `preview_task_change` → `update_task` / `complete_task` / `archive_task`. Se a tarefa mudou
   no meio, o recibo volta `stale_revision`: releia e proponha de novo.
4. Recibo `pending_approval` **não é feito**: diga onde aprovar no Imans.

**Rascunhos de processo** (receita: `discover(section="workflows", key="process_drafts.author_draft")`):

1. `preview_create_process_draft` → leia todos os achados → `create_process_draft`. Entradas de
   negócio em aberto são permitidas; erros estruturais não.
2. Editar: `preview_update_process_draft` com a revisão lida → `update_process_draft`. Conflito: recarregue,
   mescle e reenvie — nunca sobrescreva. `validate_process_draft` revalida.
3. Entregue o link do Process Studio: ensaio e ativação acontecem lá, nunca por aqui.

Nunca descreva uma ação pendente como concluída.
