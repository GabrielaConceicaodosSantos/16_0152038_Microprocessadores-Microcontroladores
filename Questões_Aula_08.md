# 16_0152038_Microprocessadores-Microcontroladores
Disciplina Microprocessadores e Microcontroladores, Curso de Engenharia Eletrônica da UnB, Campus Gama. Período 2/2017.

                                                QUESTÕES: AULA - 08

Para todas as questões, utilize os LEDs e/ou os botões da placa Launchpad do MSP430.

1. Escreva um código em C que pisca os LEDs ininterruptamente.

#include "io430.h"

int main( void )
  {
  
    int i;
    
    WDTCTL = WDTPW + WDTHOLD;
    
    P1DIR=BIT0+BIT6;         
    P1OUT=BIT0+BIT6;       
    
    while(1)
    {
    P1OUT^=(BIT0+BIT6);    
    for(i=0;i<9999;i++);    
    }
   
  }

2. Escreva um código em C que pisca os LEDs ininterruptamente. No ciclo que pisca os LEDs, o tempo que os LEDs ficam ligados deve ser duas vezes maior do que o tempo que eles ficam desligados.

int main(void) 

{

    WDTCTL = WDTPW | WDTHOLD;	   
     
  P1DIR=BIT0+BIT6;         
  P0OUT=BIT0+BIT6; 
 
    while(1){
    	P1OUT = BIT6;			   
    	__delay_cycles(2000000);	
    	P1OUT = 0x00;			    
    	__delay_cycles(1000000);	
      P1OUT = BIT0;			    
    	__delay_cycles(2000000);	
    	P1OUT = 0x00;			   
    	__delay_cycles(1000000);	
    }
}

3. Escreva um código em C que acende os LEDs quando o botão é pressionado.

#include "io430.h"
  
  int main( void )
  
  {
  
    int i;
    WDTCTL = WDTPW + WDTHOLD;
    P1DIR=BIT0+BIT6;       
    P1OUT=BIT0+BIT6;   
    
    while(1)
    {
if((P1IN & BIT3) == 0)

    {
    
P1OUT^=(BIT0+BIT6);    

    }  
    
for(i=0;i<27000;i++);    

    }
}

4. Escreva um código em C que pisca os LEDs ininterruptamente somente se o botão for pressionado.

int main(void) 

{

    WDTCTL = WDTPW | WDTHOLD;	   
     
  P1DIR=BIT0+BIT6;         
  P0OUT=BIT0+BIT6; 
 
    while(1){
    if((P1IN & BIT3) == 0)
    {
    	P1OUT = BIT6;			   
    	__delay_cycles(2000000);	
    	P1OUT = 0x00;			    
    	__delay_cycles(1000000);	
      P1OUT = BIT0;			    
    	__delay_cycles(2000000);	
    	P1OUT = 0x00;			   
    	__delay_cycles(1000000);	
      
    }
    
    for(i=0;i<27000;i++);    
    
    } 
    
}

5. Escreva um código em C que acende os LEDs quando o botão é pressionado. Deixe o MSP430 em modo de baixo consumo, e habilite a interrupção do botão.

#include <msp430g2553.h>

#include <legacymsp430.h> // Para rodar interrupcoes



#define LED1 BIT0

#define LED2 BIT6

#define LEDS (LED1|LED2)

#define BTN  BIT3



void atraso(volatile unsigned int x)

{

  TACCR0 = 1000-1; 
  
  TACTL |= TACLR;
  
  TACTL = TASSEL_2 + ID_0 + MC_1;
  
  while(x>0)
  
  {
  
    x--;
    
    while((TACTL & TAIFG) == 0);
    
    TACTL &=~TAIFG;
    
  }
  
  TACTL = MC_0;
  
}


int main(void)

{

	WDTCTL = WDTPW | WDTHOLD;
  
	P1OUT &= ~LEDS;
  
	P1DIR |= LEDS;
  
	P1DIR &= ~BTN;
  
	P1REN |= BTN;
  
	P1OUT |= BTN;
  
	P1IES |= BTN;
  
	P1IE |= BTN;
  
	_BIS_SR(GIE + LPM4_bits);
  
	return 0;
        
        while(1)
	{
		atraso(0xFFFF);
    
		P1OUT ^= LEDS;
	}
  
	return 0;
  
}

interrupt(PORT1_VECTOR) Interrupcao_P1(void)

{

	P1OUT ^= LEDS;
  
	while((P1IN&BTN)==0);
  
	P1OUT ^= LEDS;
  
	P1IFG &= ~BTN;
  
}


Observação:

P1IE -> INTERRUPÇÃO

P1IES -> DEFINE SE A INTERRUPÇÃO SERÁ CHAMADA POR BORDA DE SUBIDA OU DE DESCIDA

SUBIDA -> 0

DESCIDA -> 1



Modos de contagem:

MC_0: 00 -> contagem parada

MC_1 : 01-> até o módulo (valor) -> taccr0

Mc_2: 10 -> de 0x000 a 0xFFFF

MC_3 : 11-> progressiva/regressiva


Divisores de frequ~encia; 

ID_0: 1

ID_1: 2

ID_2: 4

ID_3: 8

tassel 2: SMCLK

