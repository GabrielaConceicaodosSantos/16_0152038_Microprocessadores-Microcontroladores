# 16_0152038_Microprocessadores-Microcontroladores
Disciplina Microprocessadores e Microcontroladores, Curso de Engenharia Eletrônica da UnB, Campus Gama. Período 2/2017.



                                                            QUESTÕES: AULA 06 e 07
                                                            
Para cada questão, escreva funções em C e/ou sub-rotinas na linguagem Assembly do MSP430. Reaproveite funções e sub-rotinas de uma 
questão em outra, se assim desejar. Leve em consideração que as sub-rotinas são utilizadas em um código maior, portanto utilize 
adequadamente os registradores R4 a R11. As instruções da linguagem Assembly do MSP430 se encontram ao final deste texto.

1. (a) Escreva uma função em C que calcule a raiz quadrada 'x' de uma variável 'S' do tipo float, utilizando o seguinte algoritmo: após 
'n+1' iterações, a raiz quadrada de 'S' é dada por

	x(n+1) = (x(n) + S/x(n))/2

O protótipo da função é:

	unsigned int Raiz_Quadrada(unsigned int S)


  
RESPOSTA:
  
 unsigned int Raiz_Quadrada(unsigned int S){
 
	float x = 1;
  
	for(int n = 1; n<5; n++){             //número de iterações de x
  
	x = (x + S/x)/2; 
  
}

return x;

}



(b) Escreva a sub-rotina equivalente na linguagem Assembly do MSP430. A variável 'S' é fornecida pelo registrador R15, e a raiz quadrada
de 'S' (ou seja, a variável 'x') é fornecida pelo registrador R15 também.




RESPOSTA:

;R15 recebe S

;Lembrando que para passar parâmetros a subrotina, usasse os registradores R15, R14, R13 e R12 (nessa ordem).

Raizquadrada:    

	mov.w #1, R13; 

  mov.w #1, R14;  
  
	LOOP: 
  
	; divisao por x
	
DIVISAO:	

	mov.w R15, R12; 
  
	sub.w R1, 
  
	add.w R12, R15; 
  
	rla.w R15; 
  
	inc.w R14; 
  
	call LOOP; 

2. (a) Escreva uma função em C que calcule 'x' elevado à 'N'-ésima potência, seguindo o seguinte protótipo: 

	int Potencia(int x, int N);



RESPOSTA:

int Potencia(int x, int N)      /no assembly x vai pra R15 e N pra R14

	{
  
	    int y;
      
		if(N==1) return x;
    
		if(N==0) return 1;
    
		y = x*Potencia(x, --N);
    
		return y;
    
	}




(b) Escreva a sub-rotina equivalente na linguagem Assembly do MSP430. 'x' e 'n' são fornecidos através dos registradores R15 e R14, 
respectivamente, e a saída deverá ser fornecida no registrador R15.



RESPOSTA:

Inicio:
	cmp #1, R14               			; cmp:  N=1 se 1<R14 (R14-1<0) e N=0 se (R14>=1)
  
	jeq FIM       ; se R14=1 vai para label:FIM     ;       Z=1 se R14=1 (R14-1=0) e Z=0 se (R14 é diferente de 1)
  
	cmp #0, R14                     	        ;       C=1 se R14-1 há carry e C=0 caso contrário
  
	jeq PotenciaZero                                ;       V=1 se R14-1 há overflow e V=0 caso contrário
  
	mov.w R15, R12 ; move R15 pra R12
  
	mov.w R15, R11 ; move R15 pra R11
  
	clr.w R15      ; limpa o R15
  
	mov.w R12, R13 ; move R12 pra R13

Multiplicacao:

	add.w R12, R15 ; R15= R15+R12
  
	cmp.w #1, R13
  
	dec.w R13
  
	jne Multiplicacao
  
	mov.w R11, R13
  
	dec.w R14
  
	cmp #1, R14
  
	mov.w R15, R12
  
	jeq FIM
  
	clr.w R15          ; Tem que limpar o R15 no final, pois usamos como variável temporária e depois pode está sendo usada em outra
  
	jmp Multiplicacao  ; parte do código.


PotenciaZero:

	mov.w #1, R15

FIM:

	ret                                               ; Se é uma função deve retornar onde foi chamada?!
	

3. Escreva uma sub-rotina na linguagem Assembly do MSP430 que calcula a divisão de 'a' por 'b', onde 'a', 'b' e o valor de saída são 
inteiros de 16 bits. 'a' e 'b' são fornecidos através dos registradores R15 e R14, respectivamente, e a saída deverá ser fornecida 
através do registrador R15.



RESPOSTA:

	clr.w R12
	
Inicio:

	mov.w R14, R11            ; R11<-R14
  
	sub.w R14, R15            ; R15=R15-R14
  
	add.w #1, R12             ; R12=R12+1
	
Compara:
	dec.w R11
  
	cmp R11, R15
  
	jeq Fim
  
	cmp #0, R11
  
	jeq Inicio
  
	jmp Compara
  
	
Fim:

	mov.w R12, R15
  
	ret
	



4. Escreva uma sub-rotina na linguagem Assembly do MSP430 que calcula o resto da divisão de 'a' por 'b', onde 'a', 'b' e o valor de saída são inteiros de 16 bits. 
'a' e 'b' são fornecidos através dos registradores R15 e R14, respectivamente, e a saída deverá ser fornecida através do registrador R15.



RESPOSTA:

clr.w R12

Inicio:

	mov.w R14, R11
  
	sub.w R14, R15
  
	add.w #1, R12
	
Compara:

	dec.w R11
  
	cmp R11, R15
  
	jeq Fim
  
	cmp #0, R11
  
	jeq Inicio
  
	jmp Compara
	
Fim:

	mov.w R12, R15
	
Resto:

mov.w R11, R15

ret



5. (a) Escreva uma função em C que indica a primalidade de uma variável inteira sem sinal, retornando o valor 1 se o número for primo, e
0, caso contrário. Siga o seguinte protótipo:

	int Primalidade(unsigned int x);

(b) Escreva a sub-rotina equivalente na linguagem Assembly do MSP430. A variável de entrada é fornecida pelo registrador R15, e o valor de saída também.

6. Escreva uma função em C que calcula o duplo fatorial de n, representado por n!!. Se n for ímpar, n!! = 1*3*5*...*n, e se n for par, n!! = 2*4*6*...*n. Por exemplo, 9!! = 1*3*5*7*9 = 945 e 10!! = 2*4*6*8*10 = 3840. Além disso, 0!! = 1!! = 1.
O protótipo da função é:

	unsigned long long DuploFatorial(unsigned long long n);

7. (a) Escreva uma função em C que calcula a função exponencial da seguinte forma:
	
Considere o cálculo até o termo n = 20. O protótipo da função é double ExpTaylor(double x);
(b) Escreva a sub-rotina equivalente na linguagem Assembly do MSP430, mas considere que os valores de entrada e de saída são inteiros de 16 bits. A variável de entrada é fornecida pelo registrador R15, e o valor de saída também.

8. Escreva uma sub-rotina na linguagem Assembly do MSP430 que indica se um vetor esta ordenado de forma decrescente. Por exemplo:
[5 4 3 2 1] e [90 23 20 10] estão ordenados de forma decrescente.
[1 2 3 4 5] e [1 2 3 2] não estão.
O primeiro endereço do vetor é fornecido pelo registrador R15, e o tamanho do vetor é fornecido pelo registrador R14. A saída deverá ser fornecida no registrador R15, valendo 1 quando o vetor estiver ordenado de forma decrescente, e valendo 0 em caso contrário.

9. Escreva uma sub-rotina na linguagem Assembly do MSP430 que calcula o produto escalar de dois vetores, 'a' e 'b':
	
O primeiro endereço do vetor 'a' deverá ser passado através do registrador R15, o primeiro endereço do vetor 'b' deverá ser passado através do registrador R14, e o tamanho do vetor deverá ser passado pelo registrador R13. A saída deverá ser fornecida no registrador R15.

10. (a) Escreva uma função em C que indica se um vetor é palíndromo. Por exemplo:
	[1 2 3 2 1] e [0 10 20 20 10 0] são palíndromos.
	[5 4 3 2 1] e [1 2 3 2] não são.
Se o vetor for palíndromo, retorne o valor 1. Caso contrário, retorne o valor 0. O protótipo da função é:

	int Palindromo(int vetor[ ], int tamanho);

(b) Escreva a sub-rotina equivalente na linguagem Assembly do MSP430. O endereço do vetor de entrada é dado pelo registrador R15, o tamanho do vetor é dado pelo registrador R14, e o resultado é dado pelo registrador R15.
