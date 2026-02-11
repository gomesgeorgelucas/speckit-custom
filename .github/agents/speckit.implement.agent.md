---
description: Execute the implementation plan by processing and executing all tasks defined in tasks.md
---

## User Input

```text
$ARGUMENTS
```

Você **DEVE** considerar o input do usuário antes de prosseguir (se não estiver vazio).

## Outline

1. Execute o script de verificação de pré-requisitos a partir da raiz do repositório e analise o FEATURE_DIR e a lista AVAILABLE_DOCS. Todos os caminhos devem ser absolutos.
   - **Windows**: `powershell -File .specify/scripts/powershell/check-prerequisites.ps1 -Json -RequireTasks -IncludeTasks`
   - **macOS / Linux**: `bash .specify/scripts/bash/check-prerequisites.sh -Json -RequireTasks -IncludeTasks`
   - **Fallback**: `pwsh -File .specify/scripts/powershell/check-prerequisites.ps1 -Json -RequireTasks -IncludeTasks` se o Bash script estiver ausente e PowerShell Core estiver instalado
   Para aspas simples em argumentos como "I'm Groot", use a sintaxe de escape: ex: 'I'\''m Groot'.

2. **Verificar status dos checklists** (se FEATURE_DIR/checklists/ existir):
   - Escaneie todos os arquivos de checklist no diretório checklists/
   - Para cada checklist, conte:
     - Itens totais: Todas as linhas correspondentes a `- [ ]` ou `- [X]` ou `- [x]`
     - Itens concluídos: Linhas correspondentes a `- [X]` ou `- [x]`
     - Itens incompletos: Linhas correspondentes a `- [ ]`
   - Crie uma tabela de status:

     ```text
     | Checklist | Total | Concluídos | Incompletos | Status |
     |-----------|-------|------------|-------------|--------|
     | ux.md     | 12    | 12         | 0           | ✓ PASS |
     | test.md   | 8     | 5          | 3           | ✗ FAIL |
     | security.md | 6   | 6          | 0           | ✓ PASS |
     ```

   - Calcule o status geral:
     - **PASS**: Todos os checklists possuem 0 itens incompletos
     - **FAIL**: Um ou mais checklists possuem itens incompletos

   - **Se algum checklist estiver incompleto**:
     - Exiba a tabela com as contagens de itens incompletos
     - **PARE** e pergunte: "Alguns checklists estão incompletos. Deseja prosseguir com a implementação mesmo assim? (sim/não)"
     - Aguarde a resposta do usuário antes de continuar
     - Se o usuário disser "não", "espere" ou "pare", interrompa a execução
     - Se o usuário disser "sim", "prossiga" ou "continue", passe para a etapa 3

   - **Se todos os checklists estiverem concluídos**:
     - Exiba a tabela mostrando que todos os checklists passaram
     - Prossiga automaticamente para a etapa 3

3. Carregue e analise o contexto de implementação:
   - **OBRIGATÓRIO**: Leia tasks.md para a lista completa de tarefas e o plano de execução
   - **OBRIGATÓRIO**: Leia plan.md para a stack técnica, arquitetura e estrutura de arquivos
   - **SE EXISTIR**: Leia data-model.md para entidades e relacionamentos
   - **SE EXISTIR**: Leia contracts/ para especificações de API e requisitos de teste
   - **SE EXISTIR**: Leia research.md para decisões técnicas e restrições
   - **SE EXISTIR**: Leia quickstart.md para cenários de integração
   - **SE EXISTIREM ARTEFATOS DE ENGENHARIA REVERSA**: Se `.specify/memory/reverse-engineering/` existir, leia `architecture.md`, `code-structure.md`, `api-documentation.md`, `dependencies.md` e outros artefatos gerados. Use esses artefatos para validar as tarefas em `tasks.md` (por exemplo, confirmar caminhos de arquivos, nomes de pacotes, contratos), preencher lacunas no plano de execução e detectar tarefas redundantes ou já concluídas. Se houver discrepâncias significativas entre `tasks.md` e os artefatos de engenharia reversa, alerte o usuário e recomende regenerar `tasks.md` usando `/speckit.tasks`.

4. **Verificação de Configuração do Projeto**:
   - **OBRIGATÓRIO**: Crie/verifique arquivos de ignore com base na configuração real do projeto:

   **Lógica de Detecção e Criação**:
   - Verifique se o seguinte comando tem sucesso para determinar se o repositório é um repositório git (crie/verifique .gitignore se sim):

     ```sh
     git rev-parse --git-dir 2>/dev/null
     ```

   - Verifique se Dockerfile* existe ou Docker em plan.md → crie/verifique .dockerignore
   - Verifique se .eslintrc* existe → crie/verifique .eslintignore
   - Verifique se eslint.config.* existe → garanta que as entradas `ignores` da config cubram os padrões necessários
   - Verifique se .prettierrc* existe → crie/verifique .prettierignore
   - Verifique se .npmrc ou package.json existe → crie/verifique .npmignore (se for publicar)
   - Verifique se arquivos terraform (*.tf) existem → crie/verifique .terraformignore
   - Verifique se .helmignore é necessário (helm charts presentes) → crie/verifique .helmignore

   **Se o arquivo ignore já existir**: Verifique se contém padrões essenciais, anexe apenas padrões críticos ausentes
   **Se o arquivo ignore estiver faltando**: Crie com o conjunto completo de padrões para a tecnologia detectada

   **Padrões Comuns por Tecnologia** (da stack técnica no plan.md):
   - **Node.js/JavaScript/TypeScript**: `node_modules/`, `dist/`, `build/`, `*.log`, `.env*`
   - **Python**: `__pycache__/`, `*.pyc`, `.venv/`, `venv/`, `dist/`, `*.egg-info/`
   - **Java**: `target/`, `*.class`, `*.jar`, `.gradle/`, `build/`
   - **C#/.NET**: `bin/`, `obj/`, `*.user`, `*.suo`, `packages/`
   - **Go**: `*.exe`, `*.test`, `vendor/`, `*.out`
   - **Ruby**: `.bundle/`, `log/`, `tmp/`, `*.gem`, `vendor/bundle/`
   - **PHP**: `vendor/`, `*.log`, `*.cache`, `*.env`
   - **Rust**: `target/`, `debug/`, `release/`, `*.rs.bk`, `*.rlib`, `*.prof*`, `.idea/`, `*.log`, `.env*`
   - **Kotlin**: `build/`, `out/`, `.gradle/`, `.idea/`, `*.class`, `*.jar`, `*.iml`, `*.log`, `.env*`
   - **C++**: `build/`, `bin/`, `obj/`, `out/`, `*.o`, `*.so`, `*.a`, `*.exe`, `*.dll`, `.idea/`, `*.log`, `.env*`
   - **C**: `build/`, `bin/`, `obj/`, `out/`, `*.o`, `*.a`, `*.so`, `*.exe`, `Makefile`, `config.log`, `.idea/`, `*.log`, `.env*`
   - **Swift**: `.build/`, `DerivedData/`, `*.swiftpm/`, `Packages/`
   - **R**: `.Rproj.user/`, `.Rhistory`, `.RData`, `.Ruserdata`, `*.Rproj`, `packrat/`, `renv/`
   - **Universal**: `.DS_Store`, `Thumbs.db`, `*.tmp`, `*.swp`, `.vscode/`, `.idea/`

   **Padrões Específicos de Ferramentas**:
   - **Docker**: `node_modules/`, `.git/`, `Dockerfile*`, `.dockerignore`, `*.log*`, `.env*`, `coverage/`
   - **ESLint**: `node_modules/`, `dist/`, `build/`, `coverage/`, `*.min.js`
   - **Prettier**: `node_modules/`, `dist/`, `build/`, `coverage/`, `package-lock.json`, `yarn.lock`, `pnpm-lock.yaml`
   - **Terraform**: `.terraform/`, `*.tfstate*`, `*.tfvars`, `.terraform.lock.hcl`
   - **Kubernetes/k8s**: `*.secret.yaml`, `secrets/`, `.kube/`, `kubeconfig*`, `*.key`, `*.crt`

5. Analise a estrutura do tasks.md e extraia:
   - **Fases das tarefas**: Setup, Testes, Core, Integração, Polimento
   - **Dependências de tarefas**: Regras de execução sequencial vs paralela
   - **Detalhes das tarefas**: ID, descrição, caminhos de arquivo, marcadores paralelos [P]
   - **Fluxo de execução**: Ordem e requisitos de dependência

6. Execute a implementação seguindo o plano de tarefas:
   - **Execução fase a fase**: Conclua cada fase antes de passar para a próxima
   - **Respeite as dependências**: Execute tarefas sequenciais em ordem, tarefas paralelas [P] podem rodar juntas
   - **Siga a abordagem TDD**: Execute as tarefas de teste antes das tarefas de implementação correspondentes
   - **Coordenação baseada em arquivos**: Tarefas que afetam os mesmos arquivos devem rodar sequencialmente
   - **Pontos de verificação de validação**: Verifique a conclusão de cada fase antes de prosseguir

7. Regras de execução da implementação:
   - **Setup primeiro**: Inicialize a estrutura do projeto, dependências, configuração
   - **Testes antes do código**: Se você precisar escrever testes para contratos, entidades e cenários de integração
   - **Desenvolvimento do Core**: Implemente modelos, serviços, comandos CLI, endpoints
   - **Trabalho de integração**: Conexões de banco de dados, middleware, logging, serviços externos
   - **Polimento e validação**: Testes unitários, otimização de performance, documentação

8. Rastreamento de progresso e tratamento de erros:
   - Relate o progresso após cada tarefa concluída
   - Interrompa a execução se qualquer tarefa não paralela falhar
   - Para tarefas paralelas [P], continue com as tarefas bem-sucedidas e relate as que falharam
   - Forneça mensagens de erro claras com contexto para depuração
   - Sugira os próximos passos se a implementação não puder prosseguir
   - **IMPORTANTE** Para tarefas concluídas, certifique-se de marcar a tarefa com [X] no arquivo de tarefas.

9. Validação de conclusão:
   - Verifique se todas as tarefas exigidas foram concluídas
   - Verifique se as funcionalidades implementadas correspondem à especificação original
   - Valide se os testes passam e se a cobertura atende aos requisitos
   - Confirme se a implementação segue o plano técnico
   - Relate o status final com um resumo do trabalho concluído

Nota: Este comando assume que um detalhamento completo das tarefas existe em tasks.md. Se as tarefas estiverem incompletas ou ausentes, sugira executar `/speckit.tasks` primeiro para regenerar a lista de tarefas.