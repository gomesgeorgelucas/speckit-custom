# Detecção de Workspace

**Propósito**: Determinar o estado do workspace e verificar projetos AI-DLC existentes.

## Passo 1: Verificar Projeto AI-DLC Existente

Verificar se `aidlc-docs/aidlc-state.md` existe:
- **Se existir**: Retomar da última fase (carregar contexto das fases anteriores).
- **Se não existir**: Continuar com a avaliação de novo projeto.

## Passo 2: Escanear Workspace em Busca de Código Existente

**Determinar se o workspace possui código existente:**
- Escanear o workspace em busca de arquivos de código fonte (.java, .py, .js, .ts, .jsx, .tsx, .kt, .kts, .scala, .groovy, .go, .rs, .rb, .php, .c, .h, .cpp, .hpp, .cc, .cs, .fs, etc.).
- Verificar arquivos de build (pom.xml, package.json, build.gradle, etc.).
- Procurar por indicadores de estrutura de projeto.
- Identificar o diretório raiz do workspace (NÃO em `aidlc-docs/`).

**Registrar descobertas:**
```markdown
## Workspace State
- **Existing Code**: [Sim/Não]
- **Programming Languages**: [Listar se encontradas]
- **Build System**: [Maven/Gradle/npm/etc. se encontrado]
- **Project Structure**: [Monolito/Microsserviços/Biblioteca/Vazio]
- **Workspace Root**: [Caminho absoluto]
```

## Passo 3: Determinar Próxima Fase

**SE o workspace estiver vazio (sem código existente)**:
- Definir flag: `brownfield = false`
- Próxima fase: Análise de Requisitos.

**SE o workspace possuir código existente**:
- Definir flag: `brownfield = true`
- Verificar artefatos de engenharia reversa existentes em `aidlc-docs/inception/reverse-engineering/`.
- **SE existirem artefatos de engenharia reversa**: Carregá-los, pular para Análise de Requisitos.
- **SE não existirem artefatos de engenharia reversa**: A próxima fase é Engenharia Reversa.

## Passo 4: Criar Arquivo de Estado Inicial

Criar `aidlc-docs/aidlc-state.md`:

```markdown
# Rastreamento de Estado AI-DLC

## Informações do Projeto
- **Project Type**: [Greenfield/Brownfield]
- **Data de Início**: [Timestamp ISO]
- **Estágio Atual**: INCEPTION - Detecção de Workspace

## Estado do Workspace
- **Código Existente**: [Sim/Não]
- **Engenharia Reversa Necessária**: [Sim/Não]
- **Raiz do Workspace**: [Caminho absoluto]

## Regras de Localização de Código
- **Código da Aplicação**: Raiz do workspace (NUNCA em aidlc-docs/)
- **Documentação**: apenas aidlc-docs/
- **Padrões de estrutura**: Ver Regras Críticas em code-generation.md

## Progresso do Estágio
[Será preenchido conforme o fluxo de trabalho progride]
```

## Passo 5: Apresentar Mensagem de Conclusão

**Para Projetos Brownfield:**
```markdown
# 🔍 Detecção de Workspace Concluída

Descobertas da análise do workspace:
• **Tipo de Projeto**: Projeto Brownfield
• [Resumo gerado por IA das descobertas do workspace em tópicos]
• **Próximo Passo**: Prosseguindo para **Engenharia Reversa** para analisar a base de código existente...
```

**Para Projetos Greenfield:**
```markdown
# 🔍 Detecção de Workspace Concluída

Descobertas da análise do workspace:
• **Tipo de Projeto**: Projeto Greenfield
• **Próximo Passo**: Prosseguindo para **Análise de Requisitos**...
```

## Passo 6: Prosseguir Automaticamente

- **Nenhuma aprovação do usuário é necessária** - isto é apenas informativo.
- Prosseguir automaticamente para a próxima fase:
  - **Brownfield**: Engenharia Reversa (se não houver artefatos existentes) ou Análise de Requisitos (se existirem artefatos).
  - **Greenfield**: Análise de Requisitos.