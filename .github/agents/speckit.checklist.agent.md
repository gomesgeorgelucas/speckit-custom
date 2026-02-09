---
description: Generate a custom checklist for the current feature based on user requirements.
---

## Propósito do Checklist: "Testes Unitários para o Texto"

**CONCEITO CRÍTICO**: Checklists são **TESTES UNITÁRIOS PARA A ESCRITA DE REQUISITOS** - eles validam a qualidade, clareza e integridade dos requisitos em um determinado domínio.

**NÃO é para verificação/teste de software**:

- ❌ NÃO é "Verificar se o botão clica corretamente"
- ❌ NÃO é "Testar se o tratamento de erro funciona"
- ❌ NÃO é "Confirmar se a API retorna 200"
- ❌ NÃO é verificar se o código/implementação corresponde à spec

**É PARA validação da qualidade dos requisitos**:

- ✅ "Os requisitos de hierarquia visual estão definidos para todos os tipos de cartões?" (integridade)
- ✅ "A 'exibição proeminente' está quantificada com tamanho/posicionamento específicos?" (clareza)
- ✅ "Os requisitos de estado de hover são consistentes em todos os elementos interativos?" (consistência)
- ✅ "Os requisitos de acessibilidade estão definidos para navegação por teclado?" (cobertura)
- ✅ "A spec define o que acontece quando a imagem do logo falha ao carregar?" (casos de borda)

**Metáfora**: Se a sua spec é um código escrito em linguagem natural, o checklist é a sua suíte de testes unitários. Você está testando se os requisitos estão bem escritos, completos, inequívocos e prontos para a implementação - NÃO se a implementação funciona.

## User Input

```text
$ARGUMENTS
```

Você **DEVE** considerar o input do usuário antes de prosseguir (se não estiver vazio).

## Passos de Execução

1. **Configuração**: Execute `.specify/scripts/powershell/check-prerequisites.ps1 -Json` a partir da raiz do repositório e analise o JSON para obter FEATURE_DIR e a lista AVAILABLE_DOCS.
   - Todos os caminhos de arquivos devem ser absolutos.
   - Para aspas simples em argumentos como "I'm Groot", use a sintaxe de escape: ex: 'I'\''m Groot' (ou aspas duplas se possível: "I'm Groot").

2. **Clarificar intenção (dinâmico)**: Derive até TRÊS perguntas iniciais de esclarecimento contextual (sem catálogo pré-definido). Elas DEVEM:
   - Ser geradas a partir da frase do usuário + sinais extraídos de spec/plan/tasks.
   - Perguntar apenas sobre informações que alterem materialmente o conteúdo do checklist.
   - Ser puladas individualmente se já estiverem claras em `$ARGUMENTS`.
   - Preferir precisão em vez de amplitude.

   Algoritmo de geração:
   1. Extrair sinais: palavras-chave do domínio da funcionalidade (ex: auth, latência, UX, API), indicadores de risco ("crítico", "obrigatório", "conformidade"), dicas de stakeholders ("QA", "revisão", "equipe de segurança") e entregáveis explícitos ("a11y", "rollback", "contratos").
   2. Agrupar sinais em áreas de foco candidatas (máx. 4) classificadas por relevância.
   3. Identificar público provável e timing (autor, revisor, QA, release) se não for explícito.
   4. Detectar dimensões ausentes: amplitude do escopo, profundidade/rigor, ênfase no risco, limites de exclusão, critérios de aceitação mensuráveis.
   5. Formular perguntas escolhidas destes arquétipos:
      - Refinamento de escopo (ex: "Isso deve incluir pontos de contato de integração com X e Y ou limitar-se à correção do módulo local?")
      - Priorização de risco (ex: "Qual destas áreas de risco potencial deve receber verificações de bloqueio obrigatórias?")
      - Calibração de profundidade (ex: "Esta é uma lista leve de sanidade pré-commit ou um portão formal de release?")
      - Enquadramento do público (ex: "Isso será usado apenas pelo autor ou por pares durante a revisão de PR?")
      - Exclusão de limites (ex: "Devemos excluir explicitamente itens de ajuste de performance nesta rodada?")
      - Lacuna de classe de cenário (ex: "Nenhum fluxo de recuperação detectado — caminhos de rollback / falha parcial estão no escopo?")

   Regras de formatação de perguntas:
   - Se apresentar opções, gere uma tabela compacta com as colunas: Opção | Candidato | Por que é Importante.
   - Limite a no máximo opções de A–E; omita a tabela se uma resposta de texto livre for mais clara.
   - Nunca peça ao usuário para repetir o que ele já disse.
   - Evite categorias especulativas (sem alucinações). Se estiver incerto, pergunte explicitamente: "Confirme se X pertence ao escopo."

   Padrões quando a interação for impossível:
   - Profundidade: Padrão (Standard)
   - Público: Revisor (PR) se for relacionado a código; Autor caso contrário.
   - Foco: Top 2 agrupamentos de relevância.

   Exiba as perguntas (rótulos Q1/Q2/Q3). Após as respostas: se ≥2 classes de cenário (Alternativo / Exceção / Recuperação / Domínio Não Funcional) permanecerem incertas, você PODE fazer até DUAS perguntas de acompanhamento direcionadas (Q4/Q5) com uma justificativa de uma linha cada (ex: "Risco de caminho de recuperação não resolvido"). Não exceda cinco perguntas no total. Pule a escalada se o usuário recusar explicitamente mais perguntas.

3. **Entender a solicitação do usuário**: Combine `$ARGUMENTS` + respostas de esclarecimento:
   - Derive o tema do checklist (ex: segurança, revisão, deploy, ux).
   - Consolide itens obrigatórios explícitos mencionados pelo usuário.
   - Mapeie as seleções de foco para a estrutura de categorias.
   - Infira qualquer contexto ausente de spec/plan/tasks (NÃO alucine).

4. **Carregar contexto da funcionalidade**: Leia de FEATURE_DIR:
   - spec.md: Requisitos e escopo da funcionalidade.
   - plan.md (se existir): Detalhes técnicos, dependências.
   - tasks.md (se existir): Tarefas de implementação.

   **Estratégia de Carregamento de Contexto**:
   - Carregue apenas as porções necessárias relevantes às áreas de foco ativas.
   - Prefira resumir seções longas em tópicos concisos de cenários/requisitos.
   - Use divulgação progressiva: adicione recuperação de dados subsequente apenas se lacunas forem detectadas.
   - Se os documentos de origem forem grandes, gere itens de resumo provisórios em vez de incorporar o texto bruto.

5. **Gerar checklist** - Crie "Testes Unitários para Requisitos":
   - Crie o diretório `FEATURE_DIR/checklists/` se ele não existir.
   - Gere um nome de arquivo de checklist único:
     - Use um nome curto e descritivo baseado no domínio (ex: `ux.md`, `api.md`, `security.md`).
     - Formato: `[dominio].md`.
     - Se o arquivo existir, anexe ao arquivo existente.
   - Numere os itens sequencialmente começando de CHK001.
   - Cada execução do `/speckit.checklist` cria um NOVO arquivo (nunca sobrescreve checklists existentes).

   **PRINCÍPIO FUNDAMENTAL - Teste os Requisitos, Não a Implementação**:
   Cada item do checklist DEVE avaliar os PRÓPRIOS REQUISITOS quanto a:
   - **Integridade (Completeness)**: Todos os requisitos necessários estão presentes?
   - **Clareza (Clarity)**: Os requisitos são específicos e inequívocos?
   - **Consistência (Consistency)**: Os requisitos estão alinhados entre si?
   - **Mensurabilidade (Measurability)**: Os requisitos podem ser verificados objetivamente?
   - **Cobertura (Coverage)**: Todos os cenários/casos de borda foram abordados?

   **Estrutura de Categorias** - Agrupe itens pelas dimensões de qualidade dos requisitos:
   - **Integridade dos Requisitos** (Todos os requisitos necessários estão documentados?)
   - **Clareza dos Requisitos** (Os requisitos são específicos e claros?)
   - **Consistência dos Requisitos** (Os requisitos se alinham sem conflitos?)
   - **Qualidade dos Critérios de Aceitação** (Os critérios de sucesso são mensuráveis?)
   - **Cobertura de Cenários** (Todos os fluxos/casos foram abordados?)
   - **Cobertura de Casos de Borda** (As condições de limite estão definidas?)
   - **Requisitos Não Funcionais** (Performance, Segurança, Acessibilidade, etc. - estão especificados?)
   - **Dependências e Suposições** (Estão documentadas e validadas?)
   - **Ambiguidades e Conflitos** (O que precisa de esclarecimento?)

   **COMO ESCREVER ITENS DE CHECKLIST - "Testes Unitários para o Texto"**:

   ❌ **ERRADO** (Testando a implementação):
   - "Verificar se a landing page exibe 3 cartões de episódios"
   - "Testar se os estados de hover funcionam no desktop"
   - "Confirmar se o clique no logo navega para a home"

   ✅ **CORRETO** (Testando a qualidade dos requisitos):
   - "O número exato e o layout dos episódios em destaque estão especificados?" [Integridade]
   - "A 'exibição proeminente' está quantificada com tamanho/posicionamento específicos?" [Clareza]
   - "Os requisitos de estado de hover são consistentes em todos os elementos interativos?" [Consistência]
   - "Os requisitos de navegação por teclado estão definidos para toda a UI interativa?" [Cobertura]
   - "O comportamento de fallback está especificado para quando a imagem do logo falha ao carregar?" [Casos de Borda]
   - "Os estados de carregamento estão definidos para os dados assíncronos de episódios?" [Integridade]
   - "A spec define a hierarquia visual para elementos de UI concorrentes?" [Clareza]

   **ESTRUTURA DO ITEM**:
   Cada item deve seguir este padrão:
   - Formato de pergunta questionando a qualidade do requisito.
   - Foco no que está ESCRITO (ou não escrito) na spec/plan.
   - Incluir a dimensão de qualidade entre colchetes [Integridade/Clareza/Consistência/etc.].
   - Referenciar a seção da spec `[Spec §X.Y]` ao verificar requisitos existentes.
   - Usar o marcador `[Lacuna]` quando verificar requisitos ausentes.

   **EXEMPLOS POR DIMENSÃO DE QUALIDADE**:

   Integridade:
   - "Os requisitos de tratamento de erro estão definidos para todos os modos de falha da API? [Lacuna]"
   - "Os requisitos de acessibilidade estão especificados para todos os elementos interativos? [Integridade]"
   - "Os requisitos de breakpoint mobile estão definidos para layouts responsivos? [Lacuna]"

   Clareza:
   - "O 'carregamento rápido' está quantificado com limiares de tempo específicos? [Clareza, Spec §NFR-2]"
   - "Os critérios de seleção de 'episódios relacionados' estão explicitamente definidos? [Clarity, Spec §FR-5]"
   - "O termo 'proeminente' está definido com propriedades visuais mensuráveis? [Ambiguidade, Spec §FR-4]"

   Consistência:
   - "Os requisitos de navegação estão alinhados em todas as páginas? [Consistência, Spec §FR-10]"
   - "Os requisitos do componente de cartão são consistentes entre as páginas de landing e de detalhes? [Consistência]"

   Cobertura:
   - "Existem requisitos definidos para cenários de estado zero (sem episódios)? [Cobertura, Caso de Borda]"
   - "Os cenários de interação simultânea de usuários são abordados? [Cobertura, Lacuna]"
   - "Os requisitos estão especificados para falhas parciais de carregamento de dados? [Cobertura, Fluxo de Exceção]"

   Mensurabilidade:
   - "Os requisitos de hierarquia visual são mensuráveis/testáveis? [Critérios de Aceitação, Spec §FR-1]"
   - "O 'peso visual equilibrado' pode ser verificado objetivamente? [Mensurabilidade, Spec §FR-2]"

   **Classificação e Cobertura de Cenários** (Foco na Qualidade dos Requisitos):
   - Verifique se existem requisitos para cenários: Primário, Alternativo, Exceção/Erro, Recuperação, Não Funcional.
   - Para cada classe de cenário, pergunte: "Os requisitos de [tipo de cenário] estão completos, claros e consistentes?"
   - Se a classe de cenário estiver ausente: "Os requisitos de [tipo de cenário] foram intencionalmente excluídos ou estão faltando? [Lacuna]"
   - Inclua resiliência/rollback quando ocorrer mutação de estado: "Os requisitos de rollback estão definidos para falhas de migração? [Lacuna]"

   **Requisitos de Rastreabilidade**:
   - MÍNIMO: ≥80% dos itens DEVEM incluir pelo menos uma referência de rastreabilidade.
   - Cada item deve referenciar: seção da spec `[Spec §X.Y]`, ou usar marcadores: `[Lacuna]`, `[Ambiguidade]`, `[Conflito]`, `[Suposição]`.
   - Se não existir sistema de IDs: "Um esquema de ID para requisitos e critérios de aceitação foi estabelecido? [Rastreabilidade]"

   **Expor e Resolver Problemas** (Problemas de Qualidade de Requisitos):
   Faça perguntas sobre os próprios requisitos:
   - Ambiguidades: "O termo 'rápido' está quantificado com métricas específicas? [Ambiguidade, Spec §NFR-1]"
   - Conflitos: "Os requisitos de navegação conflitam entre §FR-10 e §FR-10a? [Conflito]"
   - Suposições: "A suposição de 'API de podcast sempre disponível' foi validada? [Suposição]"
   - Dependências: "Os requisitos da API de podcast externa estão documentados? [Dependência, Lacuna]"
   - Definições ausentes: "A 'hierarquia visual' está definida com critérios mensuráveis? [Lacuna]"

   **Consolidação de Conteúdo**:
   - Limite suave: Se os itens candidatos brutos forem > 40, priorize por risco/impacto.
   - Mescle duplicatas próximas que verificam o mesmo aspecto do requisito.
   - Se houver >5 casos de borda de baixo impacto, crie um único item: "Os casos de borda X, Y, Z são abordados nos requisitos? [Cobertura]"

   **🚫 ABSOLUTAMENTE PROIBIDO** - Isso torna o teste de implementação e não de requisitos:
   - ❌ Qualquer item começando com "Verificar", "Testar", "Confirmar", "Checar" + comportamento de implementação.
   - ❌ Referências à execução de código, ações do usuário, comportamento do sistema.
   - ❌ "Exibe corretamente", "funciona propriamente", "funciona como esperado".
   - ❌ "Clicar", "navegar", "renderizar", "carregar", "executar".
   - ❌ Casos de teste, planos de teste, procedimentos de QA.
   - ❌ Detalhes de implementação (frameworks, APIs, algoritmos).

   **✅ PADRÕES OBRIGATÓRIOS** - Estes testam a qualidade dos requisitos:
   - ✅ "Os [tipo de requisito] estão definidos/especificados/documentados para [cenário]?"
   - ✅ "O [termo vago] está quantificado/esclarecido com critérios específicos?"
   - ✅ "Os requisitos são consistentes entre [seção A] e [seção B]?"
   - ✅ "O [requisito] pode ser medido/verificado objetivamente?"
   - ✅ "Os [casos de borda/cenários] são abordados nos requisitos?"
   - ✅ "A spec define [aspecto ausente]?"

6. **Referência de Estrutura**: Gere o checklist seguindo o template canônico em `.specify/templates/checklist-template.md` para título, seção de meta, cabeçalhos de categoria e formatação de ID. Se o template estiver indisponível, use: Título H1, linhas de meta de propósito/criação, seções de categoria `##` contendo linhas `- [ ] CHK### <item do requisito>` com IDs incrementados globalmente começando em CHK001.

7. **Relatório**: Exiba o caminho completo para o checklist criado, a contagem de itens e lembre o usuário de que cada execução cria um novo arquivo. Resuma:
   - Áreas de foco selecionadas.
   - Nível de profundidade.
   - Ator/timing.
   - Quaisquer itens obrigatórios explícitos especificados pelo usuário incorporados.

**Importante**: Cada invocação do comando `/speckit.checklist` cria um arquivo de checklist usando nomes curtos e descritivos, a menos que o arquivo já exista. Isso permite:

- Múltiplos checklists de diferentes tipos (ex: `ux.md`, `test.md`, `security.md`).
- Nomes de arquivos simples e memoráveis que indicam o propósito do checklist.
- Fácil identificação e navegação na pasta `checklists/`.

Para evitar bagunça, use tipos descritivos e limpe checklists obsoletos quando terminar.

## Exemplos de Tipos de Checklist e Itens de Amostra

**Qualidade dos Requisitos de UX:** `ux.md`

Itens de amostra (testando os requisitos, NÃO a implementação):

- "Os requisitos de hierarquia visual estão definidos com critérios mensuráveis? [Clareza, Spec §FR-1]"
- "O número e o posicionamento dos elementos de UI estão explicitamente especificados? [Integridade, Spec §FR-1]"
- "Os requisitos de estado de interação (hover, foco, ativo) estão definidos de forma consistente? [Consistência]"
- "Os requisitos de acessibilidade estão especificados para todos os elementos interativos? [Cobertura, Lacuna]"
- "O comportamento de fallback está definido para quando as imagens falham ao carregar? [Caso de Borda, Lacuna]"
- "A 'exibição proeminente' pode ser medida objetivamente? [Mensurabilidade, Spec §FR-4]"

**Qualidade dos Requisitos de API:** `api.md`

Itens de amostra:

- "Os formatos de resposta de erro estão especificados para todos os cenários de falha? [Integridade]"
- "Os requisitos de rate limiting estão quantificados com limiares específicos? [Clareza]"
- "Os requisitos de autenticação são consistentes em todos os endpoints? [Consistência]"
- "Os requisitos de retry/timeout estão definidos para dependências externas? [Cobertura, Lacuna]"
- "A estratégia de versionamento está documentada nos requisitos? [Lacuna]"

**Qualidade dos Requisitos de Performance:** `performance.md`

Itens de amostra:

- "Os requisitos de performance estão quantificados com métricas específicas? [Clareza]"
- "As metas de performance estão definidas para todas as jornadas críticas do usuário? [Cobertura]"
- "Os requisitos de performance sob diferentes condições de carga estão especificados? [Integridade]"
- "Os requisitos de performance podem ser medidos objetivamente? [Mensurabilidade]"
- "Os requisitos de degradação estão definidos para cenários de alta carga? [Caso de Borda, Lacuna]"

**Qualidade dos Requisitos de Segurança:** `security.md`

Itens de amostra:

- "Os requisitos de autenticação estão especificados para todos os recursos protegidos? [Cobertura]"
- "Os requisitos de proteção de dados estão definidos para informações sensíveis? [Integridade]"
- "O modelo de ameaça está documentado e os requisitos estão alinhados a ele? [Rastreabilidade]"
- "Os requisitos de segurança são consistentes com as obrigações de conformidade? [Consistência]"
- "Os requisitos de resposta a falhas/violações de segurança estão definidos? [Lacuna, Fluxo de Exceção]"

## Anti-Exemplos: O Que NÃO Fazer

**❌ ERRADO - Estes testam a implementação, não os requisitos:**

```markdown
- [ ] CHK001 - Verificar se a landing page exibe 3 cartões de episódios [Spec §FR-001]
- [ ] CHK002 - Testar se os estados de hover funcionam corretamente no desktop [Spec §FR-003]
- [ ] CHK003 - Confirmar se o clique no logo navega para a página inicial [Spec §FR-010]
- [ ] CHK004 - Checar se a seção de episódios relacionados mostra 3-5 itens [Spec §FR-005]
```

**✅ CORRETO - Estes testam a qualidade dos requisitos:**

```markdown
- [ ] CHK001 - O número e o layout dos episódios em destaque estão explicitamente especificados? [Integridade, Spec §FR-001]
- [ ] CHK002 - Os requisitos de estado de hover estão definidos de forma consistente para todos os elementos interativos? [Consistência, Spec §FR-003]
- [ ] CHK003 - Os requisitos de navegação estão claros para todos os elementos de marca clicáveis? [Clareza, Spec §FR-010]
- [ ] CHK004 - O critério de seleção para episódios relacionados está documentado? [Lacuna, Spec §FR-005]
- [ ] CHK005 - Os requisitos de estado de carregamento estão definidos para dados assíncronos de episódios? [Lacuna]
- [ ] CHK006 - Os requisitos de "hierarquia visual" podem ser medidos objetivamente? [Mensurabilidade, Spec §FR-001]
```

**Principais Diferenças:**

- Errado: Testa se o sistema funciona corretamente.
- Correto: Testa se os requisitos estão escritos corretamente.
- Errado: Verificação de comportamento.
- Correto: Validação da qualidade do requisito.
- Errado: "Ele faz X?"
- Correto: "X está claramente especificado?"

## Contexto

$ARGUMENTS
