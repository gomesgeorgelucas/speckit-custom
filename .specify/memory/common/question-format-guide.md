# Guia de Formatação de Perguntas

## OBRIGATÓRIO: Todas as Perguntas Devem Usar Este Formato

### Regra: Nunca Faça Perguntas no Chat
**CRÍTICO**: Você NUNCA deve fazer perguntas diretamente no chat. TODAS as perguntas devem ser colocadas em arquivos de perguntas dedicados.

### Formato do Arquivo de Perguntas

#### Convenção de Nomenclatura de Arquivo
- Use nomes descritivos: `{nome-da-fase}-questions.md`
- Exemplos:
  - `classification-questions.md`
  - `requirements-questions.md`
  - `story-planning-questions.md`
  - `design-questions.md`

#### Estrutura da Pergunta
Cada pergunta deve incluir opções significativas mais "Outro" como a última opção:

```markdown
## Question [Número]
[Texto da pergunta claro e específico]

A) [Primeira opção significativa]
B) [Segunda opção significativa]
[...opções adicionais conforme necessário...]
X) Outro (por favor, descreva após a tag [Answer]: abaixo)

[Answer]: 
```

**CRÍTICO**: 
- "Outro" é OBRIGATÓRIO como a ÚLTIMA opção para cada pergunta.
- Inclua apenas opções significativas - não invente opções apenas para preencher espaços.
- Use quantas opções fizerem sentido (mínimo de 2 + Outro).

### Exemplo Completo

```markdown
# Perguntas de Esclarecimento de Requisitos

Por favor, responda às seguintes perguntas para ajudar a esclarecer os requisitos.

## Question 1
Qual é o método principal de autenticação de usuário?

A) Nome de usuário e senha
B) Login por rede social (Google, Facebook)
C) Single Sign-On (SSO)
D) Autenticação multifator
E) Outro (por favor, descreva após a tag [Answer]: abaixo)

[Answer]: 

## Question 2
Será uma aplicação web ou móvel?

A) Aplicação Web
B) Aplicação Móvel
C) Ambos (Web e Móvel)
D) Outro (por favor, descreva após a tag [Answer]: abaixo)

[Answer]: 

## Question 3
Este é um projeto novo ou uma base de código existente?

A) Projeto novo (greenfield)
B) Base de código existente (brownfield)
C) Outro (por favor, descreva após a tag [Answer]: abaixo)

[Answer]: 
```

### Formato de Resposta do Usuário
Os usuários responderão preenchendo a letra da escolha após a tag [Answer]:

```markdown
## Question 1
Qual é o método principal de autenticação de usuário?

A) Nome de usuário e senha
B) Login por rede social (Google, Facebook)
C) Single Sign-On (SSO)
D) Autenticação multifator

[Answer]: C
```

### Lendo as Respostas do Usuário
Após o usuário confirmar a conclusão:
1. Leia o arquivo de perguntas.
2. Extraia as respostas após as tags [Answer]:.
3. Valide se todas as perguntas foram respondidas.
4. Prossiga com a análise baseada nas respostas.

### Diretrizes de Múltipla Escolha

#### Contagem de Opções
- Mínimo: 2 opções significativas + "Outro" (A, B, C)
- Típico: 3-4 opções significativas + "Outro" (A, B, C, D, E)
- Máximo: 5 opções significativas + "Outro" (A, B, C, D, E, F)
- **CRÍTICO**: Não invente opções apenas para preencher espaços - inclua apenas escolhas significativas.

#### Qualidade da Opção
- Torne as opções mutuamente exclusivas.
- Cubra os cenários mais comuns.
- Inclua apenas opções significativas e realistas.
- **SEMPRE inclua "Outro" como a ÚLTIMA opção** (OBRIGATÓRIO).
- Seja específico e claro.
- **Não invente opções para preencher os slots A, B, C, D.**

#### Bom Exemplo:
```markdown
## Question 5
Qual tecnologia de banco de dados será usada?

A) Relacional (PostgreSQL, MySQL)
B) NoSQL Documento (MongoDB, DynamoDB)
C) NoSQL Chave-Valor (Redis, Memcached)
D) Banco de Dados de Grafo (Neo4j, Neptune)
E) Outro (por favor, descreva após a tag [Answer]: abaixo)

[Answer]: 
```

#### Mau Exemplo (Evite):
```markdown
## Question 5
Qual banco de dados você vai usar?

A) Sim
B) Não
C) Talvez

[Answer]: 
```

### Integração com o Fluxo de Trabalho

#### Passo 1: Criar Arquivo de Perguntas
```markdown
Criar aidlc-docs/{nome-da-fase}-questions.md com todas as perguntas
```

#### Passo 2: Informar o Usuário
```
"Eu criei o arquivo {nome-da-fase}-questions.md com [X] perguntas. 
Por favor, responda a cada pergunta preenchendo a letra da escolha após a tag [Answer]:. 
Se nenhuma das opções atender às suas necessidades, escolha a última opção (Outro) e descreva sua preferência. Avise-me quando terminar."
```

#### Passo 3: Aguardar Confirmação
Aguarde o usuário dizer "pronto", "concluído", "finalizado" ou similar.

#### Passo 4: Ler e Analisar
```
Ler aidlc-docs/{nome-da-fase}-questions.md
Extrair todas as respostas
Validar a integridade
Prosseguir com a análise
```

### Tratamento de Erros

#### Respostas Ausentes
Se qualquer tag [Answer]: estiver vazia:
```
"Notei que a Pergunta [X] não foi respondida. Por favor, forneça uma resposta usando uma das letras 
para todas as perguntas antes de prosseguirmos."
```

#### Respostas Inválidas
Se a resposta não for uma escolha de letra válida:
```
"A Pergunta [X] tem uma resposta inválida '[resposta]'. 
Por favor, use apenas as letras fornecidas na pergunta."
```

#### Respostas Ambíguas
Se o usuário fornecer uma explicação em vez de uma letra:
```
"Para a Pergunta [X], por favor, forneça a letra que melhor corresponde à sua resposta. 
Se nenhuma corresponder, escolha 'Outro' e adicione sua descrição após a tag [Answer]:."
```

### Detecção de Contradição e Ambiguidade

**OBRIGATÓRIO**: Após ler as respostas do usuário, você DEVE verificar se há contradições e ambiguidades.

#### Detectando Contradições
Procure por respostas logicamente inconsistentes:
- Incompatibilidade de escopo: "Correção de bug", mas "Toda a base de código afetada".
- Incompatibilidade de risco: "Baixo risco", mas "Mudanças disruptivas".
- Incompatibilidade de cronograma: "Correção rápida", mas "Múltiplos subsistemas".
- Incompatibilidade de impacto: "Componente único", mas "Mudanças significativas de arquitetura".

#### Detectando Ambiguidades
Procure por respostas obscuras ou limítrofes:
- Respostas que poderiam se encaixar em múltiplas classificações.
- Respostas que carecem de especificidade.
- Indicadores conflitantes em múltiplas perguntas.

#### Criando Perguntas de Esclarecimento
Se contradições ou ambiguidades forem detectadas:

1. **Criar arquivo de esclarecimento**: `{nome-da-fase}-clarification-questions.md`
2. **Explicar o problema**: Declare claramente qual contradição/ambiguidade foi detectada.
3. **Fazer perguntas direcionadas**: Use o formato de múltipla escolha para resolver o problema.
4. **Referenciar perguntas originais**: Mostre quais perguntas tiveram respostas conflitantes.

**Exemplo**:
```markdown
# Perguntas de Esclarecimento de [Nome da Fase]

Eu detectei contradições em suas respostas que precisam de esclarecimento:

## Contradição 1: [Breve Descrição]
Você indicou "[Resposta A]" (Q[X]:[Letra]), mas também "[Resposta B]" (Q[Y]:[Letra]).
Essas respostas são contraditórias porque [explicação].

### Clarification Question 1
[Pergunta específica para resolver a contradição]

A) [Opção que resolve em direção à primeira resposta]
B) [Opção que resolve em direção à segunda resposta]
C) [Opção que fornece um meio-termo]
D) [Opção que reformula a pergunta]

[Answer]: 

## Ambiguidade 1: [Breve Descrição]
Sua resposta para a Q[X] ("[Resposta]") é ambígua porque [explicação].

### Clarification Question 2
[Pergunta específica para esclarecer a ambiguidade]

A) [Opção clara 1]
B) [Opção clara 2]
C) [Opção clara 3]
D) [Opção clara 4]

[Answer]: 
```

#### Fluxo de Trabalho para Esclarecimentos

1. **Detectar**: Analise todas as respostas em busca de contradições/ambiguidades.
2. **Criar**: Gere o arquivo de perguntas de esclarecimento se problemas forem encontrados.
3. **Informar**: Fale ao usuário sobre os problemas e o arquivo de esclarecimento.
4. **Aguardar**: Não prossiga até que o usuário forneça os esclarecimentos.
5. **Re-validar**: Após os esclarecimentos, verifique novamente a consistência.
6. **Prosseguir**: Só avance quando todas as contradições forem resolvidas.

#### Exemplo de Mensagem ao Usuário
```
"Eu detectei 2 contradições em suas respostas:

1. Escopo de correção de bug vs. impacto na base de código (Q1 vs Q2)
2. Baixo risco vs. mudanças disruptivas (Q7 vs Q4)

Criei o arquivo classification-clarification-questions.md com 2 perguntas para resolver isso.
Por favor, responda a estas perguntas de esclarecimento antes que eu possa prosseguir com a classificação."
```

### Melhores Práticas

1. **Seja Específico**: As perguntas devem ser claras e inequívocas.
2. **Seja Abrangente**: Cubra todas as informações necessárias.
3. **Seja Conciso**: Mantenha as perguntas focadas em um tópico.
4. **Seja Prático**: As opções devem ser realistas e acionáveis.
5. **Seja Consistente**: Use o mesmo formato em todos os arquivos de perguntas.

### Exemplos Específicos por Fase

#### Exemplo com 2 opções significativas:
```markdown
## Question 1
Este é um projeto novo ou uma base de código existente?

A) Projeto novo (greenfield)
B) Base de código existente (brownfield)
C) Outro (por favor, descreva após a tag [Answer]: abaixo)

[Answer]: 
```

#### Exemplo com 3 opções significativas:
```markdown
## Question 2
Qual é o destino do deploy?

A) Nuvem (AWS, Azure, GCP)
B) Servidores locais (On-premises)
C) Híbrido (nuvem e local)
D) Outro (por favor, descreva após a tag [Answer]: abaixo)

[Answer]: 
```

#### Exemplo com 4 opções significativas:
```markdown
## Question 3
Qual padrão de arquitetura deve ser usado?

A) Arquitetura monolítica
B) Arquitetura de microsserviços
C) Arquitetura Serverless
D) Arquitetura orientada a eventos
E) Outro (por favor, descreva após a tag [Answer]: abaixo)

[Answer]: 
```

## Resumo

**Lembre-se**: 
- ✅ Sempre crie arquivos de perguntas.
- ✅ Sempre use o formato de múltipla escolha.
- ✅ **Sempre inclua "Outro" como a ÚLTIMA opção (OBRIGATÓRIO).**
- ✅ Inclua apenas opções significativas - não invente opções para preencher espaços.
- ✅ Sempre use tags [Answer]:.
- ✅ Sempre aguarde a conclusão do usuário.
- ✅ Sempre valide as respostas em busca de contradições.
- ✅ Sempre crie arquivos de esclarecimento se necessário.
- ✅ Sempre resolva as contradições antes de prosseguir.
- ❌ Nunca faça perguntas no chat.
- ❌ Nunca invente opções apenas para ter A, B, C, D.
- ❌ Nunca prossiga sem respostas.
- ❌ Nunca prossiga com contradições não resolvidas.
- ❌ Nunca faça suposições sobre respostas ambíguas.