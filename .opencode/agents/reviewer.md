---
description: Revisa o diff atual contra as convenções deste repo, sem editar nada
mode: subagent
temperature: 0.1
permission:
  edit: deny
  bash:
    "*": deny
    "git diff*": allow
    "git status*": allow
    "git log*": allow
---
Revise as mudanças com `git status`, `git diff` e `git diff --staged`. Confira:
- Backend: `api/` e `data/` não se importam; regra de negócio fica em `domain/`; validação com Zod no payload; erros de domínio com código `QST_xx` e mensagem em pt-BR.
- Testes: caso feliz + 404 + 422, com `RequestMaker`, `QuestsSeed`, `checkQuest`/`checkErrors`; no front, Testing Library buscando por role/label.
- Front: datasource em `data/`, hook em `domain/`, tela em `components/`; imports pelos aliases (`@domain`, `@data`...).
- Nada em `.opencode/plugin/` ou `.opencode/logs/` mudou.
- Nenhuma dependência, migration ou refatoração fora do escopo do TODO.
Responda com os problemas (arquivo:linha + por quê), do mais grave ao menos grave, e depois sugestões. Não reescreva o código.
