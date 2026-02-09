---
description: Generate an actionable, dependency-ordered tasks.md for the feature based on available design artifacts.
handoffs: 
  - label: Analisar Consistência
    agent: speckit.analyze
    prompt: Run a project analysis for consistency
    send: true
  - label: Implementar Projeto
    agent: speckit.implement
    prompt: Start the implementation in phases
    send: true
---

## User Input

```text
$ARGUMENTS
```

Você **DEVE** considerar o input do usuário antes de prosseguir (se não estiver vazio).

## Outline

1. **Configuração**: Execute `.specify/scripts/powershell/check-prerequisites.ps1 -Json` da raiz do repositório e analise o FEATURE_DIR e a lista AVAILABLE_DOCS. Todos os caminhos devem ser absolutos. Para aspas simples em argumentos como "I'm Groot", use a sintaxe de escape: ex: 'I'\''m Groot'.

2. **Carregar documentos de design**: Leia de FEATURE_DIR:
   - **Obrigatório**: plan.md (stack técnica, bibliotecas, estrutura), spec.md (histórias de usuário com prioridades)
   - **Opcional**: data-model.md (entidades), contracts/ (endpoints de API), research.md (decisões), quickstart.md (cenários de teste)
   - Nota: Nem todos os projetos possuem todos os documentos. Gere tarefas com base no que estiver disponível.

3. **Executar fluxo de geração de tarefas**:
   - Carregue o plan.md e extraia a stack técnica, bibliotecas e estrutura do projeto
   - Carregue o spec.md e extraia as histórias de usuário com suas prioridades (P1, P2, P3, etc.)
   - Se data-model.md existir: Extraia as entidades e mapeie para as histórias de usuário
   - Se contracts/ existir: Mapeie os endpoints para as histórias de usuário
   - Se research.md existir: Extraia as decisões para as tarefas de setup
   - Gere tarefas organizadas por história de usuário (veja Regras de Geração de Tarefas abaixo)
   - Gere o gráfico de dependências mostrando a ordem de conclusão das histórias de usuário
   - Crie exemplos de execução paralela por história de usuário
   - Valide a integridade das tarefas (cada história de usuário possui todas as tarefas necessárias, testáveis de forma independente)

4. **Gerar tasks.md**: Use `.specify/templates/tasks-template.md` como estrutura, preencha com:
   - Nome correto da funcionalidade do plan.md
   - Fase 1: Tarefas de Setup (inicialização do projeto)
   - Fase 2: Tarefas Fundamentais (pré-requisitos bloqueadores para todas as histórias de usuário)
   - Fase 3+: Uma fase por história de usuário (na ordem de prioridade do spec.md)
   - Cada fase inclui: objetivo da história, critérios de teste independentes, testes (se solicitados), tarefas de implementação
   - Fase Final: Polimento e preocupações transversais (cross-cutting concerns)
   - Todas as tarefas devem seguir rigorosamente o formato de checklist (veja Regras de Geração de Tarefas abaixo)
   - Caminhos de arquivo claros para cada tarefa
   - Seção de dependências mostrando a ordem de conclusão das histórias
   - Exemplos de execução paralela por história
   - Seção de estratégia de implementação (MVP primeiro, entrega incremental)

5. **Relato**: Exiba o caminho para o tasks.md gerado e o resumo:
   - Contagem total de tarefas
   - Contagem de tarefas por história de usuário
   - Oportunidades paralelas identificadas
   - Critérios de teste independentes para cada história
   - Escopo do MVP sugerido (normalmente apenas a História de Usuário 1)
   - Validação de formato: Confirme se TODAS as tarefas seguem o formato de checklist (checkbox, ID, rótulos, caminhos de arquivo)

Contexto para geração de tarefas: $ARGUMENTS

O tasks.md deve ser imediatamente executável — cada tarefa deve ser específica o suficiente para que um LLM possa concluí-la sem contexto adicional.

## Regras de Geração de Tarefas

**CRÍTICO**: As tarefas DEVEM ser organizadas por história de usuário para permitir implementação e testes independentes.

**Testes são OPCIONAIS**: Gere tarefas de teste apenas se solicitado explicitamente na especificação da funcionalidade ou se o usuário solicitar a abordagem TDD.

### Formato de Checklist (OBRIGATÓRIO)

Toda tarefa DEVE seguir rigorosamente este formato:

```text
- [ ] [TaskID] [P?] [Story?] Descrição com caminho de arquivo
```

**Componentes do Formato**:

1. **Checkbox**: Comece SEMPRE com `- [ ]` (checkbox markdown)
2. **ID da Tarefa**: Número sequencial (T001, T002, T003...) em ordem de execução
3. **Marcador [P]**: Inclua APENAS se a tarefa for paralelizável (arquivos diferentes, sem dependências de tarefas incompletas)
4. **Rótulo [Story]**: OBRIGATÓRIO apenas para tarefas de fases de história de usuário
   - Formato: [US1], [US2], [US3], etc. (mapeia para as histórias de usuário do spec.md)
   - Fase de setup: SEM rótulo de história
   - Fase fundamental: SEM rótulo de história
   - Fases de História de Usuário: DEVEM ter rótulo de história
   - Fase de polimento: SEM rótulo de história
5. **Descrição**: Ação clara com o caminho exato do arquivo

**Exemplos**:

- ✅ CORRETO: `- [ ] T001 Criar estrutura do projeto conforme plano de implementação`
- ✅ CORRETO: `- [ ] T005 [P] Implementar middleware de autenticação em src/middleware/auth.py`
- ✅ CORRETO: `- [ ] T012 [P] [US1] Criar modelo de Usuário em src/models/user.py`
- ✅ CORRETO: `- [ ] T014 [US1] Implementar UserService em src/services/user_service.py`
- ❌ ERRADO: `- [ ] Criar modelo de Usuário` (falta ID e rótulo de História)
- ❌ ERRADO: `T001 [US1] Criar modelo` (falta o checkbox)
- ❌ ERRADO: `- [ ] [US1] Criar modelo de Usuário` (falta o ID da Tarefa)
- ❌ ERRADO: `- [ ] T001 [US1] Criar modelo` (falta o caminho do arquivo)

### Organização de Tarefas

1. **Das Histórias de Usuário (spec.md)** - ORGANIZAÇÃO PRIMÁRIA:
   - Cada história de usuário (P1, P2, P3...) ganha sua própria fase
   - Mapeie todos os componentes relacionados à sua história:
     - Modelos necessários para essa história
     - Serviços necessários para essa história
     - Endpoints/UI necessários para essa história
     - Se testes forem solicitados: Testes específicos para essa história
   - Marque as dependências de histórias (a maioria das histórias deve ser independente)

2. **Dos Contratos**:
   - Mapeie cada contrato/endpoint → para a história de usuário que ele atende
   - Se testes forem solicitados: Cada contrato → tarefa de teste de contrato [P] antes da implementação na fase daquela história

3. **Do Modelo de Dados**:
   - Mapeie cada entidade para a(s) história(s) de usuário que precisam dela
   - Se a entidade atende a múltiplas histórias: Coloque na primeira história ou na fase de Setup
   - Relacionamentos → tarefas da camada de serviço na fase da história apropriada

4. **Do Setup/Infraestrutura**:
   - Infraestrutura compartilhada → Fase de Setup (Fase 1)
   - Tarefas fundamentais/bloqueadoras → Fase Fundamental (Fase 2)
   - Setup específico da história → dentro da fase daquela história

### Estrutura de Fases

- **Fase 1**: Setup (inicialização do projeto)
- **Fase 2**: Fundamental (pré-requisitos bloqueadores — DEVE concluir antes das histórias de usuário)
- **Fase 3+**: Histórias de Usuário em ordem de prioridade (P1, P2, P3...)
  - Dentro de cada história: Testes (se solicitados) → Modelos → Serviços → Endpoints → Integração
  - Cada fase deve ser um incremento completo e testável de forma independente
- **Fase Final**: Polimento e Preocupações Transversais