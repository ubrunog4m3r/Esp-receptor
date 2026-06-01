# ESP32 Receptor - Envio de Dados ao Supabase via HTTP

Firmware para ESP32 responsavel por receber pacotes de dados via ESP-NOW de um transmissor e encaminha-los a um banco de dados na nuvem (Supabase) via HTTP POST.

---

## Funcionamento

O dispositivo opera no modo `WIFI_AP_STA`, o que permite manter o ESP-NOW e o Wi-Fi ativos ao mesmo tempo. Ao receber um pacote do transmissor, os dados sao armazenados e enviados ao Supabase a cada 2 segundos, desde que haja conexao Wi-Fi ativa. Caso a conexao caia, o firmware tenta reconectar automaticamente.

---

## Estrutura dos Arquivos

| Arquivo | Descricao |
|---|---|
| `espReceptor.ino` | Setup, loop principal e struct de dados recebidos |
| `receber.ino` | Callback de recepcao ESP-NOW e inicializacao do receptor |
| `rede.ino` | Conexao Wi-Fi, timeout e reconexao automatica |
| `http.ino` | Montagem do payload JSON e envio via HTTP POST |
| `secrets.h` | Credenciais de rede e chave da API (nao versionar) |

---

## Configuracao

### 1. Credenciais (`secrets.h`)

Crie o arquivo `secrets.h` na raiz do projeto com o seguinte conteudo:

```cpp
#ifndef SECRETS_H
#define SECRETS_H

#define WIFI_SSID     "seu_ssid"
#define WIFI_PASSWORD "sua_senha"
#define API_URL       "https://<projeto>.supabase.co/rest/v1/leituras"
#define API_KEY       "sua_chave_api"

#endif
```

Adicione ao `.gitignore` para nao versionar as credenciais:

```
secrets.h
```

### 2. Tabela no Supabase

Crie uma tabela chamada `leituras` com as seguintes colunas:

| Coluna | Tipo |
|---|---|
| `nivel_tanque` | float4 |
| `temperatura` | float4 |
| `umidade` | float4 |
| `luminosidade` | int2 |
| `presenca_detectada` | bool |

---

## Dependencias

As bibliotecas `WiFi.h`, `HTTPClient.h` e `esp_now.h` ja fazem parte do pacote ESP32 para Arduino. Nenhuma dependencia adicional e necessaria.

---

## Projeto Relacionado

Os dados recebidos por este dispositivo sao coletados e transmitidos pelo repositorio [ESP32 Transmissor](#).
