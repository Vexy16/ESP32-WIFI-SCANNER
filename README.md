# ESP32 Wi-Fi Scanner (OLED)

Scanner de redes Wi-Fi com ESP32 e display OLED SSD1306. Ele lista as redes próximas, ordenadas pela força do sinal, e mostra nome (SSID), sinal em dBm e porcentagem, canal e tipo de segurança. O botão BOOT da própria placa controla a navegação.

Projeto de aprendizado em eletrônica e firmware, feito como primeiro contato com Wi-Fi no ESP32.

<!-- Adicione aqui uma foto do projeto funcionando: ![Foto](docs/foto.jpg) -->

## Funcionalidades

- Escaneia as redes Wi-Fi ao redor (modo estação, sem conectar em nenhuma)
- Mostra SSID, sinal (dBm e %), barra de sinal, canal e segurança (ABERTA, WEP, WPA, WPA2, WPA3...)
- Ordena as redes da mais forte para a mais fraca
- Também imprime a lista completa no Serial Monitor (115200 baud)
- Navegação com o botão BOOT, sem precisar de nenhum componente extra

## Controles

| Ação no botão BOOT (GPIO 0) | Resultado |
|---|---|
| Clique curto | Próxima rede da lista |
| Clique longo (mais de 0,8 s) | Novo scan |

## Hardware

- ESP32 DevKit
- Display OLED SSD1306 com 4 pinos (I2C), 128x64
- 4 jumpers fêmea-fêmea
- Cabo USB

## Ligação

| OLED | ESP32 |
|---|---|
| GND | GND |
| VCC | 3V3 |
| SCL | GPIO 22 |
| SDA | GPIO 21 |

Os pinos I2C podem ser trocados na linha `Wire.begin(21, 22);` do código. O botão não precisa de ligação: usa o BOOT embutido na placa.

## Software

1. Instale o [Arduino IDE](https://www.arduino.cc/en/software).
2. Adicione o suporte ao ESP32: em *Gerenciador de Placas*, instale **esp32 by Espressif Systems** (versão 2.x ou superior).
3. Em *Gerenciador de Bibliotecas*, instale **Adafruit SSD1306** e **Adafruit GFX Library**.
4. Abra `wifi_scanner/wifi_scanner.ino`.
5. Selecione a placa (ex.: *ESP32 Dev Module*) e a porta COM correta.
6. Clique em *Upload*. Se der erro de conexão, segure o botão BOOT enquanto aparece "Connecting...".

## Ajustes no código

No início do arquivo `wifi_scanner.ino`:

- `SCREEN_HEIGHT`: use `32` se o seu OLED for 128x32.
- `OLED_ADDRESS`: use `0x3D` se o display não acender com `0x3C`.
- `LONG_PRESS_MS`: tempo do clique longo.
- `MAX_NETWORKS`: limite de redes exibidas.

## Problemas comuns

- **Tela não acende:** confira VCC/GND, depois SDA/SCL (são fáceis de inverter) e tente o outro endereço I2C.
- **Mensagem "Falha ao iniciar o OLED" no Serial:** mesmo checklist acima.
- **Nenhuma rede encontrada:** o ESP32 só enxerga redes de 2,4 GHz. Redes exclusivas de 5 GHz não aparecem.
- **Acentos estranhos no nome da rede:** a fonte padrão do display não tem caracteres acentuados.

## Aviso

O projeto faz apenas a leitura passiva do que os roteadores já anunciam publicamente (SSID, canal, sinal, tipo de segurança). Não captura tráfego nem tenta conectar em redes. Use apenas para fins educacionais.

## Ideias para evoluir

- Gráfico de ocupação por canal
- Mostrar o endereço MAC (BSSID) de cada rede
- Salvar o histórico de sinal de uma rede escolhida
- Modo de rolagem automática

## Licença

MIT. Veja o arquivo [LICENSE](LICENSE).
