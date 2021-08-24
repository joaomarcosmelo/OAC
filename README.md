# OAC
OAC/C/ASSEMBLE
/*
Universidade de Brasília
Organização e Arquitetura de Computadores (OAC)
Professor: Ricardo Jacobi
Turma: C
1/2021

Aluno: João Marcos Melo Monteiro
Matrícula: 13/0143031

Trabalho 1 em C/C++

Compilador online: "Replit" compilador online e eclipe(IDE)
Navegador: Opera
OS: Windows 10, Windows 10 Pro
*/


#include <stdio.h>
#include <stdint.h>

#define MEM_SIZE 4096
int32_t mem[MEM_SIZE];


void menu();

int32_t lw(uint32_t address, int32_t kte);

int32_t lb(uint32_t address, int32_t kte);

int32_t lbu(uint32_t address, int32_t kte);

void sw(uint32_t address, int32_t kte, int32_t dado);

void sb(uint32_t address, int32_t kte, int8_t dado);

void mem_begin();

void teste1();

void teste2();

void testesInd();

int main(){

	mem_begin(); 
    
	menu();
    
	return 0;
}

void menu(){
  int option;
	do{

		//system("clear"); 
		printf("\t\tTrabalho 1 - OAC\n\n");
		printf("Selecione uma opção abaixo:\n\n");
		printf("1 - Teste 1\n");
		printf("2 - Teste 2\n");
		printf("3 - Teste Individual de função\n");
		printf("4 - Sair\n");
		printf("Opção:");
		scanf("%d", &option);
    printf("\n\n");

		switch(option){

			case 1:
				teste1();
				break;
			case 2:
				teste2();
				break;
			case 3:
				testesInd();
				break;
			case 4:
				return;
			default:
				printf("Opção inválida!\n");
				break;
		}
	}while(option != 4);
}

void testesInd(){

  int option;
	do{
		
    //system("clear");
		printf("\t\tEscolha uma função:\n\n");
		printf("Selecione uma opção abaixo:\n\n");
		printf("1 - sw\n");
		printf("2 - sb\n");
		printf("3 - lw\n");
		printf("4 - lb\n");
    printf("5 - lbu\n");
    printf("6 - Sair\n");
		printf("Opção:");
		scanf("%d", &option);
    printf("\n\n");

    int address, kte, dado;

		switch(option){

			case 1:
				printf("address: ");
        scanf("%d", &address);
        printf("kte: ");
        scanf("%d", &kte);
        printf("dado: ");
        scanf("%x", &dado);

        sw(address, kte, dado);
				break;
			case 2:
				printf("address: ");
        scanf("%d", &address);
        printf("kte: ");
        scanf("%d", &kte);
        printf("dado: ");
        scanf("%x", &dado);

        sb(address, kte, dado);
				break;
			case 3:
				printf("address: ");
        scanf("%d", &address);
        printf("kte: ");
        scanf("%d", &kte);

        printf("lw(%d,%d) = %08X", address, kte, lw(address, kte));
				break;
      case 4:
				printf("address: ");
        scanf("%d", &address);
        printf("kte: ");
        scanf("%d", &kte);

        printf("lb(%d,%d) = %08X", address, kte, lb(address, kte));
				break;
			case 5:
				printf("address: ");
        scanf("%d", &address);
        printf("kte: ");
        scanf("%d", &kte);

        printf("lbu(%d,%d) = %08X", address, kte, lbu(address, kte));
				break;
			case 6:
				return;
			default:
				printf("Opção inválida!\n");
				break;
		}
    printf("\n\n");
	}while(option != 6);
}

void teste1(){

    printf("%08X\n", mem[0]);
    printf("%08X\n", mem[1]);
    printf("%08X\n", mem[2]);
    printf("%08X\n", mem[3]);
    printf("%08X\n", mem[4]);
    printf("%08X\n", mem[5]);
    printf("%08X\n", mem[6]);

    /*printf("Pressione enter");
    
    system("pause");*/ 
    return;
}

void teste2(){

    int i;
    for(i=0; i < 4; i++){
        printf("lb(4,%d) = %08X\n", i, lb(4,i));
    }
    printf("\n");
    for(i=0; i < 4; i++){
        printf("lbu(4,%d) = %08X\n", i, lbu(4,i));
    }
    printf("\n");
    for(i=12; i <= 20; i+=4){
        printf("lw(%d,0) = %08X\n", i, lw(i,0));
    }
    
    /*printf("Pressione enter");
    
    system("pause");*/ 
    return;
}

void mem_begin(){
    
    sb(0, 0, 0x04);
	sb(0, 1, 0x03);
	sb(0, 2, 0x02);
	sb(0, 3, 0x01);
	
	sb(4, 0, 0xFF);
	sb(4, 1, 0xFE);
	sb(4, 2, 0xFD);
	sb(4, 3, 0xBC);
	
	sw(12, 0, 0xFF);
    sw(16, 0, 0xFFFF);
    sw(20, 0, 0xFFFFFFFF);
    sw(24, 0, 0x80000000);
    
    return;
}

int32_t lw(uint32_t address, int32_t kte){

	if ((address+kte)%4 != 0 || (address+kte)/4 > MEM_SIZE){
		printf("Endereço (%d, %d) inválido!\nFavor fornecer um endereço válido.\n", address, kte);
		return 0;
	}

	return mem[(address+kte)/4];
}

int32_t lb(uint32_t address, int32_t kte){

	if ((address+kte)/4 > MEM_SIZE){
		printf("Endereço (%d, %d) inválido!\nFavor um endereço válido.\n", address, kte);
		return 0;
	}
	
	char *pos = (char*)mem;

	return (int32_t)pos[(address+kte)];
}

int32_t lbu(uint32_t address, int32_t kte){


    if ((address+kte)/4 > MEM_SIZE){
		printf("Endereço (%d, %d) inválido!\nFavor um endereço válido.\n", address, kte);
		return 0;
	}
	
	char *pos = (char*)mem;
	int32_t aux = (int32_t)pos[(address+kte)];
	
	if(aux < 0){
		aux = 0xffffffff - aux;
		aux = 0xff - aux; // operação para zerar bits
		//aux = aux & 0x00000011; // mascara de dados hexadecimal
	}
	return aux;
}

void sw(uint32_t address, int32_t kte, int32_t dado){

	if ((address+kte)%4 != 0 || (address+kte)/4 > MEM_SIZE){
		printf("Endereço (%d, %d) inválido!\nFavor um endereço válido.\n", address, kte);
		return;
	}

	mem[(address+kte)/4] = dado;

	return;
}

void sb(uint32_t address, int32_t kte, int8_t dado){

    if ((address+kte)/4 > MEM_SIZE){
		printf("Endereço (%d, %d) inválido!\nFavor um endereço válido.\n", address, kte);
		return;
	}
	
	char *pos = (char*)mem;

	pos[(address+kte)] = dado;

	return;
}
