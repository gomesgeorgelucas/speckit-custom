---
description: Identifica o estado atual do workspace, detecta tecnologias existentes e inicializa a memória do projeto Spec Kit.
handoffs: 
  - label: Engenharia Reversa
    agent: doc.reverse-engineering
    prompt: Realize a análise do código existente para este projeto brownfield.
  - label: Definir Especificação
    agent: speckit.specify
    prompt: Inicie a especificação da nova funcionalidade. Eu quero construir...
---

## User Input

```text
$ARGUMENTS
```

Você **DEVE** considerar o input do usuário antes de prosseguir (se não estiver vazio).

## Outline

1. **Configuração e Verificação**: Tente carregar `.specify/memory/project-state.md`. Se o projeto já estiver inicializado, identifique o último estágio concluído e retome o contexto.

2. **Varredura de Workspace**: Escaneie a raiz do repositório em busca de código-fonte, arquivos de configuração de build e indicadores de infraestrutura para determinar se o projeto é **Greenfield** (novo) ou **Brownfield** (existente).

3. **Inicialização da Memória**: Crie ou atualize o arquivo de estado do projeto com as descobertas da varredura e as regras de localização de código.

4. **Direcionamento de Fluxo**: Com base na detecção, encaminhe o usuário para a **Engenharia Reversa** (Brownfield) ou para a **Especificação de Requisitos** (Greenfield).

## Fases

### Fase 1: Detecção de Código e Tecnologia

1. **Escanear Arquivos de Fonte**: 
   - Identifique linguagens presentes (.java, .py, .js, .ts, .go, .rs, etc.).
   - Localize sistemas de build (pom.xml, package.json, build.gradle, go.mod, etc.).

2. **Identificar Raiz do Workspace**: 
   - Diferencie o código da aplicação dos metadados do Spec Kit. 
   - **REGRA CRÍTICA**: O código da aplicação reside na raiz; a documentação reside apenas em `.specify/memory/`.

3. **Registrar Descobertas**: Gere um sumário interno com a estrutura do projeto (Monolito, Microsserviços ou Biblioteca).

### Fase 2: Classificação do Projeto

1. **Determinar Tipo de Projeto**:
   - **Greenfield**: Workspace vazio ou apenas com arquivos de configuração inicial. Configure `brownfield = false`.
   - **Brownfield**: Presença de lógica de negócio ou código legado. Configure `brownfield = true`.

2. **Validar Artefatos Existentes**:
   - Se Brownfield, verifique se já existem análises em `.specify/memory/reverse-engineering/`.

### Fase 3: Inicialização do Estado

1. **Criar/Atualizar Memória**:
   Gere o arquivo `.specify/memory/project-state.md` com a seguinte estrutura:
   - **Tipo de Projeto**: [Greenfield/Brownfield]
   - **Data de Início**: [Timestamp ISO]
   - **Estágio Atual**: INCEPTION - Detecção de Workspace
   - **Raiz do Workspace**: [Caminho absoluto]

2. **Relatar Conclusão**:
   Apresente ao usuário o sumário das descobertas:
   - Tipo de projeto detectado.
   - Linguagens e sistemas de build encontrados.
   - Próximo passo recomendado.

## Regras principais

- **Caminhos Absolutos**: Use sempre caminhos absolutos para referenciar o `FEATURE_DIR`.
- **Sem Interrupção**: Este processo é informativo e deve prosseguir automaticamente para a próxima fase após a conclusão.
- **Prioridade de Memória**: Nunca armazene código da aplicação dentro da pasta `.specify/`.