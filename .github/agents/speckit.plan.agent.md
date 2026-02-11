---
description: Execute the implementation planning workflow using the plan template to generate design artifacts.
handoffs: 
  - label: Criar Tarefas
    agent: speckit.tasks
    prompt: Break the plan into tasks
    send: true
  - label: Criar Checklist
    agent: speckit.checklist
    prompt: Create a checklist for the following domain...
---

## User Input

```text
$ARGUMENTS
```

Você **DEVE** considerar o input do usuário antes de prosseguir (se não estiver vazio).

## Outline

1. **Configuração**: Execute o script de setup a partir da raiz do repositório e analise o JSON para FEATURE_SPEC, IMPL_PLAN, SPECS_DIR, BRANCH.
   - **Windows**: `powershell -File .specify/scripts/powershell/setup-plan.ps1 -Json` ou `pwsh -File .specify/scripts/powershell/setup-plan.ps1 -Json`
   - **macOS / Linux**: `bash .specify/scripts/bash/setup-plan.sh -Json`
   - **Fallback**: Se o script Bash não existir, `pwsh -File .specify/scripts/powershell/setup-plan.ps1 -Json` pode ser usado em sistemas com PowerShell Core instalado.
   Para aspas simples em argumentos como "I'm Groot", use a sintaxe de escape: ex: 'I'\''m Groot'.

2. **Carregar contexto**: Leia FEATURE_SPEC e `.specify/memory/constitution.md`. Carregue o template IMPL_PLAN (já copiado).
   - **Se existirem artefatos de engenharia reversa** em `.specify/memory/reverse-engineering/` (ex: `architecture.md`, `code-structure.md`, `api-documentation.md`), carregue-os e incorpore suas descobertas ao plano de implementação (componentes, dependências, arquitetura detectada). Dê prioridade às informações desses artefatos para preencher o Contexto Técnico e o Modelo de Dados.

3. **Executar fluxo de plano**: Siga a estrutura no template IMPL_PLAN para:
   - Preencher o Contexto Técnico (marque os desconhecidos como "NEEDS CLARIFICATION")
   - Preencher a seção Constitution Check a partir da constituição
   - Avaliar os gates (ERRO se houver violações injustificadas)
   - Fase 0: Gerar research.md (resolver todos os NEEDS CLARIFICATION)
   - Fase 1: Gerar data-model.md, contracts/, quickstart.md
   - Fase 1: Atualizar o contexto do agente executando o script do agente
   - Reavaliar o Constitution Check pós-design

4. **Parar e relatar**: O comando termina após o planejamento da Fase 2. Relate a branch, o caminho do IMPL_PLAN e os artefatos gerados.

## Fases

### Fase 0: Esboço e Pesquisa

1. **Extrair desconhecidos do Contexto Técnico** acima:
   - Para cada NEEDS CLARIFICATION → tarefa de pesquisa
   - Para cada dependência → tarefa de melhores práticas
   - Para cada integração → tarefa de padrões

2. **Gerar e despachar agentes de pesquisa**:

   ```text
   Para cada desconhecido no Contexto Técnico:
     Tarefa: "Pesquisar {unknown} para {feature context}"
   Para cada escolha de tecnologia:
     Tarefa: "Encontrar melhores práticas para {tech} no domínio {domain}"
   ```

3. **Consolidar achados** em `research.md` usando o formato:
   - Decisão: [o que foi escolhido]
   - Racional: [por que foi escolhido]
   - Alternativas consideradas: [o que mais foi avaliado]

**Resultado**: research.md com todos os NEEDS CLARIFICATION resolvidos

### Fase 1: Design e Contratos

**Pré-requisitos:** `research.md` concluído

1. **Extrair entidades da spec da funcionalidade** → `data-model.md`:
   - Nome da entidade, campos, relacionamentos
   - Regras de validação dos requisitos
   - Transições de estado, se aplicável

2. **Gerar contratos de API** a partir dos requisitos funcionais:
   - Para cada ação do usuário → endpoint
   - Use padrões REST/GraphQL padrão
   - Produza o esquema OpenAPI/GraphQL em `/contracts/`

3. **Atualização do contexto do agente**:
    - **Atualizar o contexto do agente**:
       - **Windows**: `powershell -File .specify/scripts/powershell/update-agent-context.ps1 -AgentType copilot`
       - **macOS / Linux**: `bash .specify/scripts/bash/update-agent-context.sh -AgentType copilot`
       - **Fallback**: `pwsh -File .specify/scripts/powershell/update-agent-context.ps1 -AgentType copilot` quando PowerShell Core estiver disponível em macOS/Linux
    - Estes scripts detectam qual agente de IA está em uso
   - Atualize o arquivo de contexto específico do agente apropriado
   - Adicione apenas tecnologia nova do plano atual
   - Preserve adições manuais entre os marcadores

**Resultado**: data-model.md, /contracts/*, quickstart.md, arquivo específico do agente

## Regras principais

- Use caminhos absolutos
- ERRO em falhas de gate ou esclarecimentos não resolvidos