, Arduino_PWM_Controller

1. [Introdução ao PWM](#introdução-ao-pwm)
   PWM significa "Pulse Width Modulation", que em português pode ser traduzido como Modulação por Largura de Pulso, ele controla a potência média de um sinal elétrico, variando a largura dos pulsos em um determinado período de tempo.
   No PWM, o sinal é composto por uma sequência de pulsos elétricos, onde a largura de cada pulso varia de acordo com a informação que se deseja transmitir ou o controle que se deseja realizar.
   O sinal PWM é caracterizado por ter uma frequência fixa e uma razão entre o tempo em que o pulso está em nível alto e o tempo total do período.
   
   É amplamente utilizada em eletrônica, especialmente em sistemas de controle e acionamento de dispositivos, como motores elétricos, LEDs, fontes de alimentação, entre outros. 
   Com o PWM, é possível obter um controle preciso da potência fornecida a um dispositivo, variando o ciclo de trabalho conforme necessário.
   
   
 2. [Componentes necessários](#componentes-necessários)
    -Arduino Nano;
    -Botão;
    -Bateria 5v;
    -Resistor 10k;
    -Ponte H L293D;
    -Motor DC 5v;
    
3. [Esquemático](#esquemático)
![image](https://github.com/manugraff/aulaemb/assets/107217391/73438a09-46a6-465e-b5db-e86c42fd876f)

4. [Código-fonte](#código-fonte)
````#include <Arduino.h>


#define BUTTON_PIN 2
#define PWM 9


int estado_botao = 0;
int pwm = 0;
int ultimo_estado_botao = 0;
unsigned long tempo_acionado = 0;
unsigned long tempo_delay = 50;
//declaram as variáveis utilizadas no código. estado_botao, 
//pwm e ultimo_estado_botao são variáveis inteiras, enquanto tempo_acionado e tempo_delay 
//são variáveis do tipo unsigned long, que são usadas para armazenar valores de tempo.

void setup() {
  pinMode(PWM, OUTPUT);
  pinMode(BUTTON_PIN, INPUT_PULLUP);
}

void loop() {

  int leitura = digitalRead(BUTTON_PIN);
  // lendo o estado do pino BUTTON_PIN e armazenando o valor na variável leitura.//

  if (leitura != ultimo_estado_botao) {
    ultimo_estado_botao = leitura;
    if (leitura == HIGH) {  
      tempo_acionado = millis();
    }
  }
  //Se for diferente, o último valor é atualizado e, se o valor lido for HIGH (ou seja, o botão foi pressionado), o tempo atual é armazenado na variável tempo_acionado.

  if (leitura == HIGH && ((millis() - tempo_acionado) > tempo_delay)) {
    pwm += 64;
    if(pwm > 255){
      pwm=0;
    }
  }
  //verifica se o botão está pressionado (leitura == HIGH) e se o tempo decorrido desde o último acionamento (millis() - tempo_acionado) 
  //é maior que o valor definido em tempo_delay. Se ambas as condições forem verdadeiras, o valor da variável pwm é incrementado em 64. 
  //Se pwm exceder 255, ele é reiniciado para 0.

  analogWrite(PWM, pwm);
  delay(50);
}
````

5. [Funcionamento do projeto](#funcionamento-do-projeto)
   
