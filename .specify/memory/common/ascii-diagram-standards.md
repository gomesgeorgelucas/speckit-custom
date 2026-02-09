# Padrões para Diagramas ASCII

## OBRIGATÓRIO: Use Apenas ASCII Básico

**CRÍTICO**: SEMPRE use caracteres ASCII básicos para diagramas (máxima compatibilidade).

### ✅ PERMITIDO: `+` `-` `|` `^` `v` `<` `>` e texto alfanumérico

### ❌ PROIBIDO: Caracteres Unicode de desenho de caixa
- NÃO: `┌` `─` `│` `└` `┐` `┘` `├` `┤` `┬` `┴` `┼` `▼` `▲` `►` `◄`
- Motivo: Renderização inconsistente em diferentes fontes/plataformas

## Padrões de Diagramas ASCII Comuns

### CRÍTICO: Regra de Largura de Caracteres
**Cada linha em uma caixa DEVE ter EXATAMENTE a mesma contagem de caracteres (incluindo espaços)**

✅ CORRETO (todas as linhas = 67 caracteres):
```
+---------------------------------------------------------------+
|                      Nome do Componente                       |
|  Texto de descrição aqui                                      |
+---------------------------------------------------------------+
```

❌ ERRADO (larguras inconsistentes):
```
+---------------------------------------------------------------+
|                      Nome do Componente                       |
|  Texto de descrição aqui                                   |
+---------------------------------------------------------------+
```

### Padrão de Caixa
```
+-----------------------------------------------------+
|                                                     |
|              Aplicação de Calculadora               |
|                                                     |
|  Fornece operações aritméticas básicas para usuários|
|  através de uma interface baseada na web            |
|                                                     |
+-----------------------------------------------------+
```

### Caixas Aninhadas
```
+-------------------------------------------------------+
|              Servidor Web (PHP Runtime)               |
|  +-------------------------------------------------+  |
|  |  index.php (Aplicação Monolítica)               |  |
|  |  +-------------------------------------------+  |  |
|  |  |  Template HTML (Camada de Visão)          |  |  |
|  |  |  - Renderização de formulário             |  |  |
|  |  |  - Exibição de resultados                 |  |  |
|  |  +-------------------------------------------+  |  |
|  +-------------------------------------------------+  |
+-------------------------------------------------------+
```

### Setas e Conexões
```
+----------+
|  Origem  |
+----------+
     |
     | HTTP POST
     v
+----------+
|  Destino |
+----------+
```

### Fluxo Horizontal
```
+---------+     +---------+     +---------+
| Passo 1 | --> | Passo 2 | --> | Passo 3 |
+---------+     +---------+     +---------+
```

### Fluxo Vertical com Rótulos
```
Fluxo de Ação do Usuário:
    |
    v
+----------+
| Entrada  |
+----------+
    |
    | valida
    v
+----------+
| Processo |
+----------+
    |
    | retorna
    v
+----------+
|  Saída   |
+----------+
```

## Validação

Antes de criar diagramas:
- [ ] Apenas ASCII básico: `+` `-` `|` `^` `v` `<` `>`
- [ ] Sem caracteres Unicode de desenho de caixa
- [ ] Espaços (não tabs) para alinhamento
- [ ] Cantos usam `+`
- [ ] **TODAS as linhas da caixa com a mesma largura de caracteres** (conte os caracteres, incluindo espaços)
- [ ] Teste: Verifique se os cantos se alinham verticalmente em uma fonte monoespaçada

## Alternativa

Para diagramas complexos, use Mermaid (veja `content-validation.md`)