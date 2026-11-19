<p align="center">
  <img src="assets/migration-banner.jpg" alt="ClawdBot â†’ MoltBot Migration" width="100%">
</p>

> # âš ï¸ Clawdbot estÃ¡ sendo renomeado para **Moltbot**
> 
> Estamos trabalhando em uma versÃ£o atualizada. Quando estiver estÃ¡vel, o novo repositÃ³rio serÃ¡ disponibilizado para migraÃ§Ã£o.
> 
> **Enquanto isso, continue usando este repositÃ³rio (docker-clawdbot) normalmente.**

---

<p align="center">
  <img src="assets/clawdbot-banner.jpg" alt="Clawdbot - AI Assistant" width="100%">
</p>

# ðŸ¦ž Docker Clawdbot

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


Setup Docker para [Clawdbot](https://docs.clawd.bot) â€” assistente pessoal de IA com hardening de seguranÃ§a pronto pra usar.

<p align="center">
  <img src="assets/clawdbot-robot.jpg" alt="Clawdbot Robot" width="300">
</p>

## Funcionalidades

- ðŸ”’ **SeguranÃ§a reforÃ§ada** â€” segue o [Top 10 Security Checklist](SECURITY.md)
- ðŸ³ **Setup com um comando** â€” `docker compose up -d`
- ðŸ” **Secrets via variÃ¡veis de ambiente** â€” sem credenciais em texto puro
- ðŸ‘¤ **Container nÃ£o-root** â€” roda como usuÃ¡rio sem privilÃ©gios
- ðŸ“ **Logging habilitado** â€” trilha de auditoria por padrÃ£o
- ðŸ“± **Pronto pra Telegram** â€” sÃ³ adicionar o token do bot
- ðŸŽ™ï¸ **TranscriÃ§Ã£o de Ã¡udio** â€” Faster Whisper incluso (opcional)
- ðŸªŸ **CompatÃ­vel com Windows** â€” `.gitattributes` forÃ§a finais LF, Dockerfile corrige CRLF

## InÃ­cio RÃ¡pido

### PrÃ©-requisitos

| Plataforma | Requisito | InstalaÃ§Ã£o |
|------------|-----------|------------|
| **Windows** | Docker Desktop | [docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop/) |
| **Windows** | Git | [git-scm.com](https://git-scm.com/download/win) |
| **Mac** | Docker Desktop | [docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop/) |
| **Linux** | Docker Engine + Compose | `curl -fsSL https://get.docker.com \| sh` |

> âš ï¸ **UsuÃ¡rios Windows:** Certifique-se de que o **Docker Desktop estÃ¡ rodando** antes de continuar. Verifique o Ã­cone do Docker na bandeja do sistema. Se o WSL mostrar `docker-desktop Stopped`, abra o Docker Desktop e aguarde ele iniciar.

### 1. Clone o repo

**Linux / Mac:**
```bash
git clone https://github.com/inematds/docker-clawdbot.git
cd docker-clawdbot
```

**Windows (CMD ou PowerShell):**
```cmd
git clone https://github.com/inematds/docker-clawdbot.git
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

> âš ï¸ **Importante:** VocÃª DEVE criar E editar o arquivo `.env` antes de rodar `docker compose up`.

Abra o arquivo `.env` e substitua os valores placeholder pelas suas chaves reais:

```env
# âŒ ERRADO (placeholder â€” nÃ£o vai funcionar):
ANTHROPIC_API_KEY=sk-ant-your-key-here

# âœ… CERTO (sua chave real):
ANTHROPIC_API_KEY=sk-ant-abc123-your-actual-key
```

**MÃ­nimo pra comeÃ§ar:** VocÃª precisa de pelo menos uma chave de provedor LLM (veja a tabela acima) e um token de gateway.

Pra gerar um token de gateway seguro:
```bash
# Linux / Mac:
openssl rand -hex 24

# Windows (PowerShell):
-join ((1..48) | ForEach-Object { '{0:x}' -f (Get-Random -Max 16) })

# Or just use any long random string (at least 24 characters)
```

**ObrigatÃ³rios:**
- `GATEWAY_AUTH_TOKEN` â€” gere com `openssl rand -hex 24`
- **Um provedor LLM** (escolha um ou mais):

| Provedor | VariÃ¡vel de Ambiente | Obter Chave |
|----------|---------------------|-------------|
| Anthropic (Claude) | `ANTHROPIC_API_KEY` | [console.anthropic.com](https://console.anthropic.com/) |
| OpenAI (GPT) | `OPENAI_API_KEY` | [platform.openai.com](https://platform.openai.com/api-keys) |
| OpenRouter (multi-modelo) | `OPENROUTER_API_KEY` | [openrouter.ai](https://openrouter.ai/) |
| Google (Gemini) | `GOOGLE_API_KEY` | [ai.google.dev](https://ai.google.dev/) |

> ðŸ’¡ **Dica:** O OpenRouter dÃ¡ acesso a mÃºltiplos modelos (Claude, GPT, Llama, Gemini) com uma Ãºnica API key â€” incluindo modelos gratuitos.

**Opcionais:**
- `TELEGRAM_BOT_TOKEN` â€” pegue com o [@BotFather](https://t.me/BotFather)
- `BRAVE_API_KEY` â€” para busca na web

### 3. Build e execuÃ§Ã£o
```bash
docker compose up -d
```

> ðŸ’¡ **Primeira execuÃ§Ã£o** leva alguns minutos pra fazer o build da imagem (baixa Node.js, FFmpeg, etc). As execuÃ§Ãµes seguintes sÃ£o instantÃ¢neas.

> âš ï¸ **Erro no Windows "pipe/dockerDesktopLinuxEngine"?** O Docker Desktop nÃ£o estÃ¡ rodando. Abra ele pelo menu Iniciar e aguarde atÃ© mostrar "Docker is running", depois tente novamente.

### 4. Acesse o Webchat

Abra no seu navegador:
```
http://localhost:18789/chat
```

Quando solicitado, insira seu `GATEWAY_AUTH_TOKEN` do arquivo `.env` para autenticar.

> ðŸ’¡ **Dica:** VocÃª tambÃ©m pode acessar diretamente com: `http://localhost:18789/?token=SEU_TOKEN`

### 5. Verifique o status
```bash
docker compose logs -f
```

### 6. ConfiguraÃ§Ã£o pÃ³s-instalaÃ§Ã£o

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
| `clawdbot configure` | Wizard interativo â€” configura API keys, canais (Telegram, WhatsApp, etc.), preferÃªncias de modelo |
| `clawdbot doctor --fix` | Auto-detecta e corrige problemas de config (ex: Telegram configurado mas nÃ£o habilitado) |
| `clawdbot doctor` | Mesma verificaÃ§Ã£o, mas sÃ³ **mostra** os problemas sem corrigir |
| `clawdbot status` | Mostra status do gateway, canais conectados, info do modelo |

## ConfiguraÃ§Ã£o do Telegram

1. Crie um bot com o [@BotFather](https://t.me/BotFather)
2. Adicione o token no `.env`:
   ```
   TELEGRAM_BOT_TOKEN=123456:ABC-your-token
   ```
3. Reinicie: `docker compose restart`
4. Mande uma mensagem pro seu bot â€” ele vai te dar um cÃ³digo de pareamento
5. Aprove dentro do container:
   ```bash
   docker compose exec clawdbot clawdbot pairing approve telegram <code>
   ```

## SeguranÃ§a

Este setup implementa 7 de 10 medidas de hardening de seguranÃ§a automaticamente. Veja [SECURITY.md](SECURITY.md) para o checklist completo e passos manuais.

### PadrÃµes principais:
- Gateway faz bind apenas em `127.0.0.1`
- PolÃ­tica de DM requer aprovaÃ§Ã£o de pareamento
- Arquivos de config tÃªm `chmod 600`
- Container roda como nÃ£o-root
- Logging e diagnÃ³sticos habilitados

## Volumes

| Volume | Finalidade |
|--------|------------|
| `clawdbot-data` | Dados de config e sessÃ£o |
| `clawdbot-workspace` | Workspace do agente (AGENTS.md, memÃ³ria, etc) |
| `clawdbot-logs` | Arquivos de log (`/home/clawdbot/logs`) |

## Comandos Ãšteis

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

Por padrÃ£o, o container tem acesso Ã  internet (necessÃ¡rio pras chamadas de API). Para isolamento total:

```yaml
# In docker-compose.yml, change:
networks:
  clawdbot-net:
    internal: true  # No internet access
```

âš ï¸ Isso bloqueia chamadas de API pra Anthropic/OpenAI. Use apenas se vocÃª tiver um setup de modelo local.

## Canais de Acesso

MÃºltiplas formas de interagir com seu Clawdbot de qualquer lugar:

| Canal | Tipo | Acesso | ConfiguraÃ§Ã£o |
|-------|------|--------|--------------|
| ðŸ“± **Telegram** | Mensageiro | Qualquer lugar (mobile/desktop) | Crie um bot via [@BotFather](https://t.me/BotFather) |
| ðŸ“² **WhatsApp** | Mensageiro | Qualquer lugar (mobile/desktop) | Vincule via QR code (`clawdbot channels login`) |
| ðŸ’¬ **Webchat** | Interface Web | Rede local / VPN | Integrado, roda na porta do gateway |
| ðŸŒ **Webchat (pÃºblico)** | Interface Web | Qualquer lugar | Proxy reverso Nginx + certificado SSL |
| ðŸ”’ **Tailscale** | VPN | Qualquer lugar (zero-trust) | Instale Tailscale no servidor + dispositivos |
| ðŸ’œ **Discord** | Mensageiro | Qualquer lugar | Crie um bot via Discord Developer Portal |
| ðŸ’¼ **Slack** | Mensageiro | Qualquer lugar | Crie um Slack app + bot token |
| ðŸ”µ **Signal** | Mensageiro | Qualquer lugar | Signal CLI ou dispositivo vinculado |
| ðŸŸ¢ **Matrix** | Mensageiro | Qualquer lugar | Homeserver Matrix + conta de bot |

### Qual devo usar?

**Setup mais simples:** Telegram â€” um bot token e pronto.

**Mais privado:** Signal ou Tailscale + Webchat.

**Acesso de qualquer lugar sem apps extras:** Telegram + WhatsApp (vocÃª jÃ¡ tem eles no celular).

**Melhor pra times/trabalho:** Slack ou Discord.

**Acesso remoto mais seguro ao Webchat:** Tailscale â€” VPN zero-trust, sem portas abertas, funciona de qualquer rede.

### Multi-canal
VocÃª pode habilitar **mÃºltiplos canais simultaneamente**. Todos os canais compartilham o mesmo agente, memÃ³ria e workspace. Mensagens de qualquer canal chegam no mesmo assistente.

âš ï¸ **Mensagens entre canais sÃ£o restritas** por design â€” o bot nÃ£o vaza dados entre canais.

## Acesso ao Webchat (Remoto)

O gateway faz bind em `127.0.0.1` (loopback). Para acessar o Webchat de outra mÃ¡quina, use um **tÃºnel SSH**:

```bash
# On your local machine (PC/Mac):
ssh -L 18789:localhost:18789 root@your-server-ip
```

Depois abra no seu navegador:
```
http://127.0.0.1:18789/chat
```

Essa Ã© a forma mais segura de acessar a interface web remotamente â€” sem portas expostas, criptografado via SSH.

## WhatsApp: Dicas para NÃºmero Pessoal

Se vocÃª usar seu **nÃºmero pessoal do WhatsApp** (modo self-chat), fique atento:

- âš ï¸ Por padrÃ£o, qualquer pessoa que te mandar mensagem pode receber uma resposta de cÃ³digo de pareamento do bot
- âœ… **SoluÃ§Ã£o:** Defina `dmPolicy: allowlist` com apenas seu nÃºmero pra evitar isso:
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
- ðŸ”„ **Recomendado:** Migre pra um **nÃºmero dedicado** o mais rÃ¡pido possÃ­vel pra um setup mais limpo

## Ferramentas e Skills Recomendadas

Turbine seu Clawdbot com essas ferramentas adicionais:

### ðŸ›  Ferramentas CLI

| Ferramenta | InstalaÃ§Ã£o | Finalidade |
|------------|------------|------------|
| [Codex CLI](https://github.com/openai/codex) | `npm i -g @openai/codex` | Agente de cÃ³digo IA (OpenAI) |
| [agent-browser](https://github.com/vercel-labs/agent-browser) | `npm i -g agent-browser` | AutomaÃ§Ã£o de navegador headless pra agentes IA |
| FFmpeg | `apt install ffmpeg` | Processamento de Ã¡udio/vÃ­deo |
| Faster Whisper | `pip install faster-whisper` | TranscriÃ§Ã£o de Ã¡udio local |

### ðŸŽ¨ ServiÃ§os de API

| ServiÃ§o | Finalidade | PreÃ§o |
|---------|------------|-------|
| [OpenRouter](https://openrouter.ai) | Gateway pra mÃºltiplos LLMs (modelos gratuitos disponÃ­veis) | Tier gratuito + pay-per-use |
| [Kie.ai](https://kie.ai) | GeraÃ§Ã£o de imagem, vÃ­deo e mÃºsica (Veo 3.1, Flux, Suno) | CrÃ©ditos |
| [ElevenLabs](https://elevenlabs.io) | Text-to-speech (vozes realistas) | Tier gratuito + pago |
| [Gamma](https://gamma.app) | ApresentaÃ§Ãµes e documentos com IA | Tier gratuito + pago |
| [HeyGen](https://heygen.com) | Avatares de vÃ­deo com IA | CrÃ©ditos |

### ðŸ“š Skills (para Codex / Claude Code)

| Skill | InstalaÃ§Ã£o | Finalidade |
|-------|------------|------------|
| [Remotion Skills](https://github.com/inematds/remotion-skills) | Copie para `.codex/skills/` | Crie vÃ­deos programaticamente com React |

```bash
# Install Remotion Skills for Codex
git clone https://github.com/inematds/remotion-skills.git /tmp/remotion-skills
mkdir -p .codex/skills
cp -r /tmp/remotion-skills/skills/remotion .codex/skills/
```

### ðŸ¤– OrganizaÃ§Ã£o de LLMs

EstratÃ©gia recomendada de modelos:

| Modelo | Provedor | Caso de Uso | Custo |
|--------|----------|-------------|-------|
| Claude Opus 4.5 | Anthropic Max | Assistente principal (conversas, tarefas) | Plano mensal |
| gpt-5.2-codex | OpenAI ChatGPT Team | GeraÃ§Ã£o de cÃ³digo (prioridade) | Plano mensal |
| Modelos gratuitos | OpenRouter | Sub-agentes, tarefas secundÃ¡rias | Gratuito |

**Modelos gratuitos no OpenRouter:** DeepSeek R1, Llama 3.1 405B, Llama 3.3 70B, Gemini 2.0 Flash, Qwen3 Coder

## Requisitos

- Docker Engine 24+
- Docker Compose v2+
- Pelo menos 2GB de RAM (4GB recomendado com Whisper)

## SoluÃ§Ã£o de Problemas

### Windows

| Erro | Causa | SoluÃ§Ã£o |
|------|-------|---------|
| `open //./pipe/dockerDesktopLinuxEngine: O sistema nÃ£o pode encontrar o arquivo` | Docker Desktop nÃ£o estÃ¡ rodando | Abra o Docker Desktop e aguarde ele iniciar |
| `.env not found` | Arquivo de config faltando | Execute `copy .env.example .env` e edite com `notepad .env` |
| `the attribute version is obsolete` | Formato antigo do docker-compose | Ignore (inofensivo) ou atualize pra versÃ£o mais recente do docker-clawdbot |
| `WSL docker-desktop Stopped` | WSL nÃ£o iniciou | Abra o Docker Desktop â€” ele inicia o WSL automaticamente |
| Build trava ou falha | RAM insuficiente | Garanta pelo menos 4GB alocados pro Docker (Settings â†’ Resources) |

### Linux / Mac

| Erro | Causa | SoluÃ§Ã£o |
|------|-------|---------|
| `permission denied` | NÃ£o estÃ¡ no grupo docker | Execute `sudo usermod -aG docker $USER` e depois faÃ§a logout/login |
| `port already in use` | Outro serviÃ§o na porta 18789 | Mude a porta no `docker-compose.yml` ou pare o outro serviÃ§o |
| `no space left on device` | Disco cheio | Execute `docker system prune -a` pra limpar imagens antigas |

### Docker / Container

| Erro | Causa | SoluÃ§Ã£o |
|------|-------|---------|
| `exec entrypoint.sh: no such file or directory` | Finais de linha CRLF do Windows | Isso Ã© corrigido automaticamente pelo `.gitattributes` e o Dockerfile. Se ainda acontecer: `git config core.autocrlf input` e clone novamente. Ou abra `entrypoint.sh` no VS Code, mude CRLF â†’ LF (canto inferior direito), salve, rebuild. |
| `error: unknown option '--foreground'` | Dockerfile antigo usando comando errado | Atualize o Dockerfile â€” CMD deve ser `["clawdbot", "gateway", "run"]` (nÃ£o `start --foreground`) |
| `npm error: spawn git ENOENT` | Git nÃ£o instalado na imagem | Adicione `git` na linha `apt-get install` do Dockerfile |
| Container reiniciando em loop | Verifique `docker logs clawdbot` pro erro especÃ­fico | Veja os erros acima |

### Geral

| Erro | SoluÃ§Ã£o |
|------|---------|
| Bot nÃ£o responde | Verifique os logs: `docker compose logs -f` |
| Erros de API | Verifique se as API keys no `.env` estÃ£o corretas |
| NÃ£o consegue acessar o Webchat | Use tÃºnel SSH: `ssh -L 18789:localhost:18789 user@server` |

## Contribuindo

PRs sÃ£o bem-vindas! Por favor siga o checklist de seguranÃ§a em [SECURITY.md](SECURITY.md).

## LicenÃ§a

MIT

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>