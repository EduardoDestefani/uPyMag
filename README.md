## uPyMag-[Pyboard D SF2](https://store.micropython.org/product/PYBD-SF2-W4F2) + ADC de 16-24 bits + [FLC100](https://stefan-mayer.com/en/products/magnetometers-and-sensors/magnetic-field-sensor-flc-100.html) + carregador USB + caixa

**Configuração para monitorar equipamento elétrico/mecânico ligado**, que seja importante, **detectando assinatura magnética via fluxgate FLC-100 (que é analógico e precisa de ADC de precisão para ser lido)**, com envio dos dados via WiFi p/ IoT Cloud e exibição resumida dos dados em tela LED.

Usa **modo de operação contínua dos sensores**, sem deepsleep, com monitoramento dos dados com centenas de leituras por segundo, para serem analisadas via processamento de sinais com [ulab](https://github.com/v923z/micropython-ulab#ulab).

### Para uPyGeoMag:
Escolher placa ADC de 16 a 24 bits:

- [ADS1115](https://www.ti.com/product/ADS1115) (16 bits, I2C);
- [ADS1219](https://www.ti.com/product/ADS1219) (24 bits, I2C);
- [ADS1220](https://www.ti.com/product/ADS1220) (24 bits, SPI);
- etc,

Pretende-se contruir placa de circuito impresso com os componentes soldados, filtros de sinais RC, alimentação da parte analógica separada, com pequeno comprimento dos fios e terminais, tudo para minimizar ruído no sinal analógico lido do FLC100.

### Para uPyMagSig:
Uso de ADC **interno** 12bits do Pyboard-D SF2W: pretende-se contruir placa de circuito impresso com os componentes soldados, filtros de sinais RC, alimentação da parte analógica separada, com pequeno comprimento dos fios e terminais, tudo para minimizar ruído no sinal analógico lido do FLC100.

**Opções**:

- tela OLED ou TILE-LED36;
- datalogging completo em cartão micro SD, se ausente, então datalogging resumido na memória flash interna;
- medição inteligente do(s) magnetômetros com Edge Computing, com análise espectral das medidas, comparando com espectro de fundo (assinatura magnética sem equipamento ligado) para detectar assinatura magnética diferente do equipamento ligado e/ou campo geomagnético;

**Quantidades a serem implementadas**:

- só 1x, pois o FLC100 é caro (EUR60-80) e há poucos disponíveis (precisa de 1-2 p/ desenvolvimento);
- 1x no datacenter ou na sala de entrada ou sala de trabalho, onde há mais equipamentos eletrônicos/mecânicos ligados.

**Equipe**: Eduardo Destefani Stefanato (principal), Pedro Henrique Robadel da Silva Camâra, Roberto Colistete Júnior.
