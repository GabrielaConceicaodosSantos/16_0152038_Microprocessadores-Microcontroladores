# 16_0152038_Microprocessadores-Microcontroladores
Disciplina Microprocessadores e Microcontroladores, Curso de Engenharia Eletrônica da UnB, Campus Gama. Período 2/2017.

                                                  QUESTÕES: AULA 03
1. Dada uma variável 'a' do tipo 'char' (um byte), escreva os trechos de código em C para:

OBS: O tipo char é utilizado para representar caracteres. Um caractere é representado através de um byte na memória. 
Lembre-se que um byte tem 8 bits, ou seja, é possível representar 256 números (ou no caso, codificar até 
256 caracteres distintos). A linguagem C utiliza esse número como um índice na tabela ASCII. 

	(a) Somente setar o bit menos significativo de 'a'.
  variável tipo char tem 8 bits
  a|= 0X01; //que é o mesmo de a|=1 ou 0b00000001
  
	(b) Somente setar dois bits de 'a': o menos significativo e o segundo menos significativo.
  a|= 0X03;
  
	(c) Somente zerar o terceiro bit menos significativo de 'a'.
  a|= 0XFB
 
  0x seleciona o endereço e FB é 1111 1011
  
	(d) Somente zerar o terceiro e o quarto bits menos significativo de 'a'.
  a|= 0XF3;
  
	(e) Somente inverter o bit mais significativo de 'a'. 
 a^= 0X80
  
	(f) Inverter o nibble mais significativo de 'a', e setar o nibble menos significativo de 'a'. 
 a^= 0XF0
 a|= 0X0F

OBS: nibble 4 bits
setar colocar 1

2. Considerando a placa Launchpad do MSP430, escreva o código em C para piscar os dois LEDs ininterruptamente.

#include <msp430g2553.h>

int main( void )
{
    WDTCTL = WDTPW | WDTHOLD; //para o Watch Dog
    P1DIR = 0x41;             //configura os pinos 0 e 6 como saída 0b 0100 0001  
    P1OUT = 0x41;             // leva os pinos 0 e 6 para VCC (Ligando os LEDs)

  for(;;)                     //loop infinito
  {
P1OUT ^= 0x01;                //alterna a saída usando XOR
for (i=0;i<10000;i++);        //gasto decremental de tempo por software
  }

}


#include <msp430.h>  
 
int main(void) {
    WDTCTL = WDTPW | WDTHOLD;	    // Parar watchdog timer
 
    P1DIR = 0x40;		    // Define porta como entrada ou saída
                                    // 0 para entrada, 1 para saída
 
    P1OUT = 0x00;			        // Inicia em LOW
 
    while(1){                                //loop infinito verdadeiro
    	P1OUT = 0x40;			    // Acende o led
    	__delay_cycles(1000000);	    // Aprox 1 segundo
    	P1OUT = 0x00;			   // Desliga o led
    	__delay_cycles(1000000);	    // Aprox 1 segundo
    }
}

3. Considerando a placa Launchpad do MSP430, escreva o código em C para piscar duas vezes os dois LEDs sempre que o usuário pressionar o botão.

#include <msp430g2553.h>

#define BTN BIT3
#define LED1 BIT0
#define LED2 BIT6

void main(void){

WDTCTL = WDTPW | WDTHOLD;
P1OUT = 0;
P1DIR = LED1 + LED2;

for(;;)
{
if(P1IN & BTN == 0)
P1OUT |= LED1 + LED2;
else
P1OUT &= ~(LED1 + LED2);
}
}

4. Considerando a placa Launchpad do MSP430, faça uma função em C que pisca os dois LEDs uma vez.

#include <msp430g2553.h>

int main( void )
{
    WDTCTL = WDTPW | WDTHOLD; //para o Watch Dog
    P1DIR = 0x41;             //configura os pinos 0 e 6 como saída 0b 0100 0001  
    P1OUT = 0x41;             // leva os pinos 0 e 6 para VCC (Ligando os LEDs)
           
  {
P1OUT ^= 0x01;                //alterna a saída usando XOR
while(0);                     //nunca entra em loop
  }

}

#include <msp430.h> 
 
 
int main(void) {
    WDTCTL = WDTPW | WDTHOLD;	// Parar watchdog timer
	
	P1DIR = BIT6;	// Definição de entradas e saídas
 
	P1REN = BIT3;	// Habilita o resistor de pull-up
	P1OUT = BIT3;	// Define o valor inicial do port
 
	while(1){
		if((P1IN & BIT3) == 0x00){	// Efetuar a leitura do botão
			P1OUT |= BIT6;	// Liga o led
		} else
			P1OUT &= ~BIT6; // Led desligado
	}
}

5. Reescreva o código da questão 2 usando a função da questão 4.

6. Reescreva o código da questão 3 usando a função da questão 4
