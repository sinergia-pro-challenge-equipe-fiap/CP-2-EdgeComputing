# 🍷 Vinheria Agnello – Sistema de Monitoramento Ambiental com Arduino

Projeto desenvolvido para monitorar **temperatura**, **umidade** e **luminosidade** em tempo real, com **alertas inteligentes**, **armazenamento de dados históricos** e **interface gráfica via LCD**, atendendo aos critérios de conservação ideais para vinhos.

---

## 🎯 Objetivo

Garantir a qualidade da armazenagem dos vinhos através do monitoramento automatizado de três variáveis ambientais críticas:

- 🌡️ **Temperatura**: deve estar entre 20°C e 30°C
- 💧 **Umidade**: entre 30% e 60%
- 💡 **Luminosidade**: até 60% (baixa iluminação recomendada)

---

## 🛠 Funcionalidades do Sistema

### ✅ Leitura e Processamento de Sensores
- **DHT22**: Leitura de temperatura (°C) e umidade (%)
- **LDR**: Leitura da luminosidade (%), calibrada com `map()`
- Leituras coletadas **a cada segundo** e armazenadas em **buffers circulares**

### ✅ Cálculo de Médias e Atualização de Tela
- A cada **10 segundos**:
  - Calcula a **média** das últimas 10 leituras
  - Atualiza o **LCD 16x2** com:
    - Temperatura média
    - Umidade média
    - Luminosidade média
    - Hora atual (RTC)

### ✅ Alertas Visuais e Sonoros
- Verificação dos limites:
  - Temperatura fora do ideal → 🔴 LED vermelho + 🔊 buzzer
  - Umidade fora do ideal → 🟡 LED amarelo + 🔊 buzzer
  - Luminosidade acima do ideal → 🟢 LED verde + 🔊 buzzer
- Alertas duram 5 segundos e são registrados via Serial

### ✅ Armazenamento de Dados
- A cada **5 minutos** ou **ao disparar alerta**, os dados médios são salvos na **EEPROM** junto com o **timestamp** (hora real)
- Cada registro ocupa **9 bytes**:
  - 4 bytes: Unix timestamp
  - 2 bytes: temperatura ×100
  - 2 bytes: umidade ×100
  - 1 byte: luminosidade (%)
- Armazenamento é **circular**, evitando sobreposição descontrolada

### ✅ Exportação Serial em CSV
- Comando `"export"` via monitor serial
- Exporta todos os registros salvos em formato:
  ```
  Timestamp,Data,Hora,Temperatura,Umidade,Luminosidade
  ```

### ✅ Animação Personalizada no LCD
- Sequência de **37 quadros** exibe o **logotipo da vinheria animado** ao iniciar o sistema
- Utiliza a função `lcd.createChar()` para desenhar cada parte do logo

---

## 🔌 Componentes Utilizados

| Componente        | Função                                           |
|-------------------|--------------------------------------------------|
| Arduino UNO       | Microcontrolador principal                      |
| DHT22             | Sensor de temperatura e umidade                 |
| LDR               | Sensor de luminosidade                          |
| LCD 16x2 I2C      | Exibição dos dados ambientais e do horário      |
| RTC DS1307        | Relógio em tempo real (com backup de bateria)   |
| EEPROM interna    | Armazenamento persistente dos dados             |
| LEDs (R, Y, G)     | Indicadores visuais de alerta                   |
| Buzzer            | Alerta sonoro                                   |
| Protoboard + jumpers | Montagem dos circuitos                      |

---

## 🖥️ Interface

### LCD 16x2
Exibe a cada 10 segundos:

```
T:25.3C H:58%     
L:42%   14:35     
```

### Serial
Mensagens informativas:
- Alertas disparados
- Dados médios a cada minuto
- Exportação dos logs com o comando `"export"`

---

## 🧠 Lógica de Funcionamento

```mermaid
graph TD;
    Start([Início do sistema])
    Start --> LogoAnimado[Mostrar animação no LCD]
    LogoAnimado --> Loop{A cada 1 segundo}
    Loop -->|Ler sensores| Buffers
    Buffers -->|Após 10 leituras| Media
    Media -->|Exibir LCD| Display
    Media --> VerificarAlertas
    VerificarAlertas -->|Se fora do ideal| Alertas
    VerificarAlertas -->|Se ok| Nada
    Alertas --> GravarEEPROM
    GravarEEPROM --> Serial
    Serial --> Loop
```

---

## 🧪 Testes Realizados

- ✅ Sensores testados individualmente
- ✅ Teste de alertas com valores simulados
- ✅ EEPROM preenchida até o limite circular
- ✅ Comando `"export"` retornando todos os registros corretamente
- ✅ RTC sincronizado com horário da compilação

---

## 📁 Estrutura do Projeto

```
VinheriaMonitoramento/
├── vinheria.ino              # Código-fonte completo
├── circuito.png              # Esquema do circuito eletrônico
├── README.md                 # Documentação do projeto
```

---

## 🔗 Simulação

O projeto pode ser simulado online em:  
➡️ [https://wokwi.com](https://wokwi.com)  
> *Sensor DHT22 foi utilizado no lugar do DHT11 por maior precisão e compatibilidade com a plataforma.*

---

## 🚀 Detalhes que Enriquecem o Projeto

- 🎞️ **Animação gráfica com o logo da vinheria no LCD**
- 🧠 **Cálculo de médias por buffer para leituras mais confiáveis**
- 💾 **Registro histórico com hora exata via RTC**
- 📊 **Exportação de dados em CSV para análise externa**
- 🔁 **EEPROM com sistema de armazenamento circular**
- 👨‍💻 **Código modular e bem documentado**

---

## 🏁 Conclusão

O sistema entrega uma solução completa de monitoramento ambiental para vinhos, com alertas automatizados, interface visual clara e registro inteligente de dados — oferecendo controle total sobre o ambiente de armazenagem e potencial para evoluções futuras como integração web ou automação de refrigeração.

---
