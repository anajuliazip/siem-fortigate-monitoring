# 🔐 FortiGate Threat Monitoring (Graylog SIEM)

🚨 Detecta **elevação de privilégio, acessos suspeitos e alterações críticas** em firewalls FortiGate em tempo real.

---

## ⚡ O que este projeto faz

✔ Identifica quando um usuário vira **super_admin**
✔ Detecta **falhas de login e possíveis ataques de força bruta**
✔ Monitora **alterações críticas no firewall**
✔ Exibe tudo em um **dashboard pronto para SOC**

---

## 🧠 Por que isso importa?

Ambientes corporativos frequentemente não têm visibilidade sobre:

* Quem virou administrador 🔴
* Quem está alterando regras ⚠️
* Quando houve tentativa de invasão 🚨

👉 Este projeto transforma logs em **alertas de segurança acionáveis**.

---

## 🔍 Exemplo real detectado

```plaintext
accprofile[admin_no_access->super_admin]
```

➡️ Usuário sem acesso virou administrador total (CRÍTICO)

---

## 📊 Dashboard (visão SOC)

* Falhas de login em tempo real
* Elevação de privilégio
* Acessos SSH
* Alterações de configuração
* Eventos SSL suspeitos

---

## 🛠️ Stack

* Graylog (SIEM)
* OpenSearch
* FortiGate Logs
* Linux (Rocky Linux)

---

## ⚙️ Query-chave (detecção de privilégio)

```plaintext
action:Edit AND cfgpath:system.admin AND cfgattr:*->super_admin*
```

---

## 📦 Resultado

✔ Visibilidade total sobre admins
✔ Detecção rápida de incidentes
✔ Base pronta para SOC / SIEM

---

## 🎯 Diferencial

Este projeto não só coleta logs — ele implementa **lógica de detecção de ameaças reais**.

---

## 👩‍💻 Projeto desenvolvido com foco em:

* Segurança da Informação
* Monitoramento de redes
* Análise de logs (SIEM)

---
