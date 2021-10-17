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

### [uPyMagSig] software para captar assinaturas magnéticas e envio para IoT Cloud

Como uma opção de pré-protótipo, foi elaborado uma versão do uPyMagSig para Pyboard's, com envios de dados para nuvem. A versão mais recente grava os dados de campo e tempo em um arquivo `.csv` para aplicar outras análises gráficas em Python3. Vide software de análise gráfica desenvolvido.

O software apresenta leituras do campo em micro Tesla, em um intervalo de +/- 100 uT. O IoT Cloud apresenta um gráfico com a magnitude da do campo na direção em que o FLC100 estiver apontando, podendo variar em valores negativos e positivos, orientação unidimensional.

##### As versões mais recentes do uPyMagSig (>=V0.6), tem tela LED e novas funções

As leituras de alta frequência (várias leituras do ADC por segundo), são mostradas no [TILE-LED36](https://pybd.io/hw/tile_led36.html#code-samples). Isso porque são leituras rápidas demais para o IoT Cloud receber, logo as leituras enviadas para o IoT Cloud são as médias da lista de medidas rápidas lidas pelo ADC a cada 3 segundos.

O tela de LEDs do TILE-LED36 se preenche de acordo com a intensidade de fluxo magnético na direção em que o FLC100 está apontando. Cada LED corresponde a 10uT (isso pode ser editado no código-fonte). Caso o campo tenha orientação negativa, os LEDs brilham em azul, caso a orientação seja positiva, os LEDs brilham em vermelho. 

#### Hardware

O hardware do uPyMagSig é composto por:

- [Pyboard-D SF2W](https://store.micropython.org/product/PYBD-SF2-W4F2);
- Divisor de tensão (5V para ~ 3V3);
- [Fonte Ajustável Protoboard](https://www.filipeflop.com/produto/fonte-ajustavel-protoboard/);
- [magnetômetro fluxgate FLC100](http://md-ecs.com/wp-content/uploads/2016/01/Data-sheet_FLC-100.pdf).

Opcional, presente nas versões mais atuais (>= V0.6):

- [TILE-LED36](https://pybd.io/hw/tile_led36.html#code-samples);
- [WBUS-DIP68](https://pybd.io/hw/wbus_dip68.html).

<img src="https://gitlab.com/rcolistete/computacaofisica-privado/-/blob/master/LabNerdsIoT_Vitoria/projeto9/Pyboard-D%20SF2W/img/pre-prototipo.jpg" width="300" height="300" />

#### Dashboard
O dashboard escolhido (Interface IoT) foi o do [Cayenne](https://cayenne.mydevices.com/cayenne/dashboard/device/c83cf2c0-a811-11eb-883c-638d8ce4c23d), pois possui uma interface mais rica em recursos. Porém, o tipo de grandeza dos dados que usamos é Tesla, grandeza ainda não suportada pelo Cayenne, vide [documentação](https://developers.mydevices.com/cayenne/docs/cayenne-mqtt-api/#magnetometer-widget). Logo, usamos os espaços editaveis para mostrar a escala e grandezas que trabalhamos.

<img src="https://gitlab.com/rcolistete/computacaofisica-privado/-/blob/master/LabNerdsIoT_Vitoria/projeto9/Pyboard-D%20SF2W/img/dashboard.png" width="300" height="300" />

#### Aplicação
A aplicação do pré-protótipo é simples, pois exige apenas da instalação física do pré-protótipo perto do aparelho eletromagnético desejado. Desse modo, podemos ver como o equipamento se comporta, se emite ou não um campo magnético quando ligado e/ou desligado, e se apresenta variações durante seu funcionamento. Tais dados são enviados para a núvem e são gravados na memória do MCU.

Por exemplo, podemos ver um pequeno desnível no gráfico causado pelo funcionamento do microondas:

<img src="img/dashboardon.png" width="300" height="300" />

Os dados que o IoT Cloud recebe são as médias da lista de amostragem calculadas a cada 3 segundos. Isso porque o envio dos dados possui um delay consideravel. Logo, para exibir variações do campo com frequências mais altas, usamos a pequena tela de LED's para tal.  

Por exemplo, a intensidade do campo é mostrada de acordo com o preenchimento da tela de 36 LED's (LED's vermelhos para intensidades positivas e azul para negativas):

<img src="img/aplication-off.jpg" width="300" height="300" />

Quando o aparelho é ligado:

<img src="img/aplication-on.jpg" width="300" height="300" />


**Equipe**: Eduardo Destefani Stefanato (principal), Pedro Henrique Robadel da Silva Camâra, Roberto Colistete Júnior.
