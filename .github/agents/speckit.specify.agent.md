---
description: Create or update the feature specification from a natural language feature description.
handoffs: 
  - label: Construir Plano Técnico
    agent: speckit.plan
    prompt: Create a plan for the spec. I am building with...
  - label: Clarificar Requisitos da Spec
    agent: speckit.clarify
    prompt: Clarify specification requirements
    send: true
---

## User Input

```text
$ARGUMENTS
```

Você **DEVE** considerar o input do usuário antes de prosseguir (se não estiver vazio).

## Outline

O texto que o usuário digitou após `/speckit.specify` na mensagem de ativação **é** a descrição da funcionalidade. Assuma que você sempre a tem disponível nesta conversa, mesmo que `$ARGUMENTS` apareça literalmente abaixo. Não peça ao usuário para repeti-la, a menos que ele tenha fornecido um comando vazio.

Dada essa descrição da funcionalidade, faça o seguinte:

1. **Gere um nome curto conciso** (2 a 4 palavras) para a branch:
   - Analise a descrição da funcionalidade e extraia as palavras-chave mais significativas
   - Crie um nome curto de 2 a 4 palavras que capture a essência da funcionalidade
   - Use o formato ação-substantivo quando possível (ex: "add-user-auth", "fix-payment-bug")
   - Preserve termos técnicos e acrônimos (OAuth2, API, JWT, etc.)
   - Mantenha-o conciso, mas descritivo o suficiente para entender a funcionalidade num relance
   - Exemplos:
     - "I want to add user authentication" → "user-auth"
     - "Implement OAuth2 integration for the API" → "oauth2-api-integration"
     - "Create a dashboard for analytics" → "analytics-dashboard"
     - "Fix payment processing timeout bug" → "fix-payment-timeout"

2. **Verifique branches existentes antes de criar uma nova**:

   a. Primeiro, busque todas as branches remotas para garantir que temos as informações mais recentes:

      ```bash
      git fetch --all --prune
      ```

   b. Encontre o número de funcionalidade mais alto em todas as fontes para o nome curto:
      - Branches remotas: `git ls-remote --heads origin | grep -E 'refs/heads/[0-9]+-<short-name>$'`
      - Branches locais: `git branch | grep -E '^[* ]*[0-9]+-<short-name>$'`
      - Diretórios de specs: Verifique diretórios que correspondam a `specs/[0-9]+-<short-name>`

   c. Determine o próximo número disponível:
      - Extraia todos os números das três fontes
      - Encontre o número mais alto N
      - Use N+1 para o novo número da branch

   d. Execute o script `.specify/scripts/powershell/create-new-feature.ps1 -Json "$ARGUMENTS"` com o número e nome curto calculados:
      - Passe `--number N+1` e `--short-name "seu-nome-curto"` junto com a descrição da funcionalidade
      - Exemplo Bash: `.specify/scripts/powershell/create-new-feature.ps1 -Json "$ARGUMENTS" --json --number 5 --short-name "user-auth" "Add user authentication"`
      - Exemplo PowerShell: `.specify/scripts/powershell/create-new-feature.ps1 -Json "$ARGUMENTS" -Json -Number 5 -ShortName "user-auth" "Add user authentication"`

   **IMPORTANTE**:
   - Verifique todas as três fontes (branches remotas, branches locais, diretórios de specs) para encontrar o número mais alto
   - Combine apenas branches/diretórios com o padrão exato de nome curto
   - Se nenhuma branch/diretório existente for encontrado com este nome curto, comece com o número 1
   - Você deve executar este script apenas uma vez por funcionalidade
   - O JSON é fornecido no terminal como saída — consulte-o sempre para obter o conteúdo real que você está procurando
   - A saída JSON conterá os caminhos BRANCH_NAME e SPEC_FILE
   - Para aspas simples em argumentos como "I'm Groot", use a sintaxe de escape: ex: 'I'\''m Groot'.

3. Carregue `.specify/templates/spec-template.md` para entender as seções obrigatórias.

4. Siga este fluxo de execução:

    1. Analise a descrição do usuário do Input
       Se vazio: ERRO "Nenhuma descrição de funcionalidade fornecida"
    2. Extraia os conceitos-chave da descrição
       Identifique: atores, ações, dados, restrições
    3. Para aspectos não claros:
       - Faça suposições informadas com base no contexto e nos padrões da indústria
       - Marque apenas com [NEEDS CLARIFICATION: pergunta específica] se:
         - A escolha impactar significativamente o escopo da funcionalidade ou a experiência do usuário
         - Existirem múltiplas interpretações razoáveis com implicações diferentes
         - Não existir um padrão razoável
       - **LIMITE: Máximo de 3 marcadores [NEEDS CLARIFICATION] no total**
       - Priorize esclarecimentos por impacto: escopo > segurança/privacidade > experiência do usuário > detalhes técnicos
    4. Preencha a seção Cenários de Usuário e Testes
       Se não houver fluxo de usuário claro: ERRO "Não é possível determinar os cenários de usuário"
    5. Gere Requisitos Funcionais
       Cada requisito deve ser testável
       Use padrões razoáveis para detalhes não especificados (documente as suposições na seção Suposições)
    6. Defina Critérios de Sucesso
       Crie resultados mensuráveis e independentes de tecnologia
       Inclua métricas quantitativas (tempo, performance, volume) e medidas qualitativas (satisfação do usuário, conclusão de tarefas)
       Cada critério deve ser verificável sem detalhes de implementação
    7. Identifique Entidades-Chave (se houver dados envolvidos)
    8. Retorne: SUCESSO (spec pronta para o planejamento)

5. Grave a especificação em SPEC_FILE usando a estrutura do template, substituindo os placeholders por detalhes concretos derivados da descrição da funcionalidade (argumentos), preservando a ordem das seções e os títulos.

6. **Validação da Qualidade da Especificação**: Após escrever a spec inicial, valide-a em relação aos critérios de qualidade:

   a. **Criar Checklist de Qualidade da Spec**: Gere um arquivo de checklist em `FEATURE_DIR/checklists/requirements.md` usando a estrutura do template de checklist com estes itens de validação:

      ```markdown
      # Checklist de Qualidade da Especificação: [NOME DA FUNCIONALIDADE]
      
      **Propósito**: Validar a integridade e a qualidade da especificação antes de prosseguir para o planejamento
      **Criado**: [DATA]
      **Funcionalidade**: [Link para spec.md]
      
      ## Qualidade do Conteúdo
      
      - [ ] Sem detalhes de implementação (linguagens, frameworks, APIs)
      - [ ] Focado no valor para o usuário e nas necessidades do negócio
      - [ ] Escrito para stakeholders não técnicos
      - [ ] Todas as seções obrigatórias concluídas
      
      ## Integridade dos Requisitos
      
      - [ ] Nenhum marcador [NEEDS CLARIFICATION] restante
      - [ ] Requisitos são testáveis e inequívocos
      - [ ] Critérios de sucesso são mensuráveis
      - [ ] Critérios de sucesso são independentes de tecnologia (sem detalhes de implementação)
      - [ ] Todos os cenários de aceitação estão definidos
      - [ ] Casos de borda estão identificados
      - [ ] O escopo está claramente delimitado
      - [ ] Dependências e suposições identificadas
      
      ## Prontidão da Funcionalidade
      
      - [ ] Todos os requisitos funcionais possuem critérios de aceitação claros
      - [ ] Cenários de usuário cobrem os fluxos primários
      - [ ] A funcionalidade atende aos resultados mensuráveis definidos nos Critérios de Sucesso
      - [ ] Nenhum detalhe de implementação vaza para a especificação
      
      ## Notas
      
      - Itens marcados como incompletos exigem atualizações na spec antes do `/speckit.clarify` ou `/speckit.plan`
      ```

   b. **Executar Verificação de Validação**: Revise a spec em relação a cada item do checklist:
      - Para cada item, determine se ele passa ou falha
      - Documente problemas específicos encontrados (cite seções relevantes da spec)

   c. **Lidar com os Resultados da Validação**:

      - **Se todos os itens passarem**: Marque o checklist como concluído e prossiga para a etapa 6

      - **Se itens falharem (excluindo [NEEDS CLARIFICATION])**:
        1. Liste os itens que falharam e os problemas específicos
        2. Atualize a spec para abordar cada problema
        3. Execute a validação novamente até que todos os itens passem (máximo de 3 iterações)
        4. Se ainda estiver falhando após 3 iterações, documente os problemas restantes nas notas do checklist e avise o usuário

      - **Se marcadores [NEEDS CLARIFICATION] permanecerem**:
        1. Extraia todos os marcadores [NEEDS CLARIFICATION: ...] da spec
        2. **VERIFICAÇÃO DE LIMITE**: Se existirem mais de 3 marcadores, mantenha apenas os 3 mais críticos (por impacto de escopo/segurança/UX) e faça suposições informadas para o restante
        3. Para cada esclarecimento necessário (máximo de 3), apresente as opções ao usuário neste formato:

           ```markdown
           ## Pergunta [N]: [Tópico]
           
           **Contexto**: [Citar seção relevante da spec]
           
           **O que precisamos saber**: [Pergunta específica do marcador NEEDS CLARIFICATION]
           
           **Respostas Sugeridas**:
           
           | Opção | Resposta | Implicações |
           |-------|----------|-------------|
           | A     | [Primeira resposta sugerida] | [O que isso significa para a funcionalidade] |
           | B     | [Segunda resposta sugerida] | [O que isso significa para a funcionalidade] |
           | C     | [Terceira resposta sugerida] | [O que isso significa para a funcionalidade] |
           | Custom| Forneça sua própria resposta | [Explique como fornecer input personalizado] |
           
           **Sua escolha**: _[Aguardar resposta do usuário]_
           ```

        4. **CRÍTICO - Formatação da Tabela**: Garanta que as tabelas markdown estejam formatadas corretamente:
           - Use espaçamento consistente com pipes alinhados
           - Cada célula deve ter espaços ao redor do conteúdo: `| Conteúdo |` e não `|Conteúdo|`
           - O separador de cabeçalho deve ter pelo menos 3 hifens: `|--------|`
           - Teste se a tabela renderiza corretamente na visualização markdown
        5. Numere as perguntas sequencialmente (Q1, Q2, Q3 — máx. 3 no total)
        6. Apresente todas as perguntas juntas antes de aguardar as respostas
        7. Aguarde o usuário responder com suas escolhas para todas as perguntas (ex: "Q1: A, Q2: Custom - [detalhes], Q3: B")
        8. Atualize a spec substituindo cada marcador [NEEDS CLARIFICATION] pela resposta selecionada ou fornecida pelo usuário
        9. Execute a validação novamente após todos os esclarecimentos serem resolvidos

   d. **Atualizar Checklist**: Após cada iteração de validação, atualize o arquivo de checklist com o status atual de passa/falha.

7. Relate a conclusão com o nome da branch, o caminho do arquivo de spec, os resultados do checklist e a prontidão para a próxima fase (`/speckit.clarify` ou `/speckit.plan`).

**NOTA:** O script cria e faz o checkout da nova branch e inicializa o arquivo de spec antes de escrever.

## Diretrizes Gerais

## Diretrizes Rápidas

- Foque no **O QUE** os usuários precisam e **POR QUÊ**.
- Evite COMO implementar (sem stack técnica, APIs, estrutura de código).
- Escrito para stakeholders de negócios, não para desenvolvedores.
- NÃO crie checklists que estejam incorporados na spec. Isso será um comando separado.

### Requisitos das Seções

- **Seções obrigatórias**: Devem ser preenchidas para cada funcionalidade
- **Seções opcionais**: Inclua apenas quando relevante para a funcionalidade
- Quando uma seção não se aplicar, remova-a inteiramente (não deixe como "N/A")

### Para Geração por IA

Ao criar esta spec a partir de um prompt do usuário:

1. **Faça suposições informadas**: Use o contexto, os padrões da indústria e padrões comuns para preencher lacunas
2. **Documente suposições**: Registre os padrões razoáveis na seção Suposições
3. **Limite os esclarecimentos**: Máximo de 3 marcadores [NEEDS CLARIFICATION] — use apenas para decisões críticas que:
   - Impactem significativamente o escopo da funcionalidade ou a experiência do usuário
   - Tenham múltiplas interpretações razoáveis com implicações diferentes
   - Careçam de qualquer padrão razoável
4. **Priorize esclarecimentos**: escopo > segurança/privacidade > experiência do usuário > detalhes técnicos
5. **Pense como um testador**: Cada requisito vago deve reprovar no item de checklist "testável e inequívoco"
6. **Áreas comuns que precisam de esclarecimento** (apenas se não existir um padrão razoável):
   - Escopo e limites da funcionalidade (incluir/excluir casos de uso específicos)
   - Tipos de usuários e permissões (se múltiplas interpretações conflitantes forem possíveis)
   - Requisitos de segurança/conformidade (quando significativos legal/financeiramente)

**Exemplos de padrões razoáveis** (não pergunte sobre estes):

- Retenção de dados: Práticas padrão da indústria para o domínio
- Metas de performance: Expectativas padrão de apps web/mobile, a menos que especificado
- Tratamento de erros: Mensagens amigáveis ao usuário com fallbacks apropriados
- Método de autenticação: Baseado em sessão padrão ou OAuth2 para apps web
- Padrões de integração: APIs RESTful, a menos que especificado de outra forma

### Diretrizes para Critérios de Sucesso

Os critérios de sucesso devem ser:

1. **Mensuráveis**: Inclua métricas específicas (tempo, porcentagem, contagem, taxa)
2. **Independentes de tecnologia**: Sem menção a frameworks, linguagens, bancos de dados ou ferramentas
3. **Focados no usuário**: Descreva resultados da perspectiva do usuário/negócio, não internos do sistema
4. **Verificáveis**: Podem ser testados/validados sem conhecer detalhes de implementação

**Bons exemplos**:

- "Usuários podem concluir o checkout em menos de 3 minutos"
- "O sistema suporta 10.000 usuários simultâneos"
- "95% das buscas retornam resultados em menos de 1 segundo"
- "A taxa de conclusão de tarefas melhora em 40%"

**Maus exemplos** (focados na implementação):

- "O tempo de resposta da API é inferior a 200ms" (muito técnico, use "Usuários veem os resultados instantaneamente")
- "O banco de dados pode lidar com 1000 TPS" (detalhe de implementação, use métrica voltada para o usuário)
- "Componentes React renderizam eficientemente" (específico de framework)
- "Taxa de acerto do cache Redis acima de 80%" (específico de tecnologia)
