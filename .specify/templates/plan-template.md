---

description: "Template de lista de tarefas para implementação de funcionalidades"
---

# Tarefas: [NOME DA FUNCIONALIDADE]

**Entrada**: Documentos de design de `/specs/[###-nome-da-funcionalidade]/`
**Pré-requisitos**: plan.md (obrigatório), spec.md (obrigatório para histórias de usuário), research.md, data-model.md, contracts/

**Testes**: Os exemplos abaixo incluem tarefas de teste. Testes são OPCIONAIS - inclua-os apenas se solicitado explicitamente na especificação da funcionalidade.

**Organização**: As tarefas são agrupadas por história de usuário para permitir a implementação e testes independentes de cada história.

## Formato: `[ID] [P?] [Story] Descrição`

- **[P]**: Pode rodar em paralelo (arquivos diferentes, sem dependências)
- **[Story]**: A qual história de usuário esta tarefa pertence (ex: US1, US2, US3)
- Inclua caminhos de arquivo exatos nas descrições

## Convenções de Caminho

- **Projeto único**: `src/`, `tests/` na raiz do repositório
- **Web app**: `backend/src/`, `frontend/src/`
- **Mobile**: `api/src/`, `ios/src/` ou `android/src/`
- Os caminhos mostrados abaixo assumem projeto único - ajuste com base na estrutura do plan.md

<!-- 
  ============================================================================
  IMPORTANTE: As tarefas abaixo são TAREFAS DE EXEMPLO apenas para fins ilustrativos.
  
  O comando /speckit.tasks DEVE substituir estas tarefas por tarefas reais baseadas em:
  - Histórias de usuário do spec.md (com suas prioridades P1, P2, P3...)
  - Requisitos da funcionalidade do plan.md
  - Entidades do data-model.md
  - Endpoints de contracts/
  
  As tarefas DEVEM ser organizadas por história de usuário para que cada história possa ser:
  - Implementada independentemente
  - Testada independentemente
  - Entregue como um incremento de MVP
  
  NÃO mantenha estas tarefas de exemplo no arquivo tasks.md gerado.
  ============================================================================
-->

## Fase 1: Configuração (Infraestrutura Compartilhada)

**Propósito**: Inicialização do projeto e estrutura básica

- [ ] T001 Criar estrutura do projeto conforme o plano de implementação
- [ ] T002 Inicializar projeto [linguagem] com dependências [framework]
- [ ] T003 [P] Configurar ferramentas de linting e formatação

---

## Fase 2: Fundamental (Pré-requisitos Bloqueadores)

**Propósito**: Infraestrutura central que DEVE estar concluída antes que QUALQUER história de usuário possa ser implementada

**⚠️ CRÍTICO**: Nenhum trabalho de história de usuário pode começar até que esta fase esteja concluída

Exemplos de tarefas fundamentais (ajuste com base no seu projeto):

- [ ] T004 Configurar esquema de banco de dados e framework de migrações
- [ ] T005 [P] Implementar framework de autenticação/autorização
- [ ] T006 [P] Configurar roteamento de API e estrutura de middleware
- [ ] T007 Criar modelos/entidades base dos quais todas as histórias dependem
- [ ] T008 Configurar infraestrutura de tratamento de erros e logging
- [ ] T009 Configurar gerenciamento de configuração de ambiente

**Checkpoint**: Base pronta - a implementação das histórias de usuário pode agora começar em paralelo

---

## Fase 3: História de Usuário 1 - [Título] (Prioridade: P1) 🎯 MVP

**Objetivo**: [Breve descrição do que esta história entrega]

**Teste Independente**: [Como verificar se esta história funciona por conta própria]

### Testes para a História de Usuário 1 (OPCIONAL - apenas se testes solicitados) ⚠️

> **NOTA: Escreva estes testes PRIMEIRO, garanta que eles FALHEM antes da implementação**

- [ ] T010 [P] [US1] Teste de contrato para [endpoint] em tests/contract/test_[nome].py
- [ ] T011 [P] [US1] Teste de integração para [jornada do usuário] em tests/integration/test_[nome].py

### Implementação para a História de Usuário 1

- [ ] T012 [P] [US1] Criar modelo [Entidade1] em src/models/[entidade1].py
- [ ] T013 [P] [US1] Criar modelo [Entidade2] em src/models/[entidade2].py
- [ ] T014 [US1] Implementar [Serviço] em src/services/[servico].py (depende de T012, T013)
- [ ] T015 [US1] Implementar [endpoint/funcionalidade] em src/[local]/[arquivo].py
- [ ] T016 [US1] Adicionar validação e tratamento de erros
- [ ] T017 [US1] Adicionar logging para operações da história de usuário 1

**Checkpoint**: Neste ponto, a História de Usuário 1 deve estar totalmente funcional e testável de forma independente

---

## Fase 4: História de Usuário 2 - [Título] (Prioridade: P2)

**Objetivo**: [Breve descrição do que esta história entrega]

**Teste Independente**: [Como verificar se esta história funciona por conta própria]

### Testes para a História de Usuário 2 (OPCIONAL - apenas se testes solicitados) ⚠️

- [ ] T018 [P] [US2] Teste de contrato para [endpoint] em tests/contract/test_[nome].py
- [ ] T019 [P] [US2] Teste de integração para [jornada do usuário] em tests/integration/test_[nome].py

### Implementação para a História de Usuário 2

- [ ] T020 [P] [US2] Criar modelo [Entidade] em src/models/[entidade].py
- [ ] T021 [US2] Implementar [Serviço] em src/services/[servico].py
- [ ] T022 [US2] Implementar [endpoint/funcionalidade] em src/[local]/[arquivo].py
- [ ] T023 [US2] Integrar com componentes da História de Usuário 1 (se necessário)

**Checkpoint**: Neste ponto, as Histórias de Usuário 1 E 2 devem funcionar de forma independente

---

## Fase 5: História de Usuário 3 - [Título] (Prioridade: P3)

**Objetivo**: [Breve descrição do que esta história entrega]

**Teste Independente**: [Como verificar se esta história funciona por conta própria]

### Testes para a História de Usuário 3 (OPCIONAL - apenas se testes solicitados) ⚠️

- [ ] T024 [P] [US3] Teste de contrato para [endpoint] em tests/contract/test_[nome].py
- [ ] T025 [P] [US3] Teste de integração para [jornada do usuário] em tests/integration/test_[nome].py

### Implementação para a História de Usuário 3

- [ ] T026 [P] [US3] Criar modelo [Entidade] em src/models/[entidade].py
- [ ] T027 [US3] Implementar [Serviço] em src/services/[servico].py
- [ ] T028 [US3] Implementar [endpoint/funcionalidade] em src/[local]/[arquivo].py

**Checkpoint**: Todas as histórias de usuário devem agora ser funcionalmente independentes

---

[Adicione mais fases de história de usuário conforme necessário, seguindo o mesmo padrão]

---

## Fase N: Polimento e Preocupações Transversais

**Propósito**: Melhorias que afetam múltiplas histórias de usuário

- [ ] TXXX [P] Atualizações de documentação em docs/
- [ ] TXXX Limpeza de código e refatoração
- [ ] TXXX Otimização de performance em todas as histórias
- [ ] TXXX [P] Testes unitários adicionais (se solicitados) em tests/unit/
- [ ] TXXX Endurecimento de segurança (security hardening)
- [ ] TXXX Rodar validação de quickstart.md

---

## Dependências e Ordem de Execução

### Dependências de Fase

- **Configuração (Fase 1)**: Sem dependências - pode começar imediatamente
- **Fundamental (Fase 2)**: Depende da conclusão da Configuração - BLOQUEIA todas as histórias de usuário
- **Histórias de Usuário (Fase 3+)**: Todas dependem da conclusão da fase Fundamental
  - As histórias de usuário podem então prosseguir em paralelo (se houver equipe)
  - Ou sequencialmente em ordem de prioridade (P1 → P2 → P3)
- **Polimento (Fase Final)**: Depende da conclusão de todas as histórias de usuário desejadas

### Dependências de Histórias de Usuário

- **História de Usuário 1 (P1)**: Pode começar após a fase Fundamental (Fase 2) - Sem dependências de outras histórias
- **História de Usuário 2 (P2)**: Pode começar após a fase Fundamental (Fase 2) - Pode se integrar com a US1, mas deve ser testável de forma independente
- **História de Usuário 3 (P3)**: Pode começar após a fase Fundamental (Fase 2) - Pode se integrar com a US1/US2, mas deve ser testável de forma independente

### Dentro de Cada História de Usuário

- Testes (se incluídos) DEVEM ser escritos e FALHAR antes da implementação
- Modelos antes de serviços
- Serviços antes de endpoints
- Implementação core antes da integração
- História concluída antes de passar para a próxima prioridade

### Oportunidades de Paralelismo

- Todas as tarefas de Configuração marcadas com [P] podem rodar em paralelo
- Todas as tarefas Fundamentais marcadas com [P] podem rodar em paralelo (dentro da Fase 2)
- Uma vez concluída a fase Fundamental, todas as histórias de usuário podem começar em paralelo (se a capacidade da equipe permitir)
- Todos os testes para uma história de usuário marcados com [P] podem rodar em paralelo
- Modelos dentro de uma história marcados com [P] podem rodar em paralelo
- Diferentes histórias de usuário podem ser trabalhadas em paralelo por diferentes membros da equipe

---

## Exemplo de Paralelismo: História de Usuário 1

```bash
# Iniciar todos os testes para a História de Usuário 1 juntos (se testes solicitados):
Tarefa: "Teste de contrato para [endpoint] em tests/contract/test_[nome].py"
Tarefa: "Teste de integração para [jornada do usuário] em tests/integration/test_[nome].py"

# Iniciar todos os modelos para a História de Usuário 1 juntos:
Tarefa: "Criar modelo [Entidade1] em src/models/[entidade1].py"
Tarefa: "Criar modelo [Entidade2] em src/models/[entidade2].py"
```

---

## Estratégia de Implementação

### MVP Primeiro (Apenas História de Usuário 1)

1. Concluir Fase 1: Configuração
2. Concluir Fase 2: Fundamental (CRÍTICO - bloqueia todas as histórias)
3. Concluir Fase 3: História de Usuário 1
4. **PARAR e VALIDAR**: Testar a História de Usuário 1 independentemente
5. Implantar/demonstrar se estiver pronto

### Entrega Incremental

1. Concluir Configuração + Fundamental → Base pronta
2. Adicionar História de Usuário 1 → Testar independentemente → Implantar/Demo (MVP!)
3. Adicionar História de Usuário 2 → Testar independentemente → Implantar/Demo
4. Adicionar História de Usuário 3 → Testar independentemente → Implantar/Demo
5. Cada história adiciona valor sem quebrar as histórias anteriores

### Estratégia de Equipe Paralela

Com múltiplos desenvolvedores:

1. A equipe conclui Configuração + Fundamental em conjunto
2. Uma vez que a base fundamental esteja pronta:
   - Desenvolvedor A: História de Usuário 1
   - Desenvolvedor B: História de Usuário 2
   - Desenvolvedor C: História de Usuário 3
3. As histórias são concluídas e integradas independentemente

---

## Notas

- Tarefas [P] = arquivos diferentes, sem dependências
- O rótulo [Story] mapeia a tarefa para uma história de usuário específica para rastreabilidade
- Cada história de usuário deve ser concluível e testável de forma independente
- Verifique se os testes falham antes de implementar
- Faça commit após cada tarefa ou grupo lógico
- Pare em qualquer checkpoint para validar a história independentemente
- Evite: tarefas vagas, conflitos no mesmo arquivo, dependências entre histórias que quebrem a independência