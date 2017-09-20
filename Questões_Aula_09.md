# 16_0152038_Microprocessadores-Microcontroladores
Disciplina Microprocessadores e Microcontroladores, Curso de Engenharia Eletrônica da UnB, Campus Gama. Período 2/2017.


1. Escreva uma função em C que faz o debounce de botões ligados à porta P1.

int main( void )

{
    WDTCTL = WDTPW | WDTHOLD; //para o Watch Dog
    
    P1DIR = BIT1;                       //configura o pino 1 como saída 0b 0000 0010 
    
    P1OUT = BIT1;                       // leva os pino 1 para VCC 

int i = 100;

if (BIT1 == 0)

 while(i != 0)
 
if ( BIT1 = 0)   /// apertado 0

{i--;}

else

i=100;

    }
}

RESPOSTA

#include <msp430g2553.h>

#include <legacymsp430.h>

#define LED BIT0

int main(void)
{
	WDTCTL = WDTPW + WDTHOLD;	// Stop WDT
	
	BCSCTL1 = CALBC1_1MHZ;		//MCLK e SMCLK @ 1MHz
  
	DCOCTL = CALDCO_1MHZ;		//MCLK e SMCLK @ 1MHz
  
	P1OUT &= ~LED;
  
	P1DIR |= LED;
  
	TA0CCR0 = 62500-1; //10000-1;
  
	TA0CTL = TASSEL_2 + ID_3 + MC_1 + TAIE;    1/8 x 10^6 Hz= 8.10^-6 
  
	_BIS_SR(LPM0_bits+GIE);                    Quantos 8.10-6 s cabem em 10.10
  
	return 0;
}

interrupt(TIMER0_A1_VECTOR) TA0_ISR(void)

{

	P1OUT ^= LED;
  
	TA0CTL &= ~TAIFG;
  
}



2. Escreva um código em C que lê 9 botões multiplexados por 6 pinos, e pisca os LEDs da placa Launchpad de acordo com os botões. Por exemplo, se o primeiro botão é pressionado, os LEDs piscam uma vez; se o segundo botão é pressionado, os LEDs piscam duas vezes; e assim por diante. Se mais de um botão é pressionado, os LEDs não piscam.
