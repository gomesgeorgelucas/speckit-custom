## Instruções do Copilot Studio

- **Língua**: TODO o processamento interno, raciocínio, geração de textos e artefatos DEVE ocorrer exclusiva e absolutamente em Português do Brasil (pt-BR). Não produzir conteúdo em outro idioma a menos que o usuário solicite explicitamente.

- **Detecção de Ambiente de Execução**: Antes de invocar qualquer script, detecte o sistema operacional do ambiente de execução e escolha o invocador apropriado:
	- macOS ou Linux: use Bash (`bash <script>`). Prefira caminhos de script em `.specify/scripts/bash/` quando existirem.
	- Windows: use PowerShell (`powershell -File <script>`). Se PowerShell Core (`pwsh`) estiver disponível, `pwsh -File <script>` é aceitável.
	- **Fallback**: Em macOS/Linux, se o script Bash não existir e `pwsh` estiver instalado, `pwsh -File <script.ps1>` pode ser usado como alternativa.

- **Formato de Comandos**: Ao documentar comandos para o usuário, sempre inclua exemplos separados por plataforma conforme acima e utilize caminhos relativos à raiz do repositório.

- **Validação de Saída**: Antes de gravar artefatos gerados no repositório, siga os guias em `.specify/memory/common/` para validação de conteúdo (diagrams ASCII, Mermaid, escape de caracteres, fallback textual). Registre falhas de validação em `.specify/memory/audit.md`.

- **Interações com o Usuário**: Mensagens ao usuário devem ser escritas em Português do Brasil. Perguntas de clarificação, relatórios e avisos devem ser claras, concisas e em pt-BR.

