# Shell-Script-rein-cio-do-servi-o-

# Zabbix + Telegram Auto-Recovery

Este projeto é uma automação para reiniciar serviços críticos automaticamente quando o Zabbix detecta falhas, enviando alertas por Telegram para a equipe de suporte técnico.

## 🧩 Tecnologias utilizadas

- Zabbix (monitoramento e triggers)
- Shell Script (.sh)
- Telegram Bot API

## 🚨 Cenário

O servidor de aplicação estava parando fora do horário comercial, impactando o atendimento ao cliente. Para reduzir o downtime, foi criada uma automação simples:

1. O Zabbix detecta a falha do serviço.
2. Um Shell Script é executado para reiniciar o serviço automaticamente.
3. A equipe recebe uma notificação via Telegram com o status da ação.

## 💻 Exemplo de Script

```bash
#!/bin/bash
SERVICE="nome-do-servico"

if systemctl is-active --quiet $SERVICE; then
    echo "$SERVICE está rodando normalmente"
else
    systemctl restart $SERVICE
    echo "$SERVICE foi reiniciado em $(date)" | curl -s -X POST https://api.telegram.org/bot<SEU_TOKEN>/sendMessage -d chat_id=<SEU_CHAT_ID> -d text="Serviço $SERVICE foi reiniciado automaticamente em $(hostname)"
fi
📬 Configurando o Bot do Telegram

    Crie um bot via @BotFather no Telegram.

    Obtenha o TOKEN do bot e o chat_id do grupo ou usuário.

    Substitua no script e teste com bash script.sh.

📈 Resultados

    Redução de downtime em horários críticos

    Menos acionamentos de plantão

    Ação proativa e rastreável

Contribuições são bem-vindas!

---

📄 auto_restart_service.sh

#!/bin/bash

# Nome do serviço que será monitorado
SERVICE="nome-do-servico"  # Ex: apache2, nginx, docker

# Dados do Telegram
BOT_TOKEN="SEU_BOT_TOKEN_AQUI"
CHAT_ID="SEU_CHAT_ID_AQUI"

# Verifica se o serviço está ativo
if systemctl is-active --quiet "$SERVICE"; then
    echo "$(date): Serviço '$SERVICE' está rodando normalmente."
else
    echo "$(date): Serviço '$SERVICE' está parado. Tentando reiniciar..."

    # Tenta reiniciar o serviço
    systemctl restart "$SERVICE"

    # Verifica novamente após a tentativa de reinício
    if systemctl is-active --quiet "$SERVICE"; then
        MSG="✅ Serviço '$SERVICE' foi reiniciado com sucesso em $(hostname) às $(date)."
    else
        MSG="❌ Falha ao reiniciar o serviço '$SERVICE' em $(hostname) às $(date)."
    fi

    # Envia notificação via Telegram
    curl -s -X POST "https://api.telegram.org/bot$BOT_TOKEN/sendMessage" \
        -d chat_id="$CHAT_ID" \
        -d text="$MSG"
fi
✅ Instruções rápidas:

    Substitua:

        "nome-do-servico" pelo nome do seu serviço (ex: apache2, myapp.service, etc.)

        BOT_TOKEN pelo token do seu bot do Telegram

        CHAT_ID pelo chat ID de quem vai receber a mensagem (pode ser grupo ou usuário)

    Torne o script executável:
chmod +x auto_restart_service.sh

Teste manualmente:
./auto_restart_service.sh

Integre com o Zabbix usando uma Ação de Trigger que execute esse script via Remote Command ou Custom Script.
