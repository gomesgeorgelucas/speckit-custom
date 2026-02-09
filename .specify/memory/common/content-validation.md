# Regras de Validação de Conteúdo

## OBRIGATÓRIO: Validação de Conteúdo Antes da Criação de Arquivos

**CRÍTICO**: Todo conteúdo gerado DEVE ser validado antes de ser gravado em arquivos para evitar erros de processamento (parsing).

## Padrões para Diagramas ASCII

**CRÍTICO**: Antes de criar QUALQUER arquivo com diagramas ASCII:

1. **CARREGUE** `common/ascii-diagram-standards.md`
2. **VALIDE** cada diagrama:
   - Conte os caracteres por linha (todas as linhas DEVEM ter a mesma largura)
   - Use APENAS: `+` `-` `|` `^` `v` `<` `>` e espaços
   - SEM caracteres Unicode de desenho de caixa
   - Apenas espaços (NÃO use tabs)
3. **TESTE** o alinhamento verificando se os cantos das caixas se alinham verticalmente

**Consulte `common/ascii-diagram-standards.md` para padrões e checklist de validação.**

## Validação de Diagramas Mermaid

### Passos de Validação Obrigatórios
1. **Verificação de Sintaxe**: Valide a sintaxe Mermaid antes da criação do arquivo
2. **Escape de Caracteres**: Garanta que caracteres especiais sejam escapados corretamente
3. **Conteúdo de Fallback**: Forneça uma alternativa em texto caso o Mermaid falhe na validação

### Regras de Validação Mermaid
```markdown
## ANTES de criar qualquer arquivo com diagramas Mermaid:

1. Verifique caracteres inválidos em IDs de nós (use apenas alfanuméricos + underscore)
2. Escape caracteres especiais em rótulos: " → \" e ' → \'
3. Valide a sintaxe do fluxograma: as conexões entre nós devem ser válidas
4. Teste o processamento do diagrama com uma validação simples

## FALLBACK: Se a validação Mermaid falhar, use a representação do fluxo baseada em texto
```

### Padrão de Implementação
```markdown
## Visualização do Fluxo de Trabalho

### Diagrama Mermaid (se a sintaxe for válida)
```mermaid
[conteúdo do diagrama validado]
```

### Alternativa em Texto (sempre incluir)
```
Fase 1: INCEPÇÃO
- Estágio 1: Detecção de Workspace (CONCLUÍDO)
- Estágio 2: Análise de Requisitos (CONCLUÍDO)
[continue com a representação em texto]
```

## Validação Geral de Conteúdo

### Checklist de Validação Pré-Criação
- [ ] Validar blocos de código incorporados (Mermaid, JSON, YAML)
- [ ] Verificar escape de caracteres especiais
- [ ] Verificar correção da sintaxe markdown
- [ ] Testar compatibilidade de processamento do conteúdo
- [ ] Incluir conteúdo de fallback para elementos complexos

### Regras de Prevenção de Erros
1. **Sempre valide antes de usar ferramentas/comandos para gravar arquivos**: Nunca grave conteúdo não validado.
2. **Escape caracteres especiais**: Particularmente em diagramas e blocos de código.
3. **Forneça alternativas**: Inclua versões em texto de conteúdos visuais.
4. **Teste a sintaxe**: Valide estruturas de conteúdo complexas.

## Tratamento de Falha de Validação

### Quando a Validação Falha
1. **Registre o erro**: Grave o que falhou na validação.
2. **Use conteúdo de fallback**: Mude para a alternativa baseada em texto.
3. **Continue o fluxo de trabalho**: Não bloqueie o processo por falhas de validação de conteúdo.
4. **Informe o usuário**: Mencione que um conteúdo simplificado foi usado devido a restrições de processamento.