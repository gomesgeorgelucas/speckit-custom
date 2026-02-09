---
description: Identifica áreas subespecificadas na especificação da funcionalidade atual, fazendo até 5 perguntas de esclarecimento altamente direcionadas e codificando as respostas de volta na spec.
handoffs: 
  - label: Construir Plano Técnico
    agent: speckit.plan
    prompt: Create a plan for the spec. I am building with...
---

## User Input

```text
$ARGUMENTS
```

Você **DEVE** considerar o input do usuário antes de prosseguir (se não estiver vazio).

## Outline

Objetivo: Detectar e reduzir ambiguidades ou pontos de decisão ausentes na especificação da funcionalidade ativa e registrar os esclarecimentos diretamente no arquivo de spec.

Nota: Espera-se que este fluxo de esclarecimento seja executado (e concluído) ANTES de invocar o `/speckit.plan`. Se o usuário declarar explicitamente que está pulando o esclarecimento (ex: spike exploratório), você pode prosseguir, mas deve alertar que o risco de retrabalho downstream aumenta.

Passos de execução:

1. Execute `.specify/scripts/powershell/check-prerequisites.ps1 -Json -PathsOnly` da raiz do repositório **uma vez** (modo combinado `--json --paths-only` / `-Json -PathsOnly`). Analise os campos mínimos do payload JSON:
   - `FEATURE_DIR`
   - `FEATURE_SPEC`
   - (Opcionalmente capture `IMPL_PLAN`, `TASKS` para fluxos encadeados futuros.)
   - Se a análise do JSON falhar, aborte e instrua o usuário a executar novamente `/speckit.specify` ou verificar o ambiente da branch da funcionalidade.
   - Para aspas simples em argumentos como "I'm Groot", use a sintaxe de escape: ex: 'I'\''m Groot'.

2. Carregue o arquivo de spec atual. Realize uma varredura estruturada de ambiguidade e cobertura usando esta taxonomia. Para cada categoria, marque o status: Claro / Parcial / Ausente. Produza um mapa de cobertura interno usado para priorização (não exiba o mapa bruto, a menos que nenhuma pergunta seja feita).

   Escopo Funcional e Comportamento:
   - Objetivos centrais do usuário e critérios de sucesso
   - Declarações explícitas de fora do escopo
   - Diferenciação de papéis / personas de usuário

   Domínio e Modelo de Dados:
   - Entidades, atributos, relacionamentos
   - Regras de identidade e unicidade
   - Transições de estado/ciclo de vida
   - Suposições de volume / escala de dados

   Interação e Fluxo de UX:
   - Jornadas críticas do usuário / sequências
   - Estados de erro/vazio/carregamento
   - Notas de acessibilidade ou localização

   Atributos de Qualidade Não Funcionais:
   - Performance (metas de latência, throughput)
   - Escalabilidade (horizontal/vertical, limites)
   - Confiabilidade e disponibilidade (uptime, expectativas de recuperação)
   - Observabilidade (sinais de logging, métricas, rastreamento)
   - Segurança e privacidade (authN/Z, proteção de dados, suposições de ameaças)
   - Restrições de conformidade / regulatórias (se houver)

   Integração e Dependências Externas:
   - Serviços externos/APIs e modos de falha
   - Formatos de importação/exportação de dados
   - Suposições de protocolo/versionamento

   Casos de Borda e Tratamento de Falhas:
   - Cenários negativos
   - Rate limiting / throttling
   - Resolução de conflitos (ex: edições concorrentes)

   Restrições e Tradeoffs:
   - Restrições técnicas (linguagem, armazenamento, hospedagem)
   - Tradeoffs explícitos ou alternativas rejeitadas

   Terminologia e Consistência:
   - Termos do glossário canônico
   - Sinônimos evitados / termos depreciados

   Sinais de Conclusão:
   - Testabilidade dos critérios de aceitação
   - Indicadores mensuráveis no estilo Definição de Pronto (DoD)

   Diversos / Placeholders:
   - Marcadores TODO / decisões não resolvidas
   - Adjetivos ambíguos ("robusto", "intuitivo") sem quantificação

   Para cada categoria com status Parcial ou Ausente, adicione uma oportunidade de pergunta candidata, a menos que:
   - O esclarecimento não altere materialmente a estratégia de implementação ou validação.
   - A informação seja melhor postergada para a fase de planejamento (anote internamente).

3. Gere (internamente) uma fila priorizada de perguntas de esclarecimento candidatas (máximo de 5). NÃO exiba todas de uma vez. Aplique estas restrições:
    - Máximo de 10 perguntas totais em toda a sessão.
    - Cada pergunta deve ser respondida com:
       - Uma seleção de múltipla escolha curta (2 a 5 opções distintas e mutuamente exclusivas), OU
       - Uma resposta de uma palavra / frase curta (restrinja explicitamente: "Responda em <= 5 palavras").
    - Inclua apenas perguntas cujas respostas impactem materialmente a arquitetura, modelagem de dados, decomposição de tarefas, design de testes, comportamento de UX, prontidão operacional ou validação de conformidade.
    - Garanta o equilíbrio da cobertura de categorias: tente cobrir as categorias não resolvidas de maior impacto primeiro; evite fazer duas perguntas de baixo impacto quando uma única área de alto impacto (ex: postura de segurança) não estiver resolvida.
    - Exclua perguntas já respondidas, preferências estilísticas triviais ou detalhes de execução em nível de plano (a menos que bloqueiem a correção).
    - Favoreça esclarecimentos que reduzam o risco de retrabalho downstream ou evitem testes de aceitação desalinhados.
    - Se mais de 5 categorias permanecerem não resolvidas, selecione as 5 principais pela heurística (Impacto * Incerteza).

4. Loop de perguntas sequenciais (interativo):
    - Apresente EXATAMENTE UMA pergunta de cada vez.
    - Para perguntas de múltipla escolha:
       - **Analise todas as opções** e determine a **opção mais adequada** com base em:
          - Melhores práticas para o tipo de projeto
          - Padrões comuns em implementações similares
          - Redução de risco (segurança, performance, manutenibilidade)
          - Alinhamento com quaisquer objetivos ou restrições explícitas do projeto visíveis na spec
       - Apresente sua **opção recomendada com destaque** no topo, com um raciocínio claro (1-2 frases explicando por que esta é a melhor escolha).
       - Formato: `**Recomendado:** Opção [X] - <raciocínio>`
       - Em seguida, renderize todas as opções como uma tabela Markdown:

       | Opção | Descrição |
       |-------|-----------|
       | A | <Descrição da Opção A> |
       | B | <Descrição da Opção B> |
       | C | <Descrição da Opção C> (adicione D/E conforme necessário até 5) |
       | Curta | Forneça uma resposta curta diferente (<=5 palavras) (Incluir apenas se alternativa livre for apropriada) |

       - Após a tabela, adicione: `Você pode responder com a letra da opção (ex: "A"), aceitar a recomendação dizendo "sim" ou "recomendado", ou fornecer sua própria resposta curta.`
    - Para estilo de resposta curta (sem opções discretas significativas):
       - Forneça sua **resposta sugerida** com base em melhores práticas e contexto.
       - Formato: `**Sugerido:** <sua resposta proposta> - <breve raciocínio>`
       - Em seguida, exiba: `Formato: Resposta curta (<=5 palavras). Você pode aceitar a sugestão dizendo "sim" ou "sugerido", ou fornecer sua própria resposta.`
    - Após a resposta do usuário:
       - Se o usuário responder "sim", "recomendado" ou "sugerido", use sua recomendação/sugestão declarada anteriormente como a resposta.
       - Caso contrário, valide se a resposta mapeia para uma opção ou se encaixa na restrição de <= 5 palavras.
       - Se for ambíguo, peça uma desambiguação rápida (a contagem ainda pertence à mesma pergunta; não avance).
       - Uma vez satisfatório, registre-o na memória de trabalho (não grave no disco ainda) e passe para a próxima pergunta da fila.
    - Pare de fazer perguntas quando:
       - Todas as ambiguidades críticas forem resolvidas precocemente (itens da fila tornam-se desnecessários), OU
       - O usuário sinalizar a conclusão ("pronto", "bom", "chega"), OU
       - Você atingir 5 perguntas feitas.
    - Nunca revele perguntas futuras da fila antecipadamente.
    - Se não existirem perguntas válidas no início, relate imediatamente que não há ambiguidades críticas.

5. Integração após CADA resposta aceita (abordagem de atualização incremental):
    - Mantenha a representação em memória da spec (carregada uma vez no início) mais os conteúdos brutos do arquivo.
    - Para a primeira resposta integrada nesta sessão:
       - Garanta que uma seção `## Clarifications` exista (crie-a logo após a seção de visão geral/contextual de nível mais alto, conforme o template da spec, se estiver faltando).
       - Sob ela, crie (se não estiver presente) um subtítulo `### Session YYYY-MM-DD` para hoje.
    - Adicione uma linha de tópico imediatamente após a aceitação: `- Q: <pergunta> → A: <resposta final>`.
    - Em seguida, aplique imediatamente o esclarecimento à(s) seção(ões) mais apropriada(s):
       - Ambiguidade funcional → Atualize ou adicione um tópico em Requisitos Funcionais.
       - Interação do usuário / distinção de ator → Atualize as Histórias de Usuário ou a subseção de Atores (se presente) com o papel, restrição ou cenário esclarecido.
       - Formato dos dados / entidades → Atualize o Modelo de Dados (adicione campos, tipos, relacionamentos) preservando a ordenação; anote as restrições adicionadas de forma sucinta.
       - Restrição não funcional → Adicione/modifique critérios mensuráveis na seção de Atributos Não Funcionais / de Qualidade (converta adjetivo vago em métrica ou alvo explícito).
       - Caso de borda / fluxo negativo → Adicione um novo tópico sob Casos de Borda / Tratamento de Erros (ou crie tal subseção se o template fornecer um placeholder para isso).
       - Conflito de terminologia → Normalize o termo em toda a spec; mantenha o original apenas se necessário adicionando `(anteriormente referido como "X")` uma vez.
    - Se o esclarecimento invalidar uma declaração ambígua anterior, substitua essa declaração em vez de duplicar; não deixe nenhum texto contraditório obsoleto.
    - Salve o arquivo de spec APÓS cada integração para minimizar o risco de perda de contexto (sobrescrita atômica).
    - Preserve a formatação: não reordene seções não relacionadas; mantenha a hierarquia de títulos intacta.
    - Mantenha cada esclarecimento inserido mínimo e testável (evite desvios narrativos).

6. Validação (realizada após CADA gravação mais uma passagem final):
   - A sessão de esclarecimentos contém exatamente um tópico por resposta aceita (sem duplicatas).
   - Total de perguntas feitas (aceitas) ≤ 5.
   - As seções atualizadas não contêm placeholders vagos remanescentes que a nova resposta deveria resolver.
   - Nenhuma declaração anterior contraditória permanece (verifique se escolhas alternativas agora inválidas foram removidas).
   - Estrutura Markdown válida; apenas novos títulos permitidos: `## Clarifications`, `### Session YYYY-MM-DD`.
   - Consistência de terminologia: o mesmo termo canônico usado em todas as seções atualizadas.

7. Grave a spec atualizada de volta em `FEATURE_SPEC`.

8. Relate a conclusão (após o término do loop de perguntas ou encerramento antecipado):
   - Número de perguntas feitas e respondidas.
   - Caminho para a spec atualizada.
   - Seções tocadas (liste os nomes).
   - Tabela de resumo de cobertura listando cada categoria da taxonomia com o Status: Resolvido (era Parcial/Ausente e foi abordado), Adiado (excede a cota de perguntas ou melhor se adapta ao planejamento), Claro (já suficiente), Pendente (ainda Parcial/Ausente, mas de baixo impacto).
   - Se algum Pendente ou Adiado permanecer, recomende se deve prosseguir para o `/speckit.plan` ou executar o `/speckit.clarify` novamente mais tarde, após o plano.
   - Próximo comando sugerido.

Regras de comportamento:

- Se nenhuma ambiguidade significativa for encontrada (ou se todas as perguntas potenciais forem de baixo impacto), responda: "Nenhuma ambiguidade crítica detectada que justifique esclarecimento formal." e sugira prosseguir.
- Se o arquivo de spec estiver ausente, instrua o usuário a executar `/speckit.specify` primeiro (não crie uma nova spec aqui).
- Nunca exceda 5 perguntas totais feitas (tentativas de esclarecimento para uma única pergunta não contam como novas perguntas).
- Evite perguntas especulativas sobre stack técnica, a menos que a ausência bloqueie a clareza funcional.
- Respeite os sinais de encerramento antecipado do usuário ("parar", "pronto", "prosseguir").
- Se nenhuma pergunta for feita devido à cobertura total, exiba um resumo de cobertura compacto (todas as categorias Claras) e sugira avançar.
- Se a cota for atingida com categorias de alto impacto não resolvidas remanescentes, sinalize-as explicitamente como Adiado com a justificativa.