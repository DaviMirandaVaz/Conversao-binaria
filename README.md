#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<iostream>
#include<math.h>
#include<sstream>

#define MAX 100

/*
	Trabalho feito por: Davi Miranda Vaz
	4° periodo BCC
	Ra: 202111000029 
*/


double convertdouble( char numt[]) {
    int i;
	double num = 0;
	char numt2[100];
	for(i=0;i<strlen(numt2);i++){
		numt2[i] = '0';
	}
	
	for(i=0;i<strlen(numt);i++){
		if(numt[i] == ','){
			numt2[i] = '.';
			continue;
		}
		numt2[i] = numt[i];
	}
    
    strcpy(numt, numt2);
    
    std::stringstream ss(numt);
    ss >> num;
    
    return num;
}







void convertbin(double num, char numbin[]){
	if(num<0){
		num = -num;
	}
	long int  num1 = 0, op2 = 1, j = 0, i = 0;
	double aux1 = 0;
	char aux = '0';
	for(i=0;i<strlen(numbin);i++){
		numbin[i] = 'f';
	}
	i = 0;
	num1 = num;
	while(op2 != 0){
		if(num1 == 0){
			numbin[i] = '0';
			break;
		}
		if(num1 == 1){
			numbin[i] = '1';
			break;
		}
		if(num1%2 == 0){
			numbin[i] = '0';
		}else{
			numbin[i] = '1';
		}
		if(num1 <= 3){
			numbin[i+1] = '1';
			op2 = 0;
		}
		num1 = num1/2;
		i++;
	}	
	for(j = 0; j <= i/2; j++){
		aux = numbin[i - j];
		numbin[i - j] = numbin[j];
		numbin[j] = aux;
	}
	
	op2 = 1;
	i++;
	numbin[i] = ',';
	i++;
	num1 = num;
	aux1 = num1;
	num = num - aux1;	
	if(num != 0){
		while(op2 != 0){
			num = num*2;
			if(num < 1){
				numbin[i] = '0';
			}else if(num > 1){
				numbin[i] = '1';
				num = num - 1;
			}else if(num == 1){
				numbin[i] = '1';
				op2 = 0;
			}
			i++;
		}
	}
}

void normalizabin(char numbin[]){
	long int i, j, intp, exp;
	char numbin2[MAX];
	memset(numbin2, 'f', MAX);
	numbin2[0] = '0';
	numbin2[1] = ',';
	
	for(i=0;i<strlen(numbin);i++){
		if(numbin[i] == '1'){
			intp = i;
			break;
		}
	}
	i = 2;
	j = intp;
	while(i < strlen(numbin) && j < strlen(numbin)){
		if(numbin[j] == ','){
			j++;
		}else{
			numbin2[i] = numbin[j];
			i++;
			j++;
		}
		
	}
	strcpy(numbin, numbin2);
}


void convertnumbineg(char numbin[], int n){
	int i, aux;
	char bineg[MAX], mostra[MAX];
	memset(mostra, 'f', MAX);
	memset(bineg, 'f', MAX);
	strcpy(mostra, numbin);
	normalizabin(mostra);
	printf("\nNumero normal:\n1 ");
	for(i=0;i<MAX;i++){
		if(mostra[i] == 'f'){
			break;
		}
		printf("%c", mostra[i]);
	}
	for(i=0;i<MAX;i++){
		if(numbin[i] == 'f'){
			break;
		}else if(numbin[i] == '1'){
			bineg[i] = '0';
		}else if(numbin[i] == '0'){
			bineg[i] = '1';
		}else if(numbin[i] == ','){
			bineg[i] = ',';
		}
	}//inverte
	strcpy(mostra, bineg);
	normalizabin(mostra);
	printf("\nComplemento de 1:\n1 ");
	for(i=0;i<MAX;i++){
		if(bineg[i] == 'f'){
			break;
		}
		printf("%c", bineg[i]);
	}
	
	for(i=0;i<MAX;i++){
		if(bineg[i] == 'f'){
			i--;
			aux = i;
			break;
		}
	}//pega ultimo bit
	
	for(i=0;i<=aux;i++){
		if(bineg[aux - i] == '0'){
			bineg[aux - i] = '1';
			break;
		}else if(bineg[aux - i] == '1'){
			bineg[aux - i] = '0';
		}
	}
	strcpy(numbin, bineg);
	strcpy(mostra, numbin);
	normalizabin(mostra);
	printf("\nComplemento de 2:\n0 ");
	for(i=0;i<MAX;i++){
		if(mostra[i] == 'f'){
			break;
		}
		printf("%c", mostra[i]);
	}
	printf("\n\n");
}

int pegaexp(char numbin[], double num){
	long int i, j, exp, v = 0;
	if(num >= 1){
		for(i=0;i<MAX;i++){
			if(numbin[i] == 'f'){
				break;
			}
			if(numbin[i] == ','){
				exp = i;
				return exp;
			}
		}
	}else if(num <= -2){
		for(i=0;i<MAX;i++){
			if(numbin[i] == 'f'){
				break;
			}
			if(numbin[i] == '1'){
				for(j=i;j<MAX;j++){
					if(numbin[i] == 'f'){
						break;
					}
					if(numbin[j] == ','){
						break;
					}
					v++;
				}
				return v;
				
			}
		}
	}else if(num < 0){
		if(numbin[0] == '1'){
			return 1;
		}else{
			for(i=2;i<MAX;i++){
				if(numbin[i] == 'f'){
					break;
				}
				if(numbin[i] == '1'){
					return -(i-2);
				}
			}
		}
	}else if(num > 0){
		for(i=2;i<MAX;i++){
				if(numbin[i] == 'f'){
					break;
				}
				if(numbin[i] == '1'){
					return -(i-2);
				}
		}
	}
}


double convertdec(char numfin[], char numfineg[], int exp, int expneg, double num, int n){
	int i, tam, j;
	char numfin2[MAX];
	double numd = 0, aux;
	for(i=0;i<MAX;i++){
		numfin2[i] = 'f';
	}
	
	
	
	for(i=2;i<MAX;i++){
		if(numfin[i] == 'f'){
			break;
		}
		numfin2[i-2] = numfin[i];
	}
	strcpy(numfin, numfin2);
	
	
	if(num > 0){
		for(i=0;i<MAX;i++){
			if(numfin[i] == 'f'){
				break;
			}
			if(numfin[i] == '1'){
				numd+= pow(2, ((exp-1)-i));
			}
		}
		return numd;
	}else if(num <= -1){
		for(i=0;i<MAX;i++){
			if(numfineg[i] == 'f'){
				tam = i-1;
				break;
			}
		}
		for(i=0;i<MAX;i++){
			if(numfineg[i] == 'f'){
				break;
			}
			if(numfineg[tam-i] == '1'){
				numfineg[tam-i] = '0';
				break;
			}else if(numfineg[tam -i ] == '0'){
				numfineg[tam - i] = '1';
			}
		}
		
		for(i=0;i<MAX;i++){
			if(numfineg[i] == 'f'){
				break;
			}
			if(numfineg[i] == '0'){
				numfineg[i] = '1';
			}else if(numfineg[i] == '1'){
				numfineg[i] = '0';
			}
		}
		

		memset(numfin2, 'f', MAX);
		for(i=0;i<MAX;i++){
			if(numfineg[i] == 'f'){
				break;
			}
			if(numfineg[i] == ','){
				for(j=i+1;j<MAX;j++){
					numfin2[j-1] = numfineg[j];
				}
				break;
			}else{
				numfin2[i] = numfineg[i];
			}
		}
		strcpy(numfineg, numfin2);
		for(i=0;i<n;i++){
			if(numfineg[i] == 'f'){
				break;
			}
			if(numfineg[i] == '1'){
				numd+= pow(2, ((expneg-1)-i));
			}
		}
		return -numd;
	}else if(num < 0 && num > -1){
		for(i=0;i<MAX;i++){
			if(numfineg[i] == 'f'){
				tam = i-1;
				break;
			}
		}
		for(i=0;i<MAX;i++){
			if(numfineg[i] == 'f'){
				break;
			}
			if(numfineg[tam-i] == '1'){
				numfineg[tam-i] = '0';
				break;
			}else if(numfineg[tam -i ] == '0'){
				numfineg[tam - i] = '1';
			}
		}
		
		for(i=0;i<MAX;i++){
			if(numfineg[i] == 'f'){
				break;
			}
			if(numfineg[i] == '0'){
				numfineg[i] = '1';
			}else if(numfineg[i] == '1'){
				numfineg[i] = '0';
			}
		}
		

		memset(numfin2, 'f', MAX);
		for(i=0;i<MAX;i++){
			if(numfineg[i] == 'f'){
				break;
			}
			if(numfineg[i] == ','){
				for(j=i+1;j<MAX;j++){
					numfin2[j-1] = numfineg[j];
				}
				break;
			}else{
				numfin2[i] = numfineg[i];
			}
		}
		strcpy(numfineg, numfin2);

		for(i=0;i<n;i++){
			if(numfineg[i] == 'f'){
				break;
			}
			if(numfineg[i] == '1'){
				numd+= pow(2, ((exp-1)-i));
			}
		}
		return -numd;
	}
	
}

int pegatam(char numbin[100], double num){
	int i;
	for(i=0;i<MAX;i++){
		if(numbin[i] == 'f'){
			return i;
		}
	}
}



int main(){
	int op = 0, n, l, u, op1 = 1, exp, i, expfin, expneg, tam2;
	double num, num2, numfincabo;
	char numfin[MAX], numfineg[MAX], numt[100];
	do{
		system("cls");
		printf("Menu de opcoes:\n");
		printf("1 . Preencher parametros.\n");
		printf("2 . Inserir numeros.\n");
		printf("0 . Encerrar programa.\n");
		printf("Escolha uma opcao:   ");
		scanf("%d", &op);
		system("cls");
		switch(op){
			case 1:
				printf("preencha os paramtros no formato(n, l, u):\n");
				scanf("%d, %d, %d", &n, &l, &u);
			break;	
			case 2:
				while(op1 != 0){
					system("cls");
					char numbin[MAX];
					memset(numbin, 'f', MAX);
					memset(numfin, 'f', MAX);
					memset(numfineg, 'f', MAX);
					for(i=0;i<strlen(numt);i++){
							numt[i] = '0';
					}
					printf("Digite o numero a ser usado ou 0 caso queira voltar ao menu:\n");
					scanf("%s", numt);
					num = convertdouble(numt);
					num2 = num;
					if(num == 0){
						op1 = 1;
						break;
					}
					convertbin(num, numbin);
					
					tam2 = pegatam(numbin, num2);
					
					if(num2 < 0){
						expneg = pegaexp(numbin, num2);
						convertnumbineg(numbin, n);
						strcpy(numfineg, numbin);
					}
					exp = pegaexp(numbin, num2);
					normalizabin(numbin);
					printf("Numero finalizado:\n", exp);
					for(i=0;i<n+2;i++){
						if(numbin[i] == 'f'){
							break;
						}
						printf("%c", numbin[i]);
						numfin[i] = numbin[i];
					}

					if(exp < u && exp > l){
						printf(" * 2 ^ %d\n", exp);
						expfin = exp;
					}else if(exp >= u){
						printf(" * 2 ^ %d\n", u);
						expfin = u;						
					}else if(exp <= l){
						printf(" * 2 ^ %d\n", l);
						expfin = l;
					}
					
					
					
					numfincabo = convertdec(numfin, numfineg, expfin, expneg, num, n);
					printf("\n%lf  ", numfincabo);
					
					if(num2>0){
						if(exp > u){
							printf(" overflow\n", u);
							
						}else if(exp < l){
							printf(" underflow\n", l);
						}else if(tam2 > n){
							printf(" truncamento\n", u);
						}
					}else{
						if(expneg > u){
							printf(" overflow\n", u);
							
						}else if(expneg < l){
							printf(" underflow\n", l);
						}else if(tam2 > n){
							printf(" truncamento\n", u);
						}
					}
					system("pause");
				}
			break;
			case 3:
				printf("Programa encerrado!");
			break;
		}
	}while(op != 0);
}







