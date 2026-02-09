# Constituição do [NOME_DO_PROJETO]
<!-- Exemplo: Constituição Spec, Constituição TaskFlow, etc. -->

## Princípios Fundamentais

### [NOME_DO_PRINCIPIO_1]
<!-- Exemplo: I. Primeiro a Biblioteca -->
[DESCRICAO_DO_PRINCIPIO_1]
<!-- Exemplo: Cada funcionalidade começa como uma biblioteca independente; Bibliotecas devem ser autocontidas, testáveis de forma independente e documentadas; Objetivo claro exigido - sem bibliotecas apenas organizacionais -->

### [NOME_DO_PRINCIPIO_2]
<!-- Exemplo: II. Interface CLI -->
[DESCRICAO_DO_PRINCIPIO_2]
<!-- Exemplo: Cada biblioteca expõe funcionalidade via CLI; Protocolo de entrada/saída de texto: stdin/args → stdout, erros → stderr; Suporte a formatos JSON + legível por humanos -->

### [NOME_DO_PRINCIPIO_3]
<!-- Exemplo: III. Testes Primeiro (NÃO NEGOCIÁVEL) -->
[DESCRICAO_DO_PRINCIPIO_3]
<!-- Exemplo: TDD obrigatório: Testes escritos → Aprovados pelo usuário → Testes falham → Então implementar; Ciclo Red-Green-Refactor estritamente aplicado -->

### [NOME_DO_PRINCIPIO_4]
<!-- Exemplo: IV. Testes de Integração -->
[DESCRICAO_DO_PRINCIPIO_4]
<!-- Exemplo: Áreas de foco que exigem testes de integração: Testes de contrato de nova biblioteca, Mudanças de contrato, Comunicação entre serviços, Esquemas compartilhados -->

### [NOME_DO_PRINCIPIO_5]
<!-- Exemplo: V. Observabilidade, VI. Versionamento e Mudanças Disruptivas, VII. Simplicidade -->
[DESCRICAO_DO_PRINCIPIO_5]
<!-- Exemplo: E/S de texto garante depurabilidade; Logging estruturado obrigatório; Ou: Formato MAJOR.MINOR.BUILD; Ou: Comece simples, princípios YAGNI -->

## [NOME_DA_SECAO_2]
<!-- Exemplo: Restrições Adicionais, Requisitos de Segurança, Padrões de Performance, etc. -->

[CONTEUDO_DA_SECAO_2]
<!-- Exemplo: Requisitos da stack tecnológica, padrões de conformidade, políticas de deploy, etc. -->

## [NOME_DA_SECAO_3]
<!-- Exemplo: Fluxo de Desenvolvimento, Processo de Revisão, Portões de Qualidade, etc. -->

[CONTEUDO_DA_SECAO_3]
<!-- Exemplo: Requisitos de code review, portões de teste, processo de aprovação de deploy, etc. -->

## Governança
<!-- Exemplo: A constituição prevalece sobre todas as outras práticas; Emendas exigem documentação, aprovação e plano de migração -->

[REGRAS_DE_GOVERNANCA]
<!-- Exemplo: Todos os PRs/revisões devem verificar a conformidade; A complexidade deve ser justificada; Use [ARQUIVO_DE_ORIENTACAO] para orientação de desenvolvimento em tempo de execução -->

**Versão**: [VERSAO_DA_CONSTITUICAO] | **Ratificada**: [DATA_DE_RATIFICACAO] | **Última Alteração**: [DATA_DA_ULTIMA_ALTERACAO]
<!-- Exemplo: Versão: 2.1.1 | Ratificada: 2025-06-13 | Última Alteração: 2025-07-16 -->