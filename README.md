# 🚰 Caixa d'Água IoT – Monitoramento de Nível com ESP32

Sistema de monitoramento de nível de caixa d'água usando **ESP32 + sensor ultrassônico HC-SR04**, com envio de dados via **MQTT (EMQX)**, armazenamento e processamento no **Supabase** e visualização em painel hospedado no **Cloudflare**.

---

## 💡 Origem da Ideia

A ideia surgiu da necessidade de simular de forma simples e educativa o monitoramento de uma caixa d'água residencial.  

O objetivo é:

- Medir a distância até a superfície da água com o sensor HC-SR04 posicionado no topo da caixa;
- Converter essa distância em **porcentagem de nível** no ESP32;
- Gerar um **alerta** quando o nível ficar abaixo de **20%**;
- Enviar os dados para a nuvem e visualizar tudo em um painel web.

O projeto foi pensado para ser fácil de simular, testar e evoluir — por isso a simulação começa no **Wokwi**.

🔗 **Simulação Wokwi:**  
[https://wokwi.com/projects/480980769363156993](https://wokwi.com/projects/480980769363156993)

---

## 🏗️ Arquitetura

```
┌─────────────────────┐
│  Wokwi (Simulação)  │
│  ESP32 + HC-SR04    │
└─────────┬───────────┘
          │ MQTT
          ▼
┌─────────────────────┐
│      EMQX           │  (Broker MQTT)
└─────────┬───────────┘
          │
          ▼
┌─────────────────────┐
│     Supabase        │  (Edge Function + Banco de dados)
└─────────┬───────────┘
          │
          ▼
┌─────────────────────┐
│  Painel Cloudflare  │  (Dashboard web)
└─────────────────────┘
```

---

## 🔧 Como funciona o tratamento no ESP32

1. **Leitura do sensor**  
   O HC-SR04 (trig + echo) mede a distância até a superfície da água.

2. **Média de 3 leituras**  
   Para reduzir ruído, são feitas 3 medições e calculada a média.

3. **Validação**  
   Só são aceitos valores entre **2 cm e 400 cm** (faixa válida do HC-SR04).

4. **Conversão para porcentagem**  
   A distância é convertida em nível de 0 a 100% (limitado).

5. **Alerta**  
   Se o nível ficar **abaixo de 20%**, o sistema entra em estado de alerta.

---

## 📦 Componentes utilizados

| Componente          | Função                              |
|---------------------|-------------------------------------|
| ESP32               | Microcontrolador + Wi-Fi            |
| HC-SR04             | Sensor ultrassônico de distância    |
| EMQX                | Broker MQTT                         |
| Supabase            | Banco de dados + Edge Functions     |
| Cloudflare          | Hospedagem do painel web            |

---

## 📁 Estrutura do repositório

```
caixa-dagua-iot/
├── firmware/           # Código do ESP32 (Arduino / PlatformIO)
├── supabase/           # Edge Functions e schema do banco
├── dashboard/          # Frontend do painel (Cloudflare)
├── docs/               # Documentação extra
├── .gitignore
├── LICENSE
└── README.md
```

> As pastas serão preenchidas conforme o desenvolvimento.

---

## 🚀 Como começar

### 1. Simulação no Wokwi
Abra o projeto:  
[https://wokwi.com/projects/480980769363156993](https://wokwi.com/projects/480980769363156993)

### 2. Firmware (ESP32)
- Configure as credenciais Wi-Fi e do broker MQTT (EMQX)
- Ajuste a altura da caixa d'água no código
- Compile e faça o upload

### 3. EMQX
- Crie um broker (pode ser o gratuito do EMQX Cloud ou local)
- Anote o host, porta, usuário e senha

### 4. Supabase
- Crie um projeto
- Configure a tabela de leituras e a Edge Function que recebe os dados via MQTT (ou webhook)

### 5. Painel (Cloudflare)
- Deploy do frontend no Cloudflare Pages
- Conecte com a API do Supabase

---

## 📊 Dados enviados

Exemplo de payload MQTT (JSON):

```json
{
  "distance_cm": 42.5,
  "level_percent": 68.3,
  "alert": false,
  "timestamp": "2026-11-22T14:30:00Z"
}
```

Quando o nível estiver baixo:

```json
{
  "distance_cm": 85.0,
  "level_percent": 15.2,
  "alert": true,
  "timestamp": "2026-11-22T14:35:00Z"
}
```

---

## 🛠️ Tecnologias

- **Hardware / Firmware:** ESP32 + HC-SR04 (Arduino Framework)
- **Comunicação:** MQTT (EMQX)
- **Backend:** Supabase (PostgreSQL + Edge Functions)
- **Frontend:** HTML/CSS/JS ou framework moderno (hospedado no Cloudflare Pages)
- **Simulação:** Wokwi

---

## 📄 Licença

Este projeto está sob a licença **MIT**. Veja o arquivo [LICENSE](LICENSE) para mais detalhes.

---

## 🤝 Contribuições

Sinta-se à vontade para abrir issues e pull requests.  
Sugestões de melhorias (filtro de leituras, calibração remota, histórico, notificações, etc.) são bem-vindas!

---

Feito com 💧 para aprender IoT de forma prática.
