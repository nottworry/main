#include <stdio.h>
#include <locale.h>
#include <stdlib.h>

int *createArray(int *arr, int *N);
void insert(int *arr, int i1, int k, int N);
void de1ete(int *arr, int i1, int k, int N);
int enterIndex(int N);
int enterElement(int N);
int add(int *arr, int N);
int del(int *arr, int N);
int addMultiple(int *arr, int N);
int delMultiple(int *arr, int N);
void arrayPrint(int *arr, int N);
void getRandomArray(int *arr, int N);
void getArray(int *arr, int N);
int getchoice();

int main() {
	system("chcp 1251");
	system ("cls");
	printf("Задание - 1\n\n");
	int choice = 1, N, x, y, select;
	int *arr;

	while(choice) {
		select=1;
		arr=createArray(arr, &N);
		while(select) {
			fflush(stdin);
			printf("\n Использовать рандомный масив - 1\n Использовать введенный масив - 2 \n");
			scanf("%d", &x);
			switch(x) {
				case 2:
					getArray(arr, N);
					select=0;
					break;
				case 1:
					getRandomArray(arr,N);
					select=0;
					break;
				default:
					printf("\nОшибка. Повторите ввод \n");
			}
		}
		arrayPrint(arr, N);
		select=1;
		while(select) {
			fflush(stdin);
			printf("\n1 - Добавить один элемент в массив\n");
			printf("2 - Удалить один элемент массива\n");
			printf("3 - Добавить несколько элементов в массив\n");
			printf("4 - Удалить несколько элементов массива\n");
			printf("0 - обратно <--\n");
			scanf("%d", &y);
			switch(y) {
				case 0:
					select=0;
					break;
				case 4:
					N=delMultiple(arr, N);
					break;
				case 3:
					N=addMultiple(arr, N);
					break;
				case 2:
					N=del(arr, N);
					break;
				case 1:
					N=add(arr, N);
					break;
				default:
					printf("\nОшибка. Повторите ввод \n");
			}
		}
		choice = getchoice();
	}
}

int *createArray(int *arr, int *N) {
		while(1) {
		printf("Введите размер массива N: \n");
		if(!scanf("%d", &*N)) {
			printf("Введено некорректное  значение. Введите число!\n");
			fflush(stdin);
			continue;
		}  
		else if (*N<0 || *N==0) {
			printf("Недопустимый размер массива\n");
			fflush(stdin);
			continue;
		}
		else arr=(int *)calloc(*N, sizeof(int));
		if((!arr)) {
			printf("Недопустимый размер массива\n");
			fflush(stdin);
			continue;
		} else return;
	}

}


void insert(int *arr, int i1, int k, int N) { 
	int i;
	for(i=N-1; i>i1; i--) arr[i]=arr[i-k];
}
void de1ete(int *arr, int i1, int k, int N) { 
	int i;
	for(i=i1; i<N-k; i++)arr[i]=arr[i+k];
}

int enterIndex(int N) { 
	int index;
	while(1) {
		printf("Введите индекс: \n>");
		if(!scanf("%d", &index)) {
			printf("Введено некорректное  значение. Введите число!\n");
			fflush(stdin);
		} else if ( index>N || index<0 ) {
			printf("Введено некорректное  значение. Индекс за пределами массива!\n");
			fflush(stdin);
		} else break;
	}
	return index;
}

int enterElement(int N) { 
	int element;
	while(1) {
		if(!scanf("%d", &element)) {
			printf("Введено некорректное  значение. Введите число!\n");
			fflush(stdin);
		} else if ( element>9 || element<0 ) {
			printf("Введено некорректное  значение. Введите число от 0 до 9 !\n");
			fflush(stdin);
		} else break;
	}
	return element;
}

int add(int *arr, int N) {
	int i1,el;
	printf("Введите индекс элемента, который нужно вставить:\n>");
	i1=enterIndex(N);
	printf("Введите значение элемента:\n>");
	el=enterElement(N);
	N++;
	arr=(int *)realloc(arr,N*sizeof(int));
	insert(arr, i1, 1, N);
	arr[i1]=el;
	printf("Элемент добавлен!\n");
	arrayPrint(arr, N);
	return N;
}

int del(int *arr, int N) {
	int i1;
	printf("Введите индекс элемента, который нужно удалить:\n>");
	i1=enterIndex(N);
	de1ete(arr, i1, 1, N);
	N--;
	printf("Элемент удален.\n");
	arrayPrint(arr, N);
	return N;
}

int addMultiple(int *arr, int N) {
	int k,i1,el,i;
	printf("Введите индекс элемента начала:\n>");
	i1=enterIndex(N);
	while(1) {
		printf("Введите количество вставляемых элементов:\n>");
		if(!scanf("%d", &k)) {
			printf("Введено некорректное  значение. Введите число!\n");
			fflush(stdin);
		} else if ( k<1 ) {
			printf("Введено некорректное  значение. Введите число от 1!\n");
			fflush(stdin);
		} else break;
	}
	N+=k;
	arr=(int *)realloc(arr,N*sizeof(int));
	insert(arr, i1, k, N);
	printf("Введите значение элементов:\n>");
	for(i=i1; i<i1+k; i++) {
		el=enterElement(N);
		*(arr+i)=el;
	}
	printf("Элементы добавлены.\n");
	arrayPrint(arr, N);
	return N;
}

int delMultiple(int *arr, int N) {
	int i1,k;
	printf("Введите индекс элемента, с которого нужно удалить:\n>");
	i1=enterIndex(N);
	while(1) {
		printf("Введите количество удаляемых элементов:\n>");
		if(!scanf("%d", &k)) {
			printf("Введено некорректное  значение. Введите число!\n");
			fflush(stdin);
		} else if ((i1+k)>=N || k<1 ) {
			printf("Ошибка. Указанные элементы уходят за пределы массива!!\n");
			fflush(stdin);
		} else break;
	}
	de1ete(arr, i1, k, N);
	N-=k;
	printf("Элементы удалены.\n");
	arrayPrint(arr, N);
	return N;
}

void arrayPrint(int *arr, int N) {
	int  i;
	printf("\n");
	for( i=0; i < N; i++ ) {
		printf("|%d| ", arr[i]);
	}
}

void getArray(int *arr, int N) {
	int i;
	printf("\nВведите масив \n");
	for(i = 0; i < N ; i++ ) {
		arr[i]=enterElement(N);
	}
	printf("\nМассив \n");
}

void getRandomArray(int *arr, int N) {
	int i;
	printf("\n\n Рандомный массив \n");
	for(i = 0; i < N; i++) {
		arr[i]= rand()%10;
	}
}

int getchoice() {
	int choice;
	while(1) {
		printf("\n\nПовторить задание введите - 1\nЗавершить программу - 0\n>");
		scanf("%d", &choice);
		switch(choice) {
			case 1:
				system ("cls");
				fflush(stdin);
				return 1;
			case 0:
				system ("cls");
				return 0;
			default:
				printf("Выбран не существующий вариант\n");
				fflush(stdin);
				system ("pause");
				system ("cls");
		}
	}
}

