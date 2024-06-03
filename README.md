# Telegram_esp
# Controle de LED via Telegram com ESP8266

Este projeto demonstra como controlar um LED usando um ESP8266 e um bot do Telegram. O LED pode ser ligado ou desligado enviando comandos "on" e "off" para o bot.

## Materiais Necessários

- ESP8266 (NodeMCU ou similar)
- LED
- Resistor (220Ω a 1kΩ)
- Breadboard e jumpers
- Conexão à internet

## Configuração do Hardware

1. Conecte o anodo (perna mais longa) do LED ao pino GPIO5 do ESP8266.
2. Conecte o catodo (perna mais curta) do LED a um resistor.
3. Conecte a outra extremidade do resistor ao GND.

## Criação e Configuração do Bot no Telegram

1. **Crie um Bot no Telegram:**
   - Abra o Telegram e procure por "BotFather".
   - Envie o comando `/start` para iniciar a conversa com o BotFather.
   - Envie o comando `/newbot` para criar um novo bot.
   - Siga as instruções e forneça um nome e um username para o seu bot. O username deve terminar com "bot" (por exemplo, "MyLampBot").
   - Após a criação, o BotFather fornecerá um token de API. Este token será usado no seu código para autenticar e controlar o bot.

2. **Obtenha o ID do Chat:**
   - Para obter o ID do chat, você pode usar um bot como o [IDBot](https://telegram.me/myidbot). Adicione e inicie uma conversa com o IDBot.
   - Envie o comando `/getid` para obter o seu ID do chat. Este ID é necessário para enviar mensagens específicas ao chat.

## Configuração do Código

1. Certifique-se de ter a biblioteca `FastBot` instalada no Arduino IDE. Caso não tenha, você pode instalar a biblioteca manualmente.
2. Substitua `"NomeDoWifi"`, `"IntroduzaSenhaDoWifi"` e `"IntroduzaSeuTokenAqui"` com as suas informações de Wi-Fi e token do bot do Telegram.

```cpp
#include <ESP8266WiFi.h>
#include "FastBot.h"

#define WIFI_SSID "NomeDoWifi"
#define WIFI_PASS "IntroduzaSenhaDoWifi"
#define BOT_TOKEN "IntroduzaSeuTokenAqui"
//#define CHAT_ID "SeuID aqui" // Não usado neste código, use IDbot para descobrir o seu 

#define LED_PIN 5 // GPIO5, ajuste conforme necessário

FastBot bot;

void setup() {
  Serial.begin(115200);
  connectWiFi();
  pinMode(LED_PIN, OUTPUT);
  digitalWrite(LED_PIN, LOW); // Inicia com o LED desligado
  bot.setToken(BOT_TOKEN);
  bot.attach(newMsg);
}

void newMsg(FB_msg& msg) {
  if (msg.text.equals("on")) {
    digitalWrite(LED_PIN, HIGH);
    bot.sendMessage("LED ligado", msg.chatID);
  } else if (msg.text.equals("off")) {
    digitalWrite(LED_PIN, LOW);
    bot.sendMessage("LED desligado", msg.chatID);
  }
}

void loop() {
  bot.tick();
}

void connectWiFi() {
  WiFi.begin(WIFI_SSID, WIFI_PASS);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
    if (millis() > 15000) {
      ESP.restart();
    }
  }
  Serial.println("\nConectado ao WiFi!");
}
Carregar o Código
Abra o Arduino IDE e carregue o código no seu ESP8266.
Certifique-se de selecionar a placa correta e a porta correta no Arduino IDE.
Abra o monitor serial para verificar a conexão Wi-Fi e receber mensagens de depuração.
Testando o Bot
Envie mensagens "on" e "off" para o seu bot no Telegram.
O bot deve responder com "LED ligado" ou "LED desligado" e controlar o LED conectado ao seu ESP8266.
Contribuições
Contribuições são bem-vindas! Se você encontrar bugs ou tiver melhorias, sinta-se à vontade para abrir uma issue ou enviar um pull request.

Licença
Este projeto está licenciado sob a MIT License.
