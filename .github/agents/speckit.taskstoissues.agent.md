---
description: Convert existing tasks into actionable, dependency-ordered GitHub issues for the feature based on available design artifacts.
tools: ['github/github-mcp-server/issue_write']
---

## User Input

```text
$ARGUMENTS
```

Você **DEVE** considerar o input do usuário antes de prosseguir (se não estiver vazio).

## Outline

1. Execute o script de verificação de pré-requisitos a partir da raiz do repositório e analise o FEATURE_DIR e a lista AVAILABLE_DOCS. Todos os caminhos devem ser absolutos.
	- **Windows**: `powershell -File .specify/scripts/powershell/check-prerequisites.ps1 -Json -RequireTasks -IncludeTasks` ou `pwsh -File .specify/scripts/powershell/check-prerequisites.ps1 -Json -RequireTasks -IncludeTasks`
	- **macOS / Linux**: `bash .specify/scripts/bash/check-prerequisites.sh -Json -RequireTasks -IncludeTasks`
	- **Fallback**: `pwsh -File .specify/scripts/powershell/check-prerequisites.ps1 -Json -RequireTasks -IncludeTasks` se o script Bash não existir e PowerShell Core estiver instalado
	Para aspas simples em argumentos como "I'm Groot", use a sintaxe de escape: ex: 'I'\''m Groot' (ou aspas duplas se possível: "I'm Groot").
2. A partir do script executado, extraia o caminho para **tasks**.
3. Obtenha o remote do Git executando:

```bash
git config --get remote.origin.url
```

> [!CAUTION]
> PROSSIGA PARA OS PRÓXIMOS PASSOS APENAS SE O REMOTE FOR UMA URL DO GITHUB

4. Para cada tarefa na lista, use o GitHub MCP server para criar uma nova issue no repositório que seja representativa do remote do Git.

> [!CAUTION]
> SOB NENHUMA CIRCUNSTÂNCIA CRIE ISSUES EM REPOSITÓRIOS QUE NÃO CORRESPONDAM À URL DO REMOTE