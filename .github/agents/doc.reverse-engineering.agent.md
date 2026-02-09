---
description: Analisa a base de código existente e gera artefatos de design abrangentes para projetos brownfield.
handoffs:
  - label: Iniciar Especificação da Funcionalidade
    agent: speckit.specify
    prompt: Com base na análise de engenharia reversa, vamos definir a nova funcionalidade. Eu quero...
  - label: Clarificar Requisitos
    agent: speckit.clarify
    prompt: Clarify specification requirements based on existing code analysis.
---

# Reverse Engineering

**Propósito**: Analisar a base de código existente e gerar artefatos de design abrangentes.

**Executar quando**: Projeto Brownfield detectado (código existente encontrado no workspace).

**Pular quando**: Projeto Greenfield (sem código existente).

**Comportamento de reexecução**: Sempre reexecutar quando um projeto brownfield for detectado, mesmo que os artefatos existam. Isso garante que os artefatos reflitam o estado atual do código.

## Passo 1: Descoberta de Múltiplos Pacotes

### 1.1 Escanear Workspace
- Todos os pacotes (não apenas os mencionados).
- Relacionamentos entre pacotes via arquivos de configuração.
- Tipos de pacotes: Aplicação, CDK/Infraestrutura, Modelos, Clientes, Testes.

### 1.2 Entender o Contexto de Negócio
- O negócio principal que o sistema implementa como um todo.
- A visão geral de negócio de cada pacote.
- Lista de Transações de Negócio que estão implementadas no sistema.

### 1.3 Descoberta de Infraestrutura
- Pacotes CDK (package.json com dependências CDK).
- Terraform (arquivos .tf).
- CloudFormation (templates .yaml/.json).
- Scripts de implantação.

### 1.4 Descoberta do Sistema de Build
- Sistemas de build: Maven, Gradle, npm, etc.
- Arquivos de configuração para declarações do sistema de build.
- Dependências de build entre pacotes.

### 1.5 Descoberta da Arquitetura de Serviços
- Funções Lambda (handlers, triggers).
- Serviços de container (configurações Docker/ECS).
- Definições de API (modelos Smithy, specs OpenAPI).
- Armazenamento de dados (DynamoDB, S3, etc.).

### 1.6 Análise de Qualidade de Código
- Linguagens de programação e frameworks.
- Indicadores de cobertura de teste.
- Configurações de linting.
- Pipelines de CI/CD.

## Passo 1: Gerar Documentação de Visão Geral de Negócio

Criar `.specify/memory/reverse-engineering/business-overview.md`:

```markdown
# Business Overview

## Business Context Diagram
[Mermaid diagram showing the Business Context]

## Business Description
- **Business Description**: [Overall Business description of what the system does]
- **Business Transactions**: [List of Business Transactions that the system implements and their descriptions]
- **Business Dictionary**: [Business dictionary terms that the system follows and their meaning]

## Component Level Business Descriptions
### [Package/Component Name]
- **Purpose**: [What it does from the business perspective]
- **Responsibilities**: [Key responsibilities]
```

## Passo 2: Gerar Documentação de Arquitetura

Criar `.specify/memory/reverse-engineering/architecture.md`:

```markdown
# System Architecture

## System Overview
[High-level description of the system]

## Architecture Diagram
[Mermaid diagram showing all packages, services, data stores, relationships]

## Component Descriptions
### [Package/Component Name]
- **Purpose**: [What it does]
- **Responsibilities**: [Key responsibilities]
- **Dependencies**: [What it depends on]
- **Type**: [Application/Infrastructure/Model/Client/Test]

## Data Flow
[Mermaid sequence diagram of key workflows]

## Integration Points
- **External APIs**: [List with purposes]
- **Databases**: [List with purposes]
- **Third-party Services**: [List with purposes]

## Infrastructure Components
- **CDK Stacks**: [List with purposes]
- **Deployment Model**: [Description]
- **Networking**: [VPC, subnets, security groups]
```

## Passo 3: Gerar Documentação de Estrutura de Código

Criar `.specify/memory/reverse-engineering/code-structure.md`:

```markdown
# Code Structure

## Build System
- **Type**: [Maven/Gradle/npm/etc]
- **Configuration**: [Key build files and settings]

## Key Classes/Modules
[Mermaid class diagram or module hierarchy]

### Existing Files Inventory
[List all source files with their purposes - these are candidates for modification in brownfield projects]

**Example format**:
- `[path/to/file]` - [Purpose/responsibility]

## Design Patterns
### [Pattern Name]
- **Location**: [Where used]
- **Purpose**: [Why used]
- **Implementation**: [How implemented]

## Critical Dependencies
### [Dependency Name]
- **Version**: [Version number]
- **Usage**: [How and where used]
- **Purpose**: [Why needed]
```

## Passo 4: Gerar Documentação de API

Criar `.specify/memory/reverse-engineering/api-documentation.md`:

```markdown
# API Documentation

## REST APIs
### [Endpoint Name]
- **Method**: [GET/POST/PUT/DELETE]
- **Path**: [/api/path]
- **Purpose**: [What it does]
- **Request**: [Request format]
- **Response**: [Response format]

## Internal APIs
### [Interface/Class Name]
- **Methods**: [List with signatures]
- **Parameters**: [Parameter descriptions]
- **Return Types**: [Return type descriptions]

## Data Models
### [Model Name]
- **Fields**: [Field descriptions]
- **Relationships**: [Related models]
- **Validation**: [Validation rules]
```

## Passo 5: Gerar Inventário de Componentes

Criar `.specify/memory/reverse-engineering/component-inventory.md`:

```markdown
# Component Inventory

## Application Packages
- [Package name] - [Purpose]

## Infrastructure Packages
- [Package name] - [CDK/Terraform] - [Purpose]

## Shared Packages
- [Package name] - [Models/Utilities/Clients] - [Purpose]

## Test Packages
- [Package name] - [Integration/Load/Unit] - [Purpose]

## Total Count
- **Total Packages**: [Number]
- **Application**: [Number]
- **Infrastructure**: [Number]
- **Shared**: [Number]
- **Test**: [Number]
```

## Passo 6: Gerar Documentação de Stack Tecnológica

Criar `.specify/memory/reverse-engineering/technology-stack.md`:

```markdown
# Technology Stack

## Programming Languages
- [Language] - [Version] - [Usage]

## Frameworks
- [Framework] - [Version] - [Purpose]

## Infrastructure
- [Service] - [Purpose]

## Build Tools
- [Tool] - [Version] - [Purpose]

## Testing Tools
- [Tool] - [Version] - [Purpose]
```

## Passo 7: Gerar Documentação de Dependências

Criar `.specify/memory/reverse-engineering/dependencies.md`:

```markdown
# Dependencies

## Internal Dependencies
[Mermaid diagram showing package dependencies]

### [Package A] depends on [Package B]
- **Type**: [Compile/Runtime/Test]
- **Reason**: [Why dependency exists]

## External Dependencies
### [Dependency Name]
- **Version**: [Version]
- **Purpose**: [Why used]
- **License**: [License type]
```

## Passo 8: Gerar Avaliação de Qualidade de Código

Criar `.specify/memory/reverse-engineering/code-quality-assessment.md`:

```markdown
# Code Quality Assessment

## Test Coverage
- **Overall**: [Percentage or Good/Fair/Poor/None]
- **Unit Tests**: [Status]
- **Integration Tests**: [Status]

## Code Quality Indicators
- **Linting**: [Configured/Not configured]
- **Code Style**: [Consistent/Inconsistent]
- **Documentation**: [Good/Fair/Poor]

## Technical Debt
- [Issue description and location]

## Patterns and Anti-patterns
- **Good Patterns**: [List]
- **Anti-patterns**: [List with locations]
```

## Passo 9: Criar Arquivo de Timestamp

Criar `.specify/memory/reverse-engineering/reverse-engineering-metadata.md`:

```markdown
# Reverse Engineering Metadata

**Analysis Date**: [ISO timestamp]
**Analyzer**: Spec Kit
**Workspace**: [Workspace path]
**Total Files Analyzed**: [Number]

## Artifacts Generated
- [x] architecture.md
- [x] code-structure.md
- [x] api-documentation.md
- [x] component-inventory.md
- [x] technology-stack.md
- [x] dependencies.md
- [x] code-quality-assessment.md
```

## Passo 10: Atualizar Rastreamento de Estado

Atualizar `.specify/memory/project-state.md`:

```markdown
## Reverse Engineering Status
- [x] Reverse Engineering - Concluído em [timestamp]
- **Localização dos Artefatos**: .specify/memory/reverse-engineering/
```

## Passo 11: Apresentar Mensagem de Conclusão ao Usuário

```markdown
# 🔍 Engenharia Reversa Concluída

[Resumo gerado por IA das principais descobertas da análise em formato de tópicos]

> **📋 <u>**REVISÃO NECESSÁRIA:**</u>**
> Por favor, examine os artefatos de engenharia reversa em: `.specify/memory/reverse-engineering/`

> **🚀 <u>**O QUE VEM DEPOIS?**</u>**
>
> **Você pode:**
>
> 🔧 **Solicitar Alterações** - Peça modificações na análise de engenharia reversa se necessário
> ✅ **Aprovar e Continuar** - Aprove a análise e prossiga para a **Especificação de Requisitos** com `/speckit.specify`
```

## Passo 12: Aguardar Aprovação do Usuário

- **OBRIGATÓRIO**: Não prossiga até que o usuário aprove explicitamente.
- **OBRIGATÓRIO**: Registre a resposta do usuário em `.specify/memory/audit.md` com o input bruto completo.
