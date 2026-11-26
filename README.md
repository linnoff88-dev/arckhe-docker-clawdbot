ï»¿<p align="center">
  <img src="assets/migration-banner.jpg" alt="ClawdBot ê¢â â MoltBot Migration" width="100%">
</p>

> # ê¢Å¡Â ê¯Â¸Â Clawdbot estêÂ¡ sendo renomeado para **Moltbot**
> 
> Estamos trabalhando em uma versêÂ£o atualizada. Quando estiver estêÂ¡vel, o novo repositêÂ³rio serêÂ¡ disponibilizado para migraêÂ§êÂ£o.
> 
> **Enquanto isso, continue usando este repositêÂ³rio (docker-clawdbot) normalmente.**

---

<p align="center">
  <img src="assets/clawdbot-banner.jpg" alt="Clawdbot - AI Assistant" width="100%">
</p>

# ê°Å¸Â¦Å¾ Docker Clawdbot

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


Setup Docker para [Clawdbot](https://docs.clawd.bot) ê¢â¬â assistente pessoal de IA com hardening de seguranêÂ§a pronto pra usar.

<p align="center">
  <img src="assets/clawdbot-robot.jpg" alt="Clawdbot Robot" width="300">
</p>

## Funcionalidades

- ê°Å¸ââ **SeguranêÂ§a reforêÂ§ada** ê¢â¬â segue o [Top 10 Security Checklist](SECURITY.md)
- ê°Å¸ÂÂ³ **Setup com um comando** ê¢â¬â `docker compose up -d`
- ê°Å¸âÂ **Secrets via variêÂ¡veis de ambiente** ê¢â¬â sem credenciais em texto puro
- ê°Å¸âÂ¤ **Container nêÂ£o-root** ê¢â¬â roda como usuêÂ¡rio sem privilêÂ©gios
- ê°Å¸âÂ **Logging habilitado** ê¢â¬â trilha de auditoria por padrêÂ£o
- ê°Å¸âÂ± **Pronto pra Telegram** ê¢â¬â sêÂ³ adicionar o token do bot
- ê°Å¸Å½â¢ê¯Â¸Â **TranscriêÂ§êÂ£o de êÂ¡udio** ê¢â¬â Faster Whisper incluso (opcional)
- ê°Å¸ÂªÅ¸ **CompatêÂ­vel com Windows** ê¢â¬â `.gitattributes` forêÂ§a finais LF, Dockerfile corrige CRLF

## InêÂ­cio RêÂ¡pido

### PrêÂ©-requisitos

| Plataforma | Requisito | InstalaêÂ§êÂ£o |
|------------|-----------|------------|
| **Windows** | Docker Desktop | [docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop/) |
| **Windows** | Git | [git-scm.com](https://git-scm.com/download/win) |
| **Mac** | Docker Desktop | [docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop/) |
| **Linux** | Docker Engine + Compose | `curl -fsSL https://get.docker.com \| sh` |

> ê¢Å¡Â ê¯Â¸Â **UsuêÂ¡rios Windows:** Certifique-se de que o **Docker Desktop estêÂ¡ rodando** antes de continuar. Verifique o êÂ­cone do Docker na bandeja do sistema. Se o WSL mostrar `docker-desktop Stopped`, abra o Docker Desktop e aguarde ele iniciar.

### 1. Clone o repo

**Linux / Mac:**
```bash
git clone https://github.com/linnoff88-dev/docker-clawdbot.git
cd docker-clawdbot
```

**Windows (CMD ou PowerShell):**
```cmd
git clone https://github.com/linnoff88-dev/docker-clawdbot.git
cd docker-clawdbot
```

### 2. Configure o ambiente

**Linux / Mac:**
```bash
cp .env.example .env
nano .env  # Fill in your API keys
```

**Windows (CMD):**
```cmd
copy .env.example .env
notepad .env
```

**Windows (PowerShell):**
```powershell
Copy-Item .env.example .env
notepad .env
```

> ê¢Å¡Â ê¯Â¸Â **Importante:** VocêÂª DEVE criar E editar o arquivo `.env` antes de rodar `docker compose up`.

Abra o arquivo `.env` e substitua os valores placeholder pelas suas chaves reais:

```env
# ê¢ÂÅ ERRADO (placeholder ê¢â¬â nêÂ£o vai funcionar):
ANTHROPIC_API_KEY=sk-ant-your-key-here

# ê¢Åâ¦ CERTO (sua chave real):
ANTHROPIC_API_KEY=sk-ant-abc123-your-actual-key
```

**MêÂ­nimo pra comeêÂ§ar:** VocêÂª precisa de pelo menos uma chave de provedor LLM (veja a tabela acima) e um token de gateway.

Pra gerar um token de gateway seguro:
```bash
# Linux / Mac:
openssl rand -hex 24

# Windows (PowerShell):
-join ((1..48) | ForEach-Object { '{0:x}' -f (Get-Random -Max 16) })

# Or just use any long random string (at least 24 characters)
```

**ObrigatêÂ³rios:**
- `GATEWAY_AUTH_TOKEN` ê¢â¬â gere com `openssl rand -hex 24`
- **Um provedor LLM** (escolha um ou mais):

| Provedor | VariêÂ¡vel de Ambiente | Obter Chave |
|----------|---------------------|-------------|
| Anthropic (Claude) | `ANTHROPIC_API_KEY` | [console.anthropic.com](https://console.anthropic.com/) |
| OpenAI (GPT) | `OPENAI_API_KEY` | [platform.openai.com](https://platform.openai.com/api-keys) |
| OpenRouter (multi-modelo) | `OPENROUTER_API_KEY` | [openrouter.ai](https://openrouter.ai/) |
| Google (Gemini) | `GOOGLE_API_KEY` | [ai.google.dev](https://ai.google.dev/) |

> ê°Å¸âÂ¡ **Dica:** O OpenRouter dêÂ¡ acesso a mêÂºltiplos modelos (Claude, GPT, Llama, Gemini) com uma êÂºnica API key ê¢â¬â incluindo modelos gratuitos.

**Opcionais:**
- `TELEGRAM_BOT_TOKEN` ê¢â¬â pegue com o [@BotFather](https://t.me/BotFather)
- `BRAVE_API_KEY` ê¢â¬â para busca na web

### 3. Build e execuêÂ§êÂ£o
```bash
docker compose up -d
```

> ê°Å¸âÂ¡ **Primeira execuêÂ§êÂ£o** leva alguns minutos pra fazer o build da imagem (baixa Node.js, FFmpeg, etc). As execuêÂ§êÂµes seguintes sêÂ£o instantêÂ¢neas.

> ê¢Å¡Â ê¯Â¸Â **Erro no Windows "pipe/dockerDesktopLinuxEngine"?** O Docker Desktop nêÂ£o estêÂ¡ rodando. Abra ele pelo menu Iniciar e aguarde atêÂ© mostrar "Docker is running", depois tente novamente.

### 4. Acesse o Webchat

Abra no seu navegador:
```
http://localhost:18789/chat
```

Quando solicitado, insira seu `GATEWAY_AUTH_TOKEN` do arquivo `.env` para autenticar.

> ê°Å¸âÂ¡ **Dica:** VocêÂª tambêÂ©m pode acessar diretamente com: `http://localhost:18789/?token=SEU_TOKEN`

### 5. Verifique o status
```bash
docker compose logs -f
```

### 6. ConfiguraêÂ§êÂ£o pêÂ³s-instalaêÂ§êÂ£o

Depois que o container estiver rodando, configure seu Clawdbot:

```bash
# Run the interactive setup wizard (API keys, channels, preferences)
docker compose exec -it clawdbot clawdbot configure

# Or just auto-fix detected issues (e.g. enable Telegram if token is set)
docker compose exec -it clawdbot clawdbot doctor --fix

# Check overall health
docker compose exec clawdbot clawdbot status
```

| Comando | O que faz |
|---------|-----------|
| `clawdbot configure` | Wizard interativo ê¢â¬â configura API keys, canais (Telegram, WhatsApp, etc.), preferêÂªncias de modelo |
| `clawdbot doctor --fix` | Auto-detecta e corrige problemas de config (ex: Telegram configurado mas nêÂ£o habilitado) |
| `clawdbot doctor` | Mesma verificaêÂ§êÂ£o, mas sêÂ³ **mostra** os problemas sem corrigir |
| `clawdbot status` | Mostra status do gateway, canais conectados, info do modelo |

## ConfiguraêÂ§êÂ£o do Telegram

1. Crie um bot com o [@BotFather](https://t.me/BotFather)
2. Adicione o token no `.env`:
   ```
   TELEGRAM_BOT_TOKEN=123456:ABC-your-token
   ```
3. Reinicie: `docker compose restart`
4. Mande uma mensagem pro seu bot ê¢â¬â ele vai te dar um cêÂ³digo de pareamento
5. Aprove dentro do container:
   ```bash
   docker compose exec clawdbot clawdbot pairing approve telegram <code>
   ```

## SeguranêÂ§a

Este setup implementa 7 de 10 medidas de hardening de seguranêÂ§a automaticamente. Veja [SECURITY.md](SECURITY.md) para o checklist completo e passos manuais.

### PadrêÂµes principais:
- Gateway faz bind apenas em `127.0.0.1`
- PolêÂ­tica de DM requer aprovaêÂ§êÂ£o de pareamento
- Arquivos de config têÂªm `chmod 600`
- Container roda como nêÂ£o-root
- Logging e diagnêÂ³sticos habilitados

## Volumes

| Volume | Finalidade |
|--------|------------|
| `clawdbot-data` | Dados de config e sessêÂ£o |
| `clawdbot-workspace` | Workspace do agente (AGENTS.md, memêÂ³ria, etc) |
| `clawdbot-logs` | Arquivos de log (`/home/clawdbot/logs`) |

## Comandos êÅ¡teis

```bash
# View logs
docker compose logs -f

# Enter container shell
docker compose exec clawdbot bash

# Restart
docker compose restart

# Update Clawdbot
docker compose build --no-cache
docker compose up -d

# Check clawdbot status
docker compose exec clawdbot clawdbot status
```

## Isolamento de Rede

Por padrêÂ£o, o container tem acesso êÂ  internet (necessêÂ¡rio pras chamadas de API). Para isolamento total:

```yaml
# In docker-compose.yml, change:
networks:
  clawdbot-net:
    internal: true  # No internet access
```

ê¢Å¡Â ê¯Â¸Â Isso bloqueia chamadas de API pra Anthropic/OpenAI. Use apenas se vocêÂª tiver um setup de modelo local.

## Canais de Acesso

MêÂºltiplas formas de interagir com seu Clawdbot de qualquer lugar:

| Canal | Tipo | Acesso | ConfiguraêÂ§êÂ£o |
|-------|------|--------|--------------|
| ê°Å¸âÂ± **Telegram** | Mensageiro | Qualquer lugar (mobile/desktop) | Crie um bot via [@BotFather](https://t.me/BotFather) |
| ê°Å¸âÂ² **WhatsApp** | Mensageiro | Qualquer lugar (mobile/desktop) | Vincule via QR code (`clawdbot channels login`) |
| ê°Å¸âÂ¬ **Webchat** | Interface Web | Rede local / VPN | Integrado, roda na porta do gateway |
| ê°Å¸ÅÂ **Webchat (pêÂºblico)** | Interface Web | Qualquer lugar | Proxy reverso Nginx + certificado SSL |
| ê°Å¸ââ **Tailscale** | VPN | Qualquer lugar (zero-trust) | Instale Tailscale no servidor + dispositivos |
| ê°Å¸âÅ **Discord** | Mensageiro | Qualquer lugar | Crie um bot via Discord Developer Portal |
| ê°Å¸âÂ¼ **Slack** | Mensageiro | Qualquer lugar | Crie um Slack app + bot token |
| ê°Å¸âÂµ **Signal** | Mensageiro | Qualquer lugar | Signal CLI ou dispositivo vinculado |
| ê°Å¸Å¸Â¢ **Matrix** | Mensageiro | Qualquer lugar | Homeserver Matrix + conta de bot |

### Qual devo usar?

**Setup mais simples:** Telegram ê¢â¬â um bot token e pronto.

**Mais privado:** Signal ou Tailscale + Webchat.

**Acesso de qualquer lugar sem apps extras:** Telegram + WhatsApp (vocêÂª jêÂ¡ tem eles no celular).

**Melhor pra times/trabalho:** Slack ou Discord.

**Acesso remoto mais seguro ao Webchat:** Tailscale ê¢â¬â VPN zero-trust, sem portas abertas, funciona de qualquer rede.

### Multi-canal
VocêÂª pode habilitar **mêÂºltiplos canais simultaneamente**. Todos os canais compartilham o mesmo agente, memêÂ³ria e workspace. Mensagens de qualquer canal chegam no mesmo assistente.

ê¢Å¡Â ê¯Â¸Â **Mensagens entre canais sêÂ£o restritas** por design ê¢â¬â o bot nêÂ£o vaza dados entre canais.

## Acesso ao Webchat (Remoto)

O gateway faz bind em `127.0.0.1` (loopback). Para acessar o Webchat de outra mêÂ¡quina, use um **têÂºnel SSH**:

```bash
# On your local machine (PC/Mac):
ssh -L 18789:localhost:18789 root@your-server-ip
```

Depois abra no seu navegador:
```
http://127.0.0.1:18789/chat
```

Essa êÂ© a forma mais segura de acessar a interface web remotamente ê¢â¬â sem portas expostas, criptografado via SSH.

## WhatsApp: Dicas para NêÂºmero Pessoal

Se vocêÂª usar seu **nêÂºmero pessoal do WhatsApp** (modo self-chat), fique atento:

- ê¢Å¡Â ê¯Â¸Â Por padrêÂ£o, qualquer pessoa que te mandar mensagem pode receber uma resposta de cêÂ³digo de pareamento do bot
- ê¢Åâ¦ **SoluêÂ§êÂ£o:** Defina `dmPolicy: allowlist` com apenas seu nêÂºmero pra evitar isso:
```json
{
  "channels": {
    "whatsapp": {
      "selfChatMode": true,
      "dmPolicy": "allowlist",
      "allowFrom": ["+5511999999999"]
    }
  }
}
```
- ê°Å¸ââ **Recomendado:** Migre pra um **nêÂºmero dedicado** o mais rêÂ¡pido possêÂ­vel pra um setup mais limpo

## Ferramentas e Skills Recomendadas

Turbine seu Clawdbot com essas ferramentas adicionais:

### ê°Å¸âºÂ  Ferramentas CLI

| Ferramenta | InstalaêÂ§êÂ£o | Finalidade |
|------------|------------|------------|
| [Codex CLI](https://github.com/openai/codex) | `npm i -g @openai/codex` | Agente de cêÂ³digo IA (OpenAI) |
| [agent-browser](https://github.com/vercel-labs/agent-browser) | `npm i -g agent-browser` | AutomaêÂ§êÂ£o de navegador headless pra agentes IA |
| FFmpeg | `apt install ffmpeg` | Processamento de êÂ¡udio/vêÂ­deo |
| Faster Whisper | `pip install faster-whisper` | TranscriêÂ§êÂ£o de êÂ¡udio local |

### ê°Å¸Å½Â¨ ServiêÂ§os de API

| ServiêÂ§o | Finalidade | PreêÂ§o |
|---------|------------|-------|
| [OpenRouter](https://openrouter.ai) | Gateway pra mêÂºltiplos LLMs (modelos gratuitos disponêÂ­veis) | Tier gratuito + pay-per-use |
| [Kie.ai](https://kie.ai) | GeraêÂ§êÂ£o de imagem, vêÂ­deo e mêÂºsica (Veo 3.1, Flux, Suno) | CrêÂ©ditos |
| [ElevenLabs](https://elevenlabs.io) | Text-to-speech (vozes realistas) | Tier gratuito + pago |
| [Gamma](https://gamma.app) | ApresentaêÂ§êÂµes e documentos com IA | Tier gratuito + pago |
| [HeyGen](https://heygen.com) | Avatares de vêÂ­deo com IA | CrêÂ©ditos |

### ê°Å¸âÅ¡ Skills (para Codex / Claude Code)

| Skill | InstalaêÂ§êÂ£o | Finalidade |
|-------|------------|------------|
| [Remotion Skills](https://github.com/linnoff88-dev/remotion-skills) | Copie para `.codex/skills/` | Crie vêÂ­deos programaticamente com React |

```bash
# Install Remotion Skills for Codex
git clone https://github.com/linnoff88-dev/remotion-skills.git /tmp/remotion-skills
mkdir -p .codex/skills
cp -r /tmp/remotion-skills/skills/remotion .codex/skills/
```

### ê°Å¸Â¤â OrganizaêÂ§êÂ£o de LLMs

EstratêÂ©gia recomendada de modelos:

| Modelo | Provedor | Caso de Uso | Custo |
|--------|----------|-------------|-------|
| Claude Opus 4.5 | Anthropic Max | Assistente principal (conversas, tarefas) | Plano mensal |
| gpt-5.2-codex | OpenAI ChatGPT Team | GeraêÂ§êÂ£o de cêÂ³digo (prioridade) | Plano mensal |
| Modelos gratuitos | OpenRouter | Sub-agentes, tarefas secundêÂ¡rias | Gratuito |

**Modelos gratuitos no OpenRouter:** DeepSeek R1, Llama 3.1 405B, Llama 3.3 70B, Gemini 2.0 Flash, Qwen3 Coder

## Requisitos

- Docker Engine 24+
- Docker Compose v2+
- Pelo menos 2GB de RAM (4GB recomendado com Whisper)

## SoluêÂ§êÂ£o de Problemas

### Windows

| Erro | Causa | SoluêÂ§êÂ£o |
|------|-------|---------|
| `open //./pipe/dockerDesktopLinuxEngine: O sistema nêÂ£o pode encontrar o arquivo` | Docker Desktop nêÂ£o estêÂ¡ rodando | Abra o Docker Desktop e aguarde ele iniciar |
| `.env not found` | Arquivo de config faltando | Execute `copy .env.example .env` e edite com `notepad .env` |
| `the attribute version is obsolete` | Formato antigo do docker-compose | Ignore (inofensivo) ou atualize pra versêÂ£o mais recente do docker-clawdbot |
| `WSL docker-desktop Stopped` | WSL nêÂ£o iniciou | Abra o Docker Desktop ê¢â¬â ele inicia o WSL automaticamente |
| Build trava ou falha | RAM insuficiente | Garanta pelo menos 4GB alocados pro Docker (Settings ê¢â â Resources) |

### Linux / Mac

| Erro | Causa | SoluêÂ§êÂ£o |
|------|-------|---------|
| `permission denied` | NêÂ£o estêÂ¡ no grupo docker | Execute `sudo usermod -aG docker $USER` e depois faêÂ§a logout/login |
| `port already in use` | Outro serviêÂ§o na porta 18789 | Mude a porta no `docker-compose.yml` ou pare o outro serviêÂ§o |
| `no space left on device` | Disco cheio | Execute `docker system prune -a` pra limpar imagens antigas |

### Docker / Container

| Erro | Causa | SoluêÂ§êÂ£o |
|------|-------|---------|
| `exec entrypoint.sh: no such file or directory` | Finais de linha CRLF do Windows | Isso êÂ© corrigido automaticamente pelo `.gitattributes` e o Dockerfile. Se ainda acontecer: `git config core.autocrlf input` e clone novamente. Ou abra `entrypoint.sh` no VS Code, mude CRLF ê¢â â LF (canto inferior direito), salve, rebuild. |
| `error: unknown option '--foreground'` | Dockerfile antigo usando comando errado | Atualize o Dockerfile ê¢â¬â CMD deve ser `["clawdbot", "gateway", "run"]` (nêÂ£o `start --foreground`) |
| `npm error: spawn git ENOENT` | Git nêÂ£o instalado na imagem | Adicione `git` na linha `apt-get install` do Dockerfile |
| Container reiniciando em loop | Verifique `docker logs clawdbot` pro erro especêÂ­fico | Veja os erros acima |

### Geral

| Erro | SoluêÂ§êÂ£o |
|------|---------|
| Bot nêÂ£o responde | Verifique os logs: `docker compose logs -f` |
| Erros de API | Verifique se as API keys no `.env` estêÂ£o corretas |
| NêÂ£o consegue acessar o Webchat | Use têÂºnel SSH: `ssh -L 18789:localhost:18789 user@server` |

## Contribuindo

PRs sêÂ£o bem-vindas! Por favor siga o checklist de seguranêÂ§a em [SECURITY.md](SECURITY.md).

## LicenêÂ§a

MIT

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>