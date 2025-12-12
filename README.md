#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>

#ifdef _WIN32
    #include <windows.h>

    #define clear() system("cls")

    void sleep_ms(int ms){
        Sleep(ms);
    }
#endif


void desenhaForca(int erros) {

    // base
    printf("  ________\n");
    printf("  |      |\n");

    // cabeça do boneco
    if (erros >= 1)
        printf("  |      O\n");
    else
        printf("  |\n");

    // tronco + braços
    if (erros == 2)
        printf("  |      |\n");
    else if (erros == 3)
        printf("  |     /|\n");
    else if (erros >= 4)
        printf("  |     /|\\\n");
    else
        printf("  |\n");

    // pernaaas
    if (erros == 5)
        printf("  |     /\n");
    else if (erros >= 6)
        printf("  |     / \\\n");
    else
        printf("  |\n");

    // final da forca (bonecoo)
    printf("  |\n");
    printf("__|__\n\n");
}


void telaInicial() {

    clear();

    printf("=========================================\n");
    printf("           JOGO DA FORCA\n");
    printf("=========================================\n\n");

    printf("Disciplina: APC - Algoritmos e Programacao de Computadores\n");
    printf("Professor : Clenio Emidio\n\n");

    printf("Pressione ENTER para iniciar o jogo...");
    getchar(); // Aguarda ENTER
}


int main() {


    telaInicial();


    char *palavras[] = {
        "computador",
        "programacao",
        "teclado",
        "monitor",
        "janela",
        "foguete",
        "galaxia",
        "ingles",
        "ferramenta",
        "algoritmo",
        "matematica",
        "ciencia",
        "escola"
    };

    //total de palavras na forca
    int totalPalavras = sizeof(palavras) / sizeof(palavras[0]);

    srand(time(NULL));

    char palavra[50];
    strcpy(palavra, palavras[rand() % totalPalavras]);


    int tamanho = strlen(palavra); // tamanho palavraa

    char tentativa;

    int erros = 0, acertos = 0;
    int maxErros = 6;

    // palavra armazenada no vetor (tamanho dela)
    char exibicao[50];
    for (int i = 0; i < tamanho; i++) {
        exibicao[i] = '_';
    }
    exibicao[tamanho] = '\0';


    while (1) {

        clear();

        printf("====== JOGO DA FORCA ======\n\n");

        desenhaForca(erros);

        printf("Palavra: ");
        for (int i = 0; i < tamanho; i++) {
            printf("%c ", exibicao[i]);
        }

        printf("\n\nDigite uma letra: ");
        scanf(" %c", &tentativa);  

        int encontrou = 0;

 
          /* verifica se a letra digitada existe na palavra
           caso exista, substitui '_' pela letra correta. */

        for (int i = 0; i < tamanho; i++) {
            if (palavra[i] == tentativa && exibicao[i] == '_') {
                exibicao[i] = tentativa;
                acertos++;
                encontrou = 1;
            }
        }

        // errou -> soma
        if (!encontrou) {
            erros++;
            clear();
            printf("Letra errada!\n");
            desenhaForca(erros);
            sleep_ms(700);
        }


        if (acertos == tamanho) {
            clear();
            desenhaForca(erros);
            printf("\nVOCÊ GANHOU!!!\n");
            printf("A palavra era: %s\n", palavra);
            break;
        }

        if (erros >= maxErros) {
            clear();
            desenhaForca(erros);
            printf("\nVOCÊ PERDEU!\n");
            printf("A palavra era: %s\n", palavra);
            break;
        }
    }

    printf("\nPressione ENTER para sair...");
    getchar();
    getchar();

    return 0;
}
