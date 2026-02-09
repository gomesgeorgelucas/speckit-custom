---
description: Cria ou atualiza a constituição do projeto a partir de inputs de princípios interativos ou fornecidos, garantindo que todos os templates dependentes permaneçam sincronizados.
handoffs: 
  - label: Construir Especificação
    agent: speckit.specify
    prompt: Implement the feature specification based on the updated constitution. I want to build...
---

## User Input

```text
$ARGUMENTS
```

Você **DEVE** considerar o input do usuário antes de prosseguir (se não estiver vazio).

## Outline

Você está atualizando a constituição do projeto em `.specify/memory/constitution.md`. Este arquivo é um TEMPLATE contendo tokens de placeholder em colchetes angulares (ex: `[PROJECT_NAME]`, `[PRINCIPLE_1_NAME]`). Seu trabalho é (a) coletar/derivar valores concretos, (b) preencher o template com precisão e (c) propagar quaisquer emendas pelos artefatos dependentes.

Siga este fluxo de execução:

1. Carregue o template da constituição existente em `.specify/memory/constitution.md`.
   - Identifique cada token de placeholder no formato `[IDENTIFICADOR_EM_MAIUSCULAS]`.
   **IMPORTANTE**: O usuário pode exigir menos ou mais princípios do que os utilizados no template. Se um número for especificado, respeite-o — siga o template geral. Você atualizará o documento adequadamente.

2. Colete/derive valores para os placeholders:
   - Se o input do usuário (conversa) fornecer um valor, use-o.
   - Caso contrário, infira do contexto do repositório existente (README, docs, versões anteriores da constituição, se incorporadas).
   - Para datas de governança: `RATIFICATION_DATE` é a data de adoção original (se desconhecida, pergunte ou marque como TODO), `LAST_AMENDED_DATE` é hoje se mudanças forem feitas, caso contrário, mantenha a anterior.
   - `CONSTITUTION_VERSION` deve incrementar de acordo com as regras de versionamento semântico:
     - MAJOR: Remoções ou redefinições de governança/princípios incompatíveis com versões anteriores.
     - MINOR: Novo princípio/seção adicionado ou orientação materialmente expandida.
     - PATCH: Esclarecimentos, redação, correções de erros de digitação, refinamentos não semânticos.
   - Se o tipo de bump de versão for ambíguo, proponha o raciocínio antes de finalizar.

3. Redija o conteúdo da constituição atualizado:
   - Substitua cada placeholder por texto concreto (sem tokens entre colchetes restantes, exceto slots de template intencionalmente retidos que o projeto optou por não definir ainda — justifique explicitamente quaisquer remanescentes).
   - Preserve a hierarquia de títulos e os comentários podem ser removidos uma vez substituídos, a menos que ainda adicionem orientação esclarecedora.
   - Garanta cada seção de Princípio: linha de nome sucinta, parágrafo (ou lista de tópicos) capturando regras não negociáveis, justificativa explícita se não for óbvia.
   - Garanta que a seção de Governança liste o procedimento de emenda, política de versionamento e expectativas de revisão de conformidade.

4. Checklist de propagação de consistência (converta o checklist anterior em validações ativas):
   - Leia `.specify/templates/plan-template.md` e garanta que qualquer "Constitution Check" ou regras se alinhem com os princípios atualizados.
   - Leia `.specify/templates/spec-template.md` para alinhamento de escopo/requisitos — atualize se a constituição adicionar/remover seções obrigatórias ou restrições.
   - Leia `.specify/templates/tasks-template.md` e garanta que a categorização de tarefas reflita tipos de tarefas novos ou removidos orientados por princípios (ex: observabilidade, versionamento, disciplina de testes).
   - Leia cada arquivo de comando em `.specify/templates/commands/*.md` (incluindo este) para verificar se não restam referências obsoletas (nomes específicos de agentes como CLAUDE) quando orientação genérica é exigida.
   - Leia quaisquer documentos de orientação em tempo de execução (ex: `README.md`, `docs/quickstart.md` ou arquivos de orientação específicos de agentes, se presentes). Atualize as referências aos princípios alterados.

5. Produza um Relatório de Impacto de Sincronização (Sync Impact Report) (insira como um comentário HTML no topo do arquivo da constituição após a atualização):
   - Mudança de versão: antiga → nova
   - Lista de princípios modificados (título antigo → novo título se renomeado)
   - Seções adicionadas
   - Seções removidas
   - Templates exigindo atualizações (✅ atualizado / ⚠ pendente) com caminhos de arquivos
   - TODOs de acompanhamento, se houver placeholders intencionalmente adiados.

6. Validação antes da saída final:
   - Nenhum token entre colchetes inexplicado restante.
   - Linha de versão corresponde ao relatório.
   - Datas no formato ISO YYYY-MM-DD.
   - Princípios são declarativos, testáveis e livres de linguagem vaga ("deve" → substituir por justificativa MUST/SHOULD onde apropriado).

7. Grave a constituição concluída de volta em `.specify/memory/constitution.md` (sobrescrever).

8. Exiba um resumo final ao usuário com:
   - Nova versão e justificativa do bump.
   - Quaisquer arquivos sinalizados para acompanhamento manual.
   - Mensagem de commit sugerida (ex: `docs: amend constitution to vX.Y.Z (principle additions + governance update)`).

Requisitos de Formatação e Estilo:

- Use títulos Markdown exatamente como no template (não diminua/promova níveis).
- Quebre linhas longas de justificativa para manter a legibilidade (<100 caracteres idealmente), mas não force de forma rígida com quebras estranhas.
- Mantenha uma única linha em branco entre as seções.
- Evite espaços em branco no final das linhas.

Se o usuário fornecer atualizações parciais (ex: apenas uma revisão de princípio), ainda assim realize as etapas de validação e decisão de versão.

Se informações críticas estiverem faltando (ex: data de ratificação verdadeiramente desconhecida), insira `TODO(<NOME_DO_CAMPO>): explicação` e inclua no Sync Impact Report sob itens adiados.

Não crie um novo template; opere sempre no arquivo `.specify/memory/constitution.md` existente.