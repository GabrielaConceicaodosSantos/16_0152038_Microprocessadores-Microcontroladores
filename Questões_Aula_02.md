# 16_0152038_Microprocessadores-Microcontroladores
Disciplina Microprocessadores e Microcontroladores, Curso de Engenharia Eletrônica da UnB, Campus Gama. Período 2/2017.

                                                      QUESTÕES: AULA 02
                                                      
                                                      
Q1. Quais as diferenças entre os barramentos de dados e de endereços?

✔ Barramento de dados: é por onde os dados são transmitido bidirecionalmente (enviando e recebendo dados). Por exemplo, os dados podem 
ser lidos na memório ou escritos.

✔ Barramento de endereço:é por onde são enviados os locais (origem ou destino) de armazenamento. São unidirecionais, pois esse endereço
é armazenado somente uma vez.

Q2. Quais são as diferenças entre as memórias RAM e ROM?

✔ A memória RAM não armazena os conteúdos permanentemente. Mas é responsável pela leitura de dados quando os mesmos são requeridos, por isso
Random Access Memory (Memória de Acesso Aleatório).
A memório ROM (Read-Only Memory) oferece dados somente para leitura. É uma memório de armazenamento permanente.

Q3. Considere o código abaixo:

    1.   #include <stdio.h>
    2.   int main(void)
    3.   {
    4.      int i;
    5.      printf("Insira um número inteiro: ");
    6.      scanf("%d", &i);
    7.      if(i%2)
    8.         printf("%d eh impar.\n");
    9.      else
   10.         printf("%d eh par.\n");
   11.      return 0;
   12.   }

Para este código, responda:

(a) A variável 'i' é armazenada na memória RAM ou ROM? Por quê?

✔ Na memória RAM, pois ela fica temporariamente. Quando reinicia o programa tem-se que armazenar o valor de i novamente.

(b) O programa compilado a partir deste código é armazenado na memória RAM ou ROM? Por quê?

✔ Na memório ROM, pois após salvo ele não será apagado ao desligar o computador.

Q4. Quais são as diferenças, vantagens e desvantagens das arquiteturas Harvard e Von Neumann?

✔ A arquitetura Havard os endereços e os dados a serem armazenados na memória são compilados ao mesmo tempo, existem componentes distintos
para isso. Logo é mais rápido o processamento, porém é mais complexo seus componentes internos.
Na arquitetura Von Neumann é processada uma informação por vez, no mesmo barramento.Logo o processo é mais lento, mas a arquitetura é 
mais simples.

Q5. Considere a variável inteira 'i', armazenando o valor 0x8051ABCD. Se 'i' é armazenada na memória a partir do endereço 0x0200, 
como ficam este byte e os seguintes, considerando que a memória é:

✔ (a) Little-endian?

	0x0200: 0xCD
  
	0x0201: 0xAB
  
	0x0202: 0x51
  
	0x0203: 0x80
	
✔ (b) Big-endian?

	0x0200: 0x80
  
	0x0201: 0x51
  
	0x0202: 0xAB
  
	0x0203: 0xCD

Q6. Sabendo que o processador do MSP430 tem registradores de 16 bits, como ele soma duas variáveis de 32 bits?

    ✔ Uma palavra de 32 bits é separada em dois registradores de 16 bits, vamos chamar de palavra A. A outra palavra também é separada e 
armazenada em 2 registradores de 16 bits, vamos chamar de B. Os registrados com os bits menos significativos de A e de B são somados e 
se existir carry ele será armazenado. Feito isso será somado os registradores com bits mais significativos levando em consideraçã a 
informação do carry armazenada na soma. Logo após juntam-se esses resultados.
