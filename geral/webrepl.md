# Webrepl

O Webrepl é um terminal na web para acessar um microcontrolador com Micropython instalado e com esta configuração habilidada.

## Configuração

### Instalação do firmware

- Baixe o firmware em micropython.org, [link para o ESP32/WROOM](micropython.org)
  - Sugestão: [usar a versão 18](https://micropython.org/resources/firmware/esp32-20220117-v1.18.bin)
- Grave o firmware com a IDE de sua preferência
  - Sugestão: [Usar o Thonny](https://thonny.org/)

### Microcontrolador

#### Webrepl no Micropython.

Importe o 'webrepl_setup'. No terminal Micropython da IDE, digite o seguinte comando:

```python
>>> import webrepl_setup
```

Habilite a execução do webrepl durante o boot, digitanto 'E' para a seguinte pergunta:

```
WebREPL daemon auto-start status: disabled

Would you like to (E)nable or (D)isable it running on boot?
(Empty line to quit the setup)

> E
```

Configure uma senha para o acesso do microcontrolador no terminal web (webrepl), que estará diponível através de uma página web, exemplo http://webrepl.orivaldo.net/:

```
To enable WebREPL, you must set password for it
New password: <defina uma senha>
Confirm password: <repita a senha>
```

Durante as aulas práticas de IoT, vamos usar a senha `aulaiot`.

Reinicie o microcontrolador, digitando 'y' para a pergunta abaixo:

```
Changes will be activated after reboot
Would you like to reboot now? (y/n) y
```

#### Acesso a internet

Configure o microcontrolador para acessar a internet via Wi-Fi:

```python
import webrepl
import network

ssid = "deviceXXX"
ssid_password = "senhaesp"

# O objetio 'station' ativa o Wi-Fi e realiza a conexão com uma rede Wi-Fi desejada
# Ao final, apresenta o IP do microcontrolador
station = network.WLAN(network.STA_IF)
station.active(True)
if not station.isconnected():
    print('Connecting to WiFi')
    station.connect(ssid, ssid_password)
    while not station.isconnected():
      pass
    print(f'Connected to {ssid} with success. Config: {station.ifconfig()}')

webrepl.start()
```

Anote o IP obtido, você pode usar a [IDE Thonny](https://youtu.be/nA7pf668__U) para editar este código.

### Acesso ao Webrepl

Você pode baixar a página com o webrepl em sua máquina em https://github.com/micropython/webrepl .

A figura abaixo mostra um _print_ de uma execução da página do webrepl:
![print_webrelp](https://github.com/ect-info/IoT/assets/19957124/c4e79f9d-1287-4588-9b15-0bfe7b5da714)

Para acessar o microcontrolador você vai digitar o IP:8266 na caixa de texto do lado esquerdo superior. Por exemplo, se o IP for "10.0.0.198", você digitará nesta caixa "ws://10.0.0.198:8266". A porta 8266 é uma porta padrão para a conexão com o webrepl.

Observação: Esse roteiro foi construído com testes ralizados com o NodeMCU EPS32.
