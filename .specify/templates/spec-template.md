# Especificação da Funcionalidade: [NOME DA FUNCIONALIDADE]

**Branch da Funcionalidade**: `[###-nome-da-funcionalidade]`  
**Criada em**: [DATA]  
**Status**: Rascunho  
**Entrada**: Descrição do usuário: "$ARGUMENTS"

## Cenários de Usuário e Testes *(obrigatório)*

<!--
  IMPORTANTE: As histórias de usuário devem ser PRIORIZADAS como jornadas de usuário ordenadas por importância.
  Cada história/jornada deve ser TESTÁVEL DE FORMA INDEPENDENTE - o que significa que se você implementar apenas UMA delas,
  você ainda deve ter um MVP (Produto Mínimo Viável) que entrega valor.
  
  Atribua prioridades (P1, P2, P3, etc.) a cada história, onde P1 é a mais crítica.
  Pense em cada história como uma fatia autônoma de funcionalidade que pode ser:
  - Desenvolvida independentemente
  - Testada independentemente
  - Implementada independentemente
  - Demonstrada aos usuários independentemente
-->

### História de Usuário 1 - [Título Breve] (Prioridade: P1)

[Descreva esta jornada do usuário em linguagem simples]

**Por que esta prioridade**: [Explique o valor e por que tem este nível de prioridade]

**Teste Independente**: [Descreva como isso pode ser testado de forma independente - ex: "Pode ser totalmente testado por [ação específica] e entrega [valor específico]"]

**Cenários de Aceitação**:

1. **Dado** [estado inicial], **Quando** [ação], **Então** [resultado esperado]
2. **Dado** [estado inicial], **Quando** [ação], **Então** [resultado esperado]

---

### História de Usuário 2 - [Título Breve] (Prioridade: P2)

[Descreva esta jornada do usuário em linguagem simples]

**Por que esta prioridade**: [Explique o valor e por que tem este nível de prioridade]

**Teste Independente**: [Descreva como isso pode ser testado de forma independente]

**Cenários de Aceitação**:

1. **Dado** [estado inicial], **Quando** [ação], **Então** [resultado esperado]

---

### História de Usuário 3 - [Título Breve] (Prioridade: P3)

[Descreva esta jornada do usuário em linguagem simples]

**Por que esta prioridade**: [Explique o valor e por que tem este nível de prioridade]

**Teste Independente**: [Descreva como isso pode ser testado de forma independente]

**Cenários de Aceitação**:

1. **Dado** [estado inicial], **Quando** [ação], **Então** [resultado esperado]

---

[Adicione mais histórias de usuário conforme necessário, cada uma com uma prioridade atribuída]

### Casos de Borda

<!--
  AÇÃO NECESSÁRIA: O conteúdo desta seção representa placeholders.
  Preencha-os com os casos de borda corretos.
-->

- O que acontece quando [condição de limite]?
- Como o sistema trata o [cenário de erro]?

## Requisitos *(obrigatório)*

<!--
  AÇÃO NECESSÁRIA: O conteúdo desta seção representa placeholders.
  Preencha-os com os requisitos funcionais corretos.
-->

### Requisitos Funcionais

- **FR-001**: O sistema DEVE [capacidade específica, ex: "permitir que usuários criem contas"]
- **FR-002**: O sistema DEVE [capacidade específica, ex: "validar endereços de e-mail"]  
- **FR-003**: Os usuários DEVEM ser capazes de [interação chave, ex: "redefinir sua senha"]
- **FR-004**: O sistema DEVE [requisito de dados, ex: "persistir preferências do usuário"]
- **FR-005**: O sistema DEVE [comportamento, ex: "registrar todos os eventos de segurança"]

*Exemplo de marcação de requisitos não claros:*

- **FR-006**: O sistema DEVE autenticar usuários via [NEEDS CLARIFICATION: método de autenticação não especificado - e-mail/senha, SSO, OAuth?]
- **FR-007**: O sistema DEVE reter os dados do usuário por [NEEDS CLARIFICATION: período de retenção não especificado]

### Entidades-Chave *(incluir se a funcionalidade envolver dados)*

- **[Entidade 1]**: [O que representa, atributos principais sem implementação]
- **[Entidade 2]**: [O que representa, relacionamentos com outras entidades]

## Critérios de Sucesso *(obrigatório)*

<!--
  AÇÃO NECESSÁRIA: Defina critérios de sucesso mensuráveis.
  Eles devem ser independentes de tecnologia e mensuráveis.
-->

### Resultados Mensuráveis

- **SC-001**: [Métrica mensurável, ex: "Usuários podem concluir a criação de conta em menos de 2 minutos"]
- **SC-002**: [Métrica mensurável, ex: "O sistema lida com 1000 usuários simultâneos sem degradação"]
- **SC-003**: [Métrica de satisfação do usuário, ex: "90% dos usuários concluem com sucesso a tarefa primária na primeira tentativa"]
- **SC-004**: [Métrica de negócio, ex: "Reduzir tickets de suporte relacionados a [X] em 50%"]