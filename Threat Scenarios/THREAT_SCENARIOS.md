# 🔐 Threat Scenarios — FortiGate Monitoring (Graylog SIEM)

Este documento descreve os principais cenários de ameaça detectados através da análise de logs do FortiGate utilizando Graylog.

---

## 🚨 1. Privilege Escalation (Elevação de Privilégio)

**Descrição:**
Detecta quando um usuário tem seu nível de acesso alterado para `super_admin`.

**Impacto:**
Crítico — controle total do firewall.

**Exemplo de log:**

```plaintext
cfgattr="accprofile[admin_no_access->super_admin]"
```

**Query:**

```plaintext
action:Edit AND cfgpath:system.admin AND cfgattr:*->super_admin*
```

**Resposta recomendada:**

* Validar se a alteração foi autorizada
* Auditar usuário responsável
* Revogar acesso indevido imediatamente

---

## 🔐 2. Failed Login Attempts (Tentativas de Login Falhas)

**Descrição:**
Detecta múltiplas falhas de autenticação.

**Impacto:**
Alto — possível ataque de força bruta.

**Query:**

```plaintext
action:login AND status:failed
```

**Resposta recomendada:**

* Identificar origem do IP
* Bloquear IP suspeito
* Verificar tentativas repetidas

---

## 🗑️ 3. Admin Account Deletion (Exclusão de Administrador)

**Descrição:**
Detecta quando uma conta administrativa é removida.

**Impacto:**
Crítico — possível tentativa de ocultar atividade maliciosa.

**Query:**

```plaintext
action:Delete AND cfgpath:system.admin
```

**Resposta recomendada:**

* Confirmar com equipe responsável
* Restaurar conta se necessário
* Auditar ações anteriores do usuário

---

## ⚙️ 4. Configuration Changes (Alterações de Configuração)

**Descrição:**
Detecta modificações em configurações críticas do firewall.

**Impacto:**
Alto — pode indicar mudança indevida de regras.

**Query:**

```plaintext
type:event AND subtype:system AND action:Edit
```

**Resposta recomendada:**

* Revisar mudanças aplicadas
* Validar conformidade com política de segurança
* Reverter alterações suspeitas

---

## 🌐 5. Suspicious SSL VPN Activity

**Descrição:**
Detecta eventos suspeitos relacionados ao uso de VPN SSL.

**Impacto:**
Alto — possível acesso indevido remoto.

**Query:**

```plaintext
subtype:ssl AND action:login
```

**Resposta recomendada:**

* Validar localização do acesso
* Verificar comportamento do usuário
* Aplicar MFA se necessário

---

## 🧠 6. Admin Access via Console (Acesso Administrativo)

**Descrição:**
Monitora acessos administrativos via console ou interface web.

**Impacto:**
Médio — importante para auditoria.

**Query:**

```plaintext
cfgpath:system.admin AND action:login
```

**Resposta recomendada:**

* Registrar atividade
* Correlacionar com mudanças realizadas

---

## 📊 Classificação de Risco

| Severidade | Tipo de Evento                       |
| ---------- | ------------------------------------ |
| 🔴 Crítico | Privilege Escalation, Admin Deletion |
| 🟠 Alto    | Login Failures, Config Changes, VPN  |
| 🟡 Médio   | Admin Access                         |

---

## 🎯 Objetivo

Este conjunto de cenários fornece uma base sólida para:

* Monitoramento contínuo de segurança
* Detecção de incidentes em tempo real
* Operação de um SOC (Security Operations Center)

---

## 🚀 Próximos Passos

* Implementar alertas automáticos
* Integrar com sistemas de notificação (Email, Slack)
* Criar correlação entre eventos
