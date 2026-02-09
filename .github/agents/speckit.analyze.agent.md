---
description: Perform a non-destructive cross-artifact consistency and quality analysis across spec.md, plan.md, and tasks.md after task generation.
---

## User Input

```text
$ARGUMENTS
```

Você **DEVE** considerar o input do usuário antes de prosseguir (se não estiver vazio).

## Objetivo

Identificar inconsistências, duplicações, ambiguidades e itens subespecificados nos três artefatos principais (`spec.md`, `plan.md`, `tasks.md`) antes da implementação. Este comando DEVE ser executado apenas após o `/speckit.tasks` ter produzido com sucesso um `tasks.md` completo.

## Restrições Operacionais

**ESTRITAMENTE SOMENTE LEITURA**: **Não** modifique nenhum arquivo. Gere um relatório de análise estruturado. Ofereça um plano de remediação opcional (o usuário deve aprovar explicitamente antes que qualquer comando de edição subsequente seja invocado manualmente).

**Autoridade da Constituição**: A constituição do projeto (`.specify/memory/constitution.md`) é **não negociável** dentro deste escopo de análise. Conflitos com a constituição são automaticamente CRÍTICOS e exigem ajuste da spec, plan ou tasks — não a diluição, reinterpretação ou ignorância silenciosa do princípio. Se um princípio em si precisar mudar, isso deve ocorrer em uma atualização explícita e separada da constituição, fora do `/speckit.analyze`.

## Passos de Execução

### 1. Inicializar Contexto de Análise

Execute `.specify/scripts/powershell/check-prerequisites.ps1 -Json -RequireTasks -IncludeTasks` uma vez a partir da raiz do repositório e analise o JSON para obter FEATURE_DIR e AVAILABLE_DOCS. Derive os caminhos absolutos:

- SPEC = FEATURE_DIR/spec.md
- PLAN = FEATURE_DIR/plan.md
- TASKS = FEATURE_DIR/tasks.md

Aborte com uma mensagem de erro se qualquer arquivo obrigatório estiver ausente (instrua o usuário a executar o comando de pré-requisito ausente).
Para aspas simples em argumentos como "I'm Groot", use a sintaxe de escape: ex: 'I'\''m Groot' (ou aspas duplas se possível: "I'm Groot").

### 2. Carregar Artefatos (Divulgação Progressiva)

Carregue apenas o contexto mínimo necessário de cada artefato:

**De spec.md:**

- Visão Geral/Contexto
- Requisitos Funcionais
- Requisitos Não Funcionais
- Histórias de Usuário
- Casos de Borda (se presentes)

**De plan.md:**

- Escolhas de Arquitetura/stack
- Referências do Modelo de Dados
- Fases
- Restrições Técnicas

**De tasks.md:**

- IDs de Tarefa
- Descrições
- Agrupamento de Fases
- Marcadores de Paralelismo [P]
- Caminhos de arquivos referenciados

**Da constituição:**

- Carregue `.specify/memory/constitution.md` para validação de princípios

### 3. Construir Modelos Semânticos

Crie representações internas (não inclua artefatos brutos na saída):

- **Inventário de requisitos**: Cada requisito funcional + não funcional com uma chave estável (derive um slug baseado na frase imperativa; ex: "User can upload file" → `user-can-upload-file`)
- **Inventário de histórias/ações de usuário**: Ações discretas do usuário com critérios de aceitação
- **Mapeamento de cobertura de tarefas**: Mapeie cada tarefa para um ou mais requisitos ou histórias (inferência por palavra-chave / padrões de referência explícitos como IDs ou frases-chave)
- **Conjunto de regras da constituição**: Extraia nomes de princípios e declarações normativas MUST/SHOULD

### 4. Ciclos de Detecção (Análise Eficiente de Tokens)

Foque em achados de alto sinal. Limite a 50 achados no total; agregue o restante em um resumo de transbordamento (overflow).

#### A. Detecção de Duplicação

- Identificar requisitos quase duplicados
- Marcar frases de menor qualidade para consolidação

#### B. Detecção de Ambiguidade

- Sinalizar adjetivos vagos (rápido, escalável, seguro, intuitivo, robusto) que carecem de critérios mensuráveis
- Sinalizar espaços reservados não resolvidos (TODO, TKTK, ???, `<placeholder>`, etc.)

#### C. Subespecificação

- Requisitos com verbos mas sem objeto ou resultado mensurável
- Histórias de usuário sem alinhamento de critérios de aceitação
- Tarefas referenciando arquivos ou componentes não definidos na spec/plan

#### D. Alinhamento com a Constituição

- Qualquer requisito ou elemento do plano que conflite com um princípio MUST
- Seções obrigatórias ou portões de qualidade ausentes da constituição

#### E. Lacunas de Cobertura (Gaps)

- Requisitos com zero tarefas associadas
- Tarefas sem requisito/história mapeada
- Requisitos não funcionais não refletidos nas tarefas (ex: performance, segurança)

#### F. Inconsistência

- Derivação de terminologia (mesmo conceito nomeado de forma diferente entre os arquivos)
- Entidades de dados referenciadas no plano mas ausentes na spec (ou vice-versa)
- Contradições na ordenação de tarefas (ex: tarefas de integração antes de tarefas de configuração fundamental sem nota de dependência)
- Requisitos conflitantes (ex: um exige Next.js enquanto outro especifica Vue)

### 5. Atribuição de Severidade

Use esta heurística para priorizar os achados:

- **CRÍTICO**: Viola um MUST da constituição, falta um artefato principal da spec, ou requisito com cobertura zero que bloqueia a funcionalidade base
- **ALTO**: Requisito duplicado ou conflitante, atributo de segurança/performance ambíguo, critério de aceitação não testável
- **MÉDIO**: Derivação de terminologia, falta de cobertura de tarefa não funcional, caso de borda subespecificado
- **BAIXO**: Melhorias de estilo/redação, redundância menor que não afeta a ordem de execução

### 6. Produzir Relatório de Análise Compacto

Gere um relatório em Markdown (sem gravações de arquivo) com a seguinte estrutura:

## Specification Analysis Report

| ID | Categoria | Severidade | Localização(ões) | Resumo | Recomendação |
|----|-----------|------------|------------------|--------|--------------|
| A1 | Duplicação | ALTO | spec.md:L120-134 | Dois requisitos similares ... | Mesclar redação; manter versão clara |

(Adicione uma linha por achado; gere IDs estáveis prefixados pela inicial da categoria.)

**Tabela de Resumo de Cobertura:**

| Chave do Requisito | Possui Tarefa? | IDs das Tarefas | Notas |
|--------------------|----------------|-----------------|-------|

**Problemas de Alinhamento com a Constituição:** (se houver)

**Tarefas Não Mapeadas:** (se houver)

**Métricas:**

- Total de Requisitos
- Total de Tarefas
- % de Cobertura (requisitos com >=1 tarefa)
- Contagem de Ambiguidades
- Contagem de Duplicações
- Contagem de Problemas Críticos

### 7. Fornecer Próximas Ações

Ao final do relatório, exiba um bloco de Próximas Ações conciso:

- Se existirem problemas CRÍTICOS: Recomendar a resolução antes do `/speckit.implement`
- Se houver apenas BAIXO/MÉDIO: O usuário pode prosseguir, mas forneça sugestões de melhoria
- Forneça sugestões de comandos explícitos: ex: "Execute /speckit.specify com refinamento", "Execute /speckit.plan para ajustar a arquitetura", "Edite manualmente tasks.md para adicionar cobertura para 'performance-metrics'"

### 8. Oferecer Remediação

Pergunte ao usuário: "Gostaria que eu sugerisse edições concretas de remediação para os top N problemas?" (NÃO as aplique automaticamente.)

## Princípios Operacionais

### Eficiência de Contexto

- **Tokens mínimos de alto sinal**: Foque em achados acionáveis, não em documentação exaustiva
- **Divulgação progressiva**: Carregue artefatos incrementalmente; não despeje todo o conteúdo na análise
- **Saída eficiente em tokens**: Limite a tabela de achados a 50 linhas; resuma o excedente
- **Resultados determinísticos**: Executar novamente sem alterações deve produzir IDs e contagens consistentes

### Diretrizes de Análise

- **NUNCA modifique arquivos** (esta é uma análise apenas de leitura)
- **NUNCA alucine seções ausentes** (se estiverem ausentes, relate-as com precisão)
- **Priorize violações da constituição** (estas são sempre CRÍTICAS)
- **Use exemplos em vez de regras exaustivas** (cite instâncias específicas, não padrões genéricos)
- **Relate zero problemas com elegância** (emita um relatório de sucesso com estatísticas de cobertura)

## Contexto

$ARGUMENTS
