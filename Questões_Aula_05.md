# 16_0152038_Microprocessadores-Microcontroladores
Disciplina Microprocessadores e Microcontroladores, Curso de Engenharia Eletrônica da UnB, Campus Gama. Período 2/2017.


                                                    Questões: Aula 05

Para as questões 2 a 5, considere que as variáveis 'f', 'g', 'h', 'i' e 'j' são do tipo inteiro (16 bits na arquitetura do MSP430), e que o vetor 'A[]' é do tipo inteiro. Estas variáveis estão armazenadas nos seguintes registradores:


	f: R4
	g: R5
	h: R6
	i: R7
	j: R8
	A: R9
  
  
Utilize os registradores R11, R12, R13, R14 e R15 para armazenar valores temporários.

1. Escreva os trechos de código assembly do MSP430 para:

	(a) Somente setar o bit menos significativo de R5.
  
  
mov.w #0000h, R4;

bis R11, R5;

	(b) Somente setar dois bits de R6: o menos significativo e o segundo menos significativo.
  
  
mov.w #0001h, R4;

bis R4, R6;          ;bis-> seta bit a bit 

                     ;bic -> zera bit a bit, uma opção melhor que AND porque se eu quero zerar as opções0 e 7 com AND: 10111110
                     
                     ; com bic 010000010
                            
                            
	(c) Somente zerar o terceiro bit menos significativo de R7.
  
  
mov.w #0008h, R4;

and.w R4, R7;

	(d) Somente zerar o terceiro e o quarto bits menos significativo de R8.
  
  
Se quero zerar o quarto bit, faço uma and com 1011, pra manter os outros intactos.


mov.w #000D, R4;

and.w R4, R8;

	(e) Somente inverter o bit mais significativo de R9.
  
  
XOR inverte quando é 1, então 1000

mov.w #8000, R4

XOR R4,R9;               ;XOR precisa de um número pra fazer a inversão e inv faz sem precisar do número


	(f) Inverter o nibble mais significativo de R10, e setar o nibble menos significativo de R10. 
  
  
Setar colocar 1

mov.w #F000, R11    //que vai dar #000F

XOR R11, R10

mov.w #000F

bis.w R11, R10 



2. "Traduza" o seguinte trecho de código em C para o assembly do MSP430:

	if(i>j) f = g+h+10;
  
	else f = g-h-10;



3. "Traduza" o seguinte trecho de código em C para o assembly do MSP430:

	while(save[i]!=k) 
  
	i++;
  
	
RESPOSTA:




LOOP: 

mov.w R7, R12              ; R7 = i, R12 = temporário                ; move R7 para o R12 para não mexer no R7 pois multiplica por 2 mas lá embaixo o R7 só incrementa +1 no verdadeiro R7

rla R12                    ; R12 = 2*i                               ; esquerda multiplica por 2 e direita divide por 2

add.w R10, R12             ; R10 = save, R12 = save + 2*i = &save[i] ; multiplicou por 2 porque anda de 2 posições em 2 posições

cmp 0(R12), R9             ; Compara save[i] com k (R9)              ; tipo 200 pro 202,204, etc.

                                                                     ; O R12 andou 0 posições e foi enviado pro R9
                                                                     
jeq EXIT                   ; save[ i ] != k? 

inc.w R7                   ; i += 1;

jmp LOOP


EXIT:




4. "Traduza" o seguinte trecho de código em C para o assembly do MSP430:

	for(i=0; i<100; i++) 
  
	A[i] = i*2;
  

INICIO:


mov.w R7, R12    ; joga o A pro R12 pra não modificar R7 é o i

mov.w #100, R13

rla R12

add.w R9, R12   ;  R9 = A, R12 = A + 2*i = &A[i] 

comp R12, R13

jne EXIT

add.w R12,R12

mov.w R12,R9

inc.w R7

jmp INICIO

EXIT

mov.w #0, R7

	LOOP:
  
	mov.w R7, R12
  
	cmp R12, #100
  
	jeq FIM
  
	add.w R12, R12      //// Se eu usar o rla eu mudo a posição
  
	mov.w R12, R9
  
	inc.w R7
  
	jmp LOOP
  
	FIM:
	

5. "Traduza" o seguinte trecho de código em C para o assembly do MSP430:

	for(i=99; i>=0; i--) 
  
	A[i] = i*2;
	
  
INICIO:

mov.w R7, R12    ; joga o A pro R12 pra não modificar R7 é o i

mov.w #100, R13

rla R12

add.w R9, R12   ;  R9 = A, R12 = A + 2*i = &A[i] 

comp R12, R13

jne EXIT

add.w R12,R12

mov.w R12,R9

dec.w R7

jmp INICIO

EXIT
