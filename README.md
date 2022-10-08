#include <stdbool.h>
#include <stdio.h>
#include <stdlib.h>
#include <malloc.h>
#include <string.h>

// char *tipoPessoa[3] = {"cliente", "atendente", "gerente"};

typedef struct Conta{
  struct Pessoa *titular; // CPF do titular
  char gerente[11]; // CPF do gerente
  char agencia[5];
  char numero[12];
  double saldo;
} Conta;

typedef struct Solicitacao{
  char descricao[200];
  bool resolvida;
} Solicitacao;

typedef struct Pessoa{
  char cpf[11];
  char nome[50];
  char tipo[3];
  struct Conta *conta;
  struct Pilha *solicitacoes; // Talvez usar uma estrutura de pilha
} Pessoa;

// Pilha

typedef struct RegistroSolicitacao {
  Solicitacao *solicitacao;
  struct RegistroSolicitacao *prox;
} RegistroSolicitacao;

typedef struct Pilha {
  struct RegistroSolicitacao *topo; /*aponta para o elemento qu esta Elemento topo da pilha*/
} Pilha;

RegistroSolicitacao *auxPilha;

Pilha* cria_pilha(){
    /*alocação do ponteiro pi para controlar a pilha*/
    Pilha* pi = (Pilha*) malloc(sizeof(Pilha));
    if(pi != NULL){
        pi->topo = NULL;  /*a pilha inicia-se vazia, portanto seu topo é igual a NULL*/
    }
    return pi;
}

/*todo elemento será inserido no topo da pilha*/
void insere_elemento(Pilha *pi){
	/*a cada inserção alocamos dinamicamente um espaço para um novo elemento*/
	RegistroSolicitacao *novo =(RegistroSolicitacao*) malloc(sizeof(RegistroSolicitacao));
  Solicitacao *solicitacao =(Solicitacao*) malloc(sizeof(Solicitacao));
  solicitacao->resolvida = false;
  
	printf("Digite a solicitação do cliente: ");    
	scanf("%s", solicitacao->descricao);
  
  novo->solicitacao = solicitacao;
	/*como o número inserido será o novo topo, ele apontará para o topo atual que será o segundo elemento da pilha*/
	novo->prox = pi->topo;
	/*topo aponta para o endereço de novo*/
	pi->topo = novo;
	printf("\Solicitação inserida na pilha!\n");
	// getch();
}

/*os elementos da pilha serão mostrados do último inserido(topo) ao primeiro*/
void consulta_pilha(Pilha *pi){
	/*caso a pilha esteja vazia*/
	if(pi->topo == NULL){
		printf("\nPilha Vazia!!");
	/*caso a pilha não esteja vazia*/
	}else{
		auxPilha = pi->topo;
		do{
			printf("Solicitção: %s\n", auxPilha->solicitacao->descricao);
			auxPilha = auxPilha->prox;
		} while(auxPilha != NULL);
	}
	// getch();
}

/*o elemento a ser removido será sempre o topo(último elemento inserido)*/
void remove_elemento_pilha(Pilha *pi){
	 if(pi->topo ==  NULL){
		printf("\nPilha Vazia!!");
	} else{
		auxPilha = pi->topo;
		printf("%s removido!", pi->topo->solicitacao->descricao);
		pi->topo = pi->topo->prox;
		free(auxPilha);
	}
	// getch();
}

/*a pilha será esvaziada e o espaço ocupado por ela será desalocado*/
void esvazia_pilha(Pilha *pi){
	if(pi->topo == NULL){
		printf("\nPilha Vazia!!");
	}else{
		auxPilha = pi->topo;
		do{
			pi->topo = pi->topo->prox;
			free(auxPilha);
			auxPilha = pi->topo;
		}while(auxPilha != NULL);
		printf("\nPilha Esvaziada!!");
	}
	// getch();
}


// Fila

void main() {
  Conta conta = {NULL,  "22222222222", "0001", "12345", 1000.0};
  
  // conta->gerente = "22222222222";
  // conta->agencia = "0001"
  // conta->numero = "12345"
  // conta->saldo = 1000.0;
  
  Pessoa cliente = { "11111111111", "João", "cliente", &conta, cria_pilha()};
  
  // cliente->cpf = "11111111111";
  // cliente->nome = "João";
  // cliente->tipo = "cliente";
  // cliente->conta = conta;
  // cliente->solicitacoes = cria_pilha();

  // conta->titular = cliente;

  insere_elemento(cliente.solicitacoes);
   insere_elemento(cliente.solicitacoes);
   insere_elemento(cliente.solicitacoes);

  consulta_pilha(cliente.solicitacoes);

  esvazia_pilha(cliente.solicitacoes);

  consulta_pilha(cliente.solicitacoes);

  // Como resolver solicitação?
  // Criar pilha auxiliar, para por as solicitações resolvidas
  // Muda reaolvida para true
  // Tira da pilha
  // Bota na pilha auxiliar
  // Vai para o proximo

  // Quando pilha vazia
  // Pega da aux e passa para a da pessoa

  return 0;  
}
