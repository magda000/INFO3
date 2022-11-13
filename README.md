# INFO3

#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>
#include <unistd.h>
 
#define NUM_THREADS 8
 
struct thread_data {
	int id;
	int sum;
	char *message;
};
 
char* messages[] = {
		"English: Hello World!",
		"French: Bonjour le monde!",
		"Spanish: Hola al mundo!",
		"Polski: Witaj swiecie!",
		"German: Guten Tag, Welt!",
		"Russian: Zdravstvytye, mir!",
		"Japan: Sekai e konnichiwa!",
		"Latin: Orbis, te saluto!"
};
 
void* thread_routine(void* data) {
	struct thread_data my_data = *(struct thread_data*) data;
	printf("\tWatek id: %d, control sum: %d, message: %s\n",
			my_data.id, my_data.sum, my_data.message);
 
	return NULL;
}
 
int main(void) {
/*	pthread_attr_t attr;
	pthread_attr_init(&attr);
	pthread_attr_setdetachstate(&attr, PTHREAD_CREATE_DETACHED);*/
 
	struct thread_data data[NUM_THREADS];
	for(int i = 0; i < 8; i++) {
		data[i].id = i + 1;
		data[i].sum = (i+1)*(i+2)/2;
		data[i].message = messages[i];
	}
 
	int rc;
	pthread_t thread_id[NUM_THREADS];
	for(int i = 0; i < 8; i++) {
		rc = pthread_create((thread_id + i), NULL, thread_routine, (void*) (data + i));
		if(rc) {
			printf("Blad: funkcja pthread_create() zwrocila kod: %d\n", rc);
			exit(EXIT_FAILURE);
		}
	}
	printf("Utworzono wszystkie watki\n");
 
	for(int i = 0; i < 8; i++) {
		rc = pthread_join(thread_id[i], NULL);
		if(rc) {
			printf("Blad: funkcja pthread_join() zwrocila kod: %d\n", rc);
			exit(EXIT_FAILURE);
		}
	}
 
	return 0;
}
