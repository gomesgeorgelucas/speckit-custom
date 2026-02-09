# Guia de Prevenção de Excesso de Confiança

## Declaração do Problema

O AI-DLC estava exibindo excesso de confiança ao não fazer perguntas de esclarecimento suficientes, mesmo para declarações de intenção de projetos complexos. Isso levava a suposições em vez de levantamento adequado de requisitos.

## Análise de Causa Raiz

O problema de excesso de confiança foi causado por diretrizes em múltiplos estágios que incentivavam pular perguntas:

1. **Design Funcional**: "Pule categorias inteiras se não forem aplicáveis"
2. **Histórias de Usuário**: "Use categorias como inspiração, NÃO como checklist obrigatório"
3. **Análise de Requisitos**: Padrões similares incentivando o mínimo de questionamento
4. **Requisitos NFR**: Condições "apenas se" que desencorajavam a análise completa

Essas diretrizes diziam à IA para evitar fazer perguntas em vez de incentivar um levantamento de requisitos abrangente.

## Solução Implementada

### Filosofia de Geração de Perguntas Atualizada

**ABORDAGEM ANTIGA**: "Apenas faça perguntas se for absolutamente necessário"
**NOVA ABORDAGEM**: "Na dúvida, faça a pergunta - o excesso de confiança leva a resultados ruins"

### Principais Mudanças Realizadas

#### 1. Estágio de Análise de Requisitos
- Alterado de "apenas se necessário" para "SEMPRE crie perguntas, a menos que esteja excepcionalmente claro"
- Adicionadas áreas de avaliação abrangentes (funcional, não funcional, contexto de negócio, contexto técnico)
- Enfatizada a abordagem proativa de questionamento

#### 2. Estágio de Histórias de Usuário
- Removida a diretriz de "pular categorias inteiras"
- Adicionadas categorias de perguntas abrangentes para avaliar
- Requisitos de análise de resposta aprimorados
- Mandatos de perguntas de acompanhamento fortalecidos

#### 3. Estágio de Design Funcional
- Substituídas as condições "apenas se" por avaliação abrangente
- Adicionadas mais categorias de perguntas (fluxo de dados, pontos de integração, tratamento de erros)
- Requisitos de detecção e resolução de ambiguidade fortalecidos

#### 4. Estágio de Requisitos NFR
- Expandidas as categorias de perguntas além dos NFRs básicos
- Adicionadas considerações de confiabilidade, manutenibilidade e usabilidade
- Análise de resposta aprimorada para ambiguidades técnicas

### Novos Princípios Norteadores

1. **Padrão é Perguntar**: Quando houver qualquer ambiguidade, faça perguntas de esclarecimento.
2. **Cobertura Abrangente**: Avalie TODAS as categorias relevantes, não pule áreas.
3. **Análise Minuciosa**: Analise cuidadosamente TODAS as respostas do usuário em busca de ambiguidades.
4. **Acompanhamento Obrigatório**: Crie perguntas de acompanhamento para QUAISQUER respostas obscuras.
5. **Não Prossiga com Ambiguidade**: Não avance até que TODAS as ambiguidades sejam resolvidas.

## Diretrizes de Implementação

### Para Geração de Perguntas
- Avalie TODAS as categorias de perguntas, não pule nenhuma.
- Faça perguntas sempre que o esclarecimento puder melhorar a qualidade.
- Inclua categorias de perguntas abrangentes em cada estágio.
- Use a inclusão como padrão, em vez da exclusão de perguntas.

### Para Análise de Respostas
- Procure por respostas vagas: "depende", "talvez", "não tenho certeza", "uma mistura de", "algo entre".
- Detecte termos não definidos e referências a conceitos externos.
- Identifique respostas contraditórias ou incompletas.
- Crie perguntas de acompanhamento para QUAISQUER ambiguidades.

### Para Perguntas de Acompanhamento
- Crie arquivos de esclarecimento separados quando ambiguidades forem detectadas.
- Faça perguntas específicas para resolver cada ambiguidade.
- Não prossiga até que TODAS as respostas obscuras sejam esclarecidas.
- Seja minucioso - é melhor esclarecer demais do que de menos.

## Garantia de Qualidade

### Sinais de Alerta para Monitorar
- Estágios sendo concluídos sem nenhuma pergunta em projetos complexos.
- Prosseguir com respostas de usuários vagas ou ambíguas.
- Pular categorias inteiras de perguntas sem justificativa.
- Fazer suposições em vez de pedir esclarecimentos.

### Indicadores de Sucesso
- Número apropriado de perguntas de esclarecimento para a complexidade do projeto.
- Análise minuciosa das respostas do usuário com acompanhamento quando necessário.
- Requisitos claros e inequívocos antes de prosseguir para a implementação.
- Necessidade reduzida de mudanças em estágios posteriores devido ao melhor esclarecimento inicial.

## Manutenção

Este guia deve ser referenciado ao:
- Adicionar novos estágios ao AI-DLC.
- Atualizar instruções de estágios existentes.
- Revisar o desempenho do AI-DLC em relação a problemas de excesso de confiança.
- Treinar membros da equipe nos princípios de geração de perguntas do AI-DLC.

## Lição Principal

**É melhor fazer perguntas demais do que fazer suposições incorretas.** O custo de fazer perguntas de esclarecimento antecipadamente é muito menor do que o custo de implementar a solução errada baseada em suposições.