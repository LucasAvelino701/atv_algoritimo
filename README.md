# atv_algoritimo
construa um algoritmo que simule uma carteira digital, com os campos: cliente; saldo; número (1000); tipo de ocnta (01-corrente, 02-poupança, 03-salário). A carteira possui 5 clientes, e o seguinte menu principal: *Abrir carteira, *Consultar carteira e *Sair.

#include <stdio.h>
#include <string.h>

#define MAX_CLIENTES 5

// Definição da estrutura da Carteira Digital
typedef struct {
    char cliente[50];
    int numero;
    int tipo; // 1-Corrente, 2-Poupança, 3-Salário
    float saldo;
} Carteira;

// Protótipos das funções solicitadas
void cadastrar_cliente(Carteira carteiras[], int *qtd);
void exibir_carteira(Carteira carteiras[], int qtd);

int main() {
    Carteira carteiras[MAX_CLIENTES];
    int qtd_cadastrada = 0;
    int opcao;
    int flag = 0; // Controlador do loop do menu

    // Inicializa o vetor de carteiras limpando o lixo de memória
    for (int i = 0; i < MAX_CLIENTES; i++) {
        carteiras[i].numero = 0;
        carteiras[i].saldo = 0.0f;
        carteiras[i].tipo = 0;
        strcpy(carteiras[i].cliente, "");
    }

    do {
        printf("\n=== MENU PRINCIPAL ===\n");
        printf("1. Abrir carteira\n");
        printf("2. Consultar carteira\n");
        printf("3. Sair\n");
        printf("Escolha uma opcao: ");
        scanf("%d", &opcao);
        getchar(); // Limpa o buffer do teclado

        switch (opcao) {
            case 1:
                cadastrar_cliente(carteiras, &qtd_cadastrada);
                break;
            case 2:
                exibir_carteira(carteiras, qtd_cadastrada);
                break;
            case 3:
                printf("Saindo do sistema...\n");
                flag = 1;
                break;
            default:
                printf("Opcao invalida! Tente novamente.\n");
        }
    } while (flag == 0);

    return 0;
}

// Função para cadastrar/abrir uma nova carteira
void cadastrar_cliente(Carteira carteiras[], int *qtd) {
    if (*qtd >= MAX_CLIENTES) {
        printf("\nErro: Limite maximo de %d clientes atingido!\n", MAX_CLIENTES);
        return;
    }

    printf("\n--- ABRIR CARTEIRA ---\n");
    printf("Nome do cliente: ");
    fgets(carteiras[*qtd].cliente, sizeof(carteiras[*qtd].cliente), stdin);
    // Remove a quebra de linha gerada pelo fgets
    carteiras[*qtd].cliente[strcspn(carteiras[*qtd].cliente, "\n")] = '\0';

    // Atribuição automática do número da conta a partir de 1000[cite: 1]
    carteiras[*qtd].numero = 1000 + *qtd;
    printf("Numero da conta atribuido: %d\n", carteiras[*qtd].numero);

    printf("Tipo da conta (01-Corrente, 02-Poupanca, 03-Salario): ");
    scanf("%d", &carteiras[*qtd].tipo);

    printf("Saldo inicial (R$): ");
    scanf("%f", &carteiras[*qtd].saldo);
    getchar(); // Limpa o buffer

    (*qtd)++;
    printf("Carteira aberta com sucesso!\n");
}

// Função para exibir as informações e estatísticas das carteiras[cite: 1]
void exibir_carteira(Carteira carteiras[], int qtd) {
    if (qtd == 0) {
        printf("\nNenhuma carteira cadastrada no momento.\n");
        return;
    }

    float valor_total = 0.0f;
    int qtd_corrente = 0;
    int qtd_poupanca = 0;
    int qtd_salario = 0;

    // Calcula o saldo total e conta os tipos de conta
    for (int i = 0; i < qtd; i++) {
        valor_total += carteiras[i].saldo;

        if (carteiras[i].tipo == 1) {
            qtd_corrente++;
        } else if (carteiras[i].tipo == 2) {
            qtd_poupanca++;
        } else if (carteiras[i].tipo == 3) {
            qtd_salario++;
        }
    }

    printf("\n========================================\n");
    printf("=== SALDO ===\n");
    printf("R$ %.2f\n\n", valor_total);

    printf("=== CLIENTES ===\n");
    for (int i = 0; i < qtd; i++) {
        printf("_%s: R$ %.2f_\n", carteiras[i].cliente, carteiras[i].saldo);
    }

    printf("\n=== TIPOS ===\n");
    printf("01-Corrente: %d\n", qtd_corrente);
    printf("02-Poupanca: %d\n", qtd_poupanca);
    printf("03-Salario: %d\n", qtd_salario);
    printf("========================================\n");
}

