# devcontainer-with-claude

> [Read in English](README.en.md)

Template de Dev Container para projetos Python e Node.js com Claude Code integrado.

## O que é isso

Dev Containers permitem rodar um ambiente de desenvolvimento completo dentro de um container Docker, garantindo que qualquer pessoa que clone o repositório tenha exatamente o mesmo ambiente — sem "funciona na minha máquina".

Este template vai além: ele vem com o [Claude Code](https://claude.ai/code) pré-instalado e configurado, montando as credenciais e memória do seu Claude local diretamente no container. Assim, o assistente de IA já está pronto para uso assim que o container sobe, com todo o contexto que você já tem no host.

## Por que usar

Toda vez que começo um projeto novo, quero sair do zero com Claude já configurado — sem perder tempo instalando Node, autenticando o Claude Code ou copiando configurações manualmente. Este template resolve isso: clonou, abriu no VSCode, container subiu, Claude funcionando. Em minutos o projeto está pronto para desenvolvimento assistido por IA.

Outro motivo importante: usar `claude --dangerously-skip-permissions` com segurança. Essa flag remove todas as confirmações de permissão do Claude Code, deixando-o operar de forma totalmente autônoma — mas usá-la direto na máquina host é arriscado, pois o Claude pode modificar arquivos e executar comandos sem pedir confirmação. Dentro do container o risco é isolado: o que acontece no container fica no container, protegendo o sistema host.

O container não expõe chaves SSH, então o Claude consegue fazer commits mas **não consegue fazer push** — operações autônomas ficam contidas no ambiente local.

## O que inclui

- **Python 3.12** como imagem base
- **Node.js + npm** para desenvolvimento e para rodar o Claude Code
- **Claude Code** instalado automaticamente no `postCreateCommand`
- Mounts para compartilhar configurações do host:
  - `~/.claude` — configurações e memória do Claude
  - `~/.claude.json` — credenciais do Claude Code
  - `~/.gitconfig` — configuração do git (nome e email para commits)
- Extensões VSCode: Python + Pylance

## Como usar

1. Copie a pasta `.devcontainer/` para o seu projeto
2. Substitua `<NAME-OF-PROJECT>` pelo nome do seu projeto em `devcontainer.json`
3. Abra o projeto no VSCode e clique em **Reopen in Container**

> Caso a notificação não apareça, abra a paleta de comandos (`Ctrl+Shift+P` / `Cmd+Shift+P`) e execute:
> `Dev Containers: Reopen in Container`

**Dica rápida:** para baixar só o `devcontainer.json` sem clonar o repositório, rode na raiz do seu projeto:

```bash
mkdir -p .devcontainer && curl -sL https://raw.githubusercontent.com/brlga002/devcontainer-with-claude/main/.devcontainer/devcontainer.json -o .devcontainer/devcontainer.json
```

## Customizações

### Rodar `npm install` automaticamente ao subir o container

Por padrão o `postCreateCommand` instala apenas o Claude Code. Se o seu projeto Node.js precisa instalar dependências automaticamente, edite o campo em `devcontainer.json`:

```json
"postCreateCommand": "npm install && npm install -g @anthropic-ai/claude-code"
```

Ou combinando com Python:

```json
"postCreateCommand": "pip install -r requirements.txt 2>/dev/null || true && npm install && npm install -g @anthropic-ai/claude-code"
```

## Pré-requisitos

- [Docker](https://www.docker.com/)
- [VSCode](https://code.visualstudio.com/) com a extensão [Dev Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)
- Claude Code autenticado na máquina host (`~/.claude.json` existente)
