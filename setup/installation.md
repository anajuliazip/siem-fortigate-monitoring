# ⚙️ Installation Guide — FortiGate Monitoring (Graylog SIEM)

Este guia descreve como configurar o ambiente necessário para coletar, processar e visualizar logs de um firewall FortiGate utilizando Graylog.

---

## 🎯 Objetivo

Implementar um ambiente SIEM capaz de:

* Receber logs do FortiGate via Syslog
* Processar e indexar eventos
* Detectar atividades suspeitas
* Exibir dados em dashboards

---

## 🧠 Arquitetura

```plaintext
FortiGate → Syslog → Graylog → OpenSearch → Dashboard
```

---

## 📋 Pré-requisitos

* Servidor Linux (recomendado: Rocky Linux / Ubuntu)
* Acesso root ou sudo
* Mínimo recomendado:

  * 4 GB RAM (ideal: 8 GB)
  * 2 CPU
  * 20 GB de armazenamento

---

## 📦 Componentes

* Graylog (SIEM)
* OpenSearch (armazenamento)
* MongoDB (metadados)

---

## 🚀 1. Instalação do MongoDB

```bash
sudo apt update
sudo apt install -y mongodb
sudo systemctl enable mongodb
sudo systemctl start mongodb
```

---

## 🔎 2. Instalação do OpenSearch

```bash
wget https://artifacts.opensearch.org/releases/bundle/opensearch/2.x/opensearch-2.x-linux-x64.tar.gz
tar -xvzf opensearch-2.x-linux-x64.tar.gz
cd opensearch-2.x
./opensearch
```

> ⚠️ Em ambiente produtivo, configure memória JVM e segurança.

---

## 🧩 3. Instalação do Graylog

```bash
wget https://packages.graylog2.org/releases/graylog/graylog-5.x-repository_latest.deb
sudo dpkg -i graylog-5.x-repository_latest.deb
sudo apt update
sudo apt install graylog-server
```

### Configuração básica

Edite o arquivo:

```bash
sudo nano /etc/graylog/server/server.conf
```

Defina:

```ini
password_secret = <string aleatória>
root_password_sha2 = <hash SHA256 da senha>
http_bind_address = 0.0.0.0:9000
```

---

## 🔐 Gerar senha admin

```bash
echo -n 'sua_senha' | sha256sum
```

---

## ▶️ Iniciar Graylog

```bash
sudo systemctl daemon-reexec
sudo systemctl enable graylog-server
sudo systemctl start graylog-server
```

Acesse via navegador:

```
http://<IP_DO_SERVIDOR>:9000
```

---

## 📡 4. Configurar Input Syslog no Graylog

No painel do Graylog:

1. Vá em **System → Inputs**
2. Selecione:

   * Syslog UDP (ou TCP)
3. Configure:

   * Port: `514`
   * Bind address: `0.0.0.0`
4. Clique em **Start Input**

---

## 🔥 5. Configuração no FortiGate

No firewall FortiGate:

### Via CLI:

```bash
config log syslogd setting
set status enable
set server "<IP_DO_GRAYLOG>"
set port 514
set mode udp
end
```

---

## 🧪 6. Validação

Após configurar:

* Acesse o Graylog
* Vá em **Search**
* Verifique se os logs estão chegando

Exemplo esperado:

```plaintext
cfgpath="system.admin"
action="Edit"
```

---

## 🔍 7. Teste de Query

Teste no Graylog:

```plaintext
action:Edit AND cfgpath:system.admin
```

---

## 📊 8. Importar Dashboard (Opcional)

* Vá em **Dashboards**
* Importe o JSON localizado em:

```
/dashboards/fortigate-dashboard.json
```

---

## 🚨 9. Testar Cenário Real

Execute uma ação no FortiGate, por exemplo:

* Alterar perfil de admin
* Tentar login inválido

Valide se aparece no Graylog.

---

## 🛠️ Troubleshooting

### Logs não chegam

* Verificar firewall (porta 514 aberta)
* Validar IP configurado no FortiGate

### Graylog não inicia

* Verificar conexão com MongoDB e OpenSearch
* Conferir logs:

```bash
sudo journalctl -u graylog-server
```

---

## 🎯 Resultado Esperado

Após a configuração:

✔ Logs sendo recebidos em tempo real
✔ Queries funcionando
✔ Dashboard populado
✔ Eventos de segurança visíveis

---

## 🚀 Próximos Passos

* Criar alertas automáticos
* Implementar pipelines de parsing
* Integrar com notificações (Email / Slack)
