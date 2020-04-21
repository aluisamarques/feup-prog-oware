#include <iostream>
#include <string>
#include <ctime>
#include <cstdlib>
#include <windows.h>
#include <algorithm>
#include <time.h>

using namespace std;

#define BLACK 0
#define LIGHTRED 12
#define WHITE 15
#define GREEN 2
#define CYAN 3

bool gameOver, validPlay = true, wrong_list, empty_letter,empty_array, single_player, validMood;
int score_1, score_2, playernum, total_seeds, idx_1, idx_2, keep_positions, zero_1, zero_0,seeds, put_a_zeros,list, idx_final;
int playerBoard[2][6] = {{4,4,4,4,4,4},{4,4,4,4,4,4}};
const char refBoard[2][6] = {{'f','e','d','c','b','a'},{'A','B','C','D','E','F'}};
char ref;
int counter = 1 ; // para começar o jogador 1 a jogar, ou seja playernum = 1
string player_1, player_2, player_0, mood, input;

int start()
{   //system("cls");
    cout <<"W E L C O M E   T O   O W A R E";
    cout <<endl<< "whenever you want to end the game, write 'end'" <<endl;
    validMood = false; // variável validMood é alterada para verdadeira se caractere introduzido for válido
    char modo;
    while (!validMood){
        cout << endl << "for single player version, press 's'"<< endl <<"for multiplayer version, press 'm'"<< endl;
        cin >> mood;

        if (mood == "end"){
            gameOver = true;
            return 0;
            //system('cls');
        }

        modo = mood[0];
        if (modo == 's'|| modo == 'S'|| modo == 'm'|| modo == 'M'){
            validMood = true;
        }
        else if (cout << endl << "invalid char");
    }
    if (modo == 's' || modo == 'S'){
        single_player = true;
        cout << "player: ";
        cin >> player_0;
    }
    if (modo == 'm' ||modo == 'M'){
    cout <<"player_1: ";
    cin >> player_1  ;
    cout << endl << "player_2: ";
    cin >> player_2;
    single_player = false;
    }

    if (single_player){
        player_1 = player_0;
        player_2 = "computer";
    }


}

void setcolor(unsigned int color)
{   // colorir quadro: player 1 escolhe letras azuis e player 2 escolhe letras a rosa, restante quadro a verde
    HANDLE hcon = GetStdHandle(STD_OUTPUT_HANDLE);
    SetConsoleTextAttribute(hcon, color);
}

void board()
{   // desenha o tabuleiro no início e de cada vez que é inserida uma letra válida, mostrando a jogada seguinte
    cout << endl ;
    zero_1 = 0; // variáveis para verificar se o jogador tem sementes para utilizar na funçao que
    zero_0 = 0; // obriga o jogador a dar sementes ao adversário
    setcolor(3);
    cout << "          |  ";

    for (int a = 5; a >= 0; --a)
    {    // tabuleiro de letras jogador 1
        cout << refBoard [0][a] << "  |  ";
    }
    cout << "score " << player_1 <<": " << score_1 ;
    cout << endl << "          #####################################"<< endl;
    setcolor(2);
    cout << "          #     #     #     #     #     #     #     "<< endl;
    cout << "          #  " ;

    for (int a = 5; a > -1; --a)
    {   // tabuleiro de numeros jogador 1
        if (playerBoard[0][a] < 10) {
            cout << playerBoard[0][a] << "  #  ";
        }
        else{
            cout << playerBoard[0][a] << " #  ";
        }
        zero_0 = zero_0 + playerBoard[0][a]; //para verificar se o jogador está sem sementes
    }
    cout <<endl << "          #     #     #     #     #     #     #     "<< endl;
    setcolor(15);
    cout << "          #####################################"  << endl;
    setcolor(2);
    cout << "          #     #     #     #     #     #     #     "<< endl;
    setcolor(2);
    cout << "          #  " ;

    for (int a = 0; a < 6; ++a)
    {   // tabuleiro de números jogador 2
        setcolor(2);
        if (playerBoard[1][a] < 10) {
            cout << playerBoard[1][a] << "  #  ";
        }
        else{
            cout << playerBoard[1][a] << " #  ";
        }
        zero_1 = zero_1 + playerBoard[1][a];
    }
    cout << endl <<"          #     #     #     #     #     #     #     "<< endl;
    setcolor(12);
    cout <<               "          #####################################" << endl ;
    cout <<"          |  ";

    for (int a = 0; a < 6; ++a)
    {   // tabuleiro de letras jogador 2
        cout << refBoard[1][a] <<  "  |  ";
    }
    cout << "score "<< player_2 <<": " << score_2 << endl << endl;
    setcolor (15);
}

void gameover()
{   // casos em que se perde sem estar o tabuleiro vazio, ao atingir 25 pontos ou empatar com 24
    if (score_1 >= 25){
        gameOver = true;
        validPlay = false;
        cout << "G A M E O V E R" << endl << "the winner is " << player_1;
    }
    if  (score_2 >= 25){
        gameOver = true;
        validPlay = false;
        cout << "G A M E O V E R" << endl << "the winner is " << player_2;
    }
    if (score_1 == 24 && score_2 == 24){
        gameOver = true;
        validPlay = false;
        cout << "G A M E O V E R" << endl << "it's a draw";
    }
}

void empty_player()
{   // verifica se tabuleiro de numeros de um jogador está sem sementes
    // caso o jogador mover sementes para o tabuleiro do adversário, valida jogada
    // caso o jogaor não mova sementes para o tabuleiro do adversário invalida a jogada
    // e o computador avisa do erro uma vez que empty_array é verdadeiro
    if ((zero_1 == 0 || zero_0 == 0)) {
        empty_array = true;
        validPlay = false;
        if (playerBoard[idx_1][idx_2] + idx_2 > 5) {
            empty_array = false;
            validPlay = true;
        }
    }
    else{
        validPlay = true;
        empty_array = false;
    }
}

void test()
{   /* testa se na jogada seguinta ainda há sementes possíveis para mover,
    para, nos casos em que um dos jogadores está sem sementes , nao deixar o jogo continuar*/
    bool no_more_plays;
    int fakeBoard[2][6];
    int idx, sum_seeds = 0;
    for (int i = 0; i < 2; ++i) {
        for (int j = 0; j < 6; ++j) {
            fakeBoard[i][j] = playerBoard[i][j];
        }
    }
    if ((zero_1 == 0 || zero_0 == 0)) {
        no_more_plays = true;
        for (idx = 0; idx < 6; ++idx) {
            sum_seeds = sum_seeds + fakeBoard[idx_1][idx];
            if (fakeBoard[idx_1][idx] + idx > 5) {
                no_more_plays = false;
            }
        }
    }
    /* se a variável no_more_plays estiver verdadeira, verificou-se que nao há jogadas possíveis
    o jogador que ainda tem sementes junta-as ao score e o jogo acaba sendo o vencedor quem tem maior pontuaçao;
    se a variável no_more_plays estiver falsa, significa que o jogador consegue ainda dar sementes ao adversário
    e assim o jogo continua*/

    if (no_more_plays && counter > 1) {
        if (zero_1 == 0) {
            score_1 = score_1 + sum_seeds;
            cout << "score " << player_1 <<":" << score_1 << " "<< "                        score " << player_2 <<":" << score_2 << endl;
            if (score_2 < score_1){
            cout << "gameover" << endl << "The winner is " << player_1;}
            else if (score_2>score_1){
                cout  << "gameover" << endl << "The winner is "<<player_2;
            }
            else{
                cout << "gameover" << endl << "It's a draw";
            }
            gameOver = true;
        }
        if (zero_0 == 0) {
            score_2 = score_2 + sum_seeds;
            cout << "score " << player_1 <<": " << score_1 << endl <<
            "                        score " << player_2 <<": " << score_2 << endl;
            if (score_2 < score_1){
            cout  << "G A M E O V E R" << endl << "The winner is "<<player_1;}
            else if (score_1 < score_2) {
                cout  << "G A M E O V E R" << endl << "The winner is "<<player_2;
            }
            else{
                cout << "G A M E O V E R" << endl << "It's a draw";
            }
            gameOver = true;
        }
        validPlay = false;
    }
}

void moves()
{   /*função que move as sementes em caso da letra escolhida ser do tabuleiro correto*/
    put_a_zeros = idx_1; // variavel guarda indice da lista para colocar valor da letra escolhida a zero
    int i = idx_2;
    validPlay = true;
    total_seeds = playerBoard[idx_1][i]; // guarda valor de sementes, uma vez que variável "seeds" é alterada
    seeds = total_seeds;
    if (total_seeds < 6)
    { // em caso de o numero de sementes não ser suficiente para dar ao adversário,
      // verifica se o adversario tem sementes
        empty_player();
    }
    if (total_seeds >= 12)
    { // uma vez que o valor da letra inicial é incrementado, de forma a passar a casa à frente,
        // o numero de sementes aumenta-se no numero de vezes que passa na casa inicial
        seeds = seeds + (total_seeds/6 - 1);
    }
    if (validPlay)
    { // se a jogada for válida, aumenta o indice dos elementos da lista do jogador, e aumenta o numero nessas casas,
        // de forma a mover as sementes para a direita.
        // chegando a ultima casa da lista do jogador, passa para a lista do adversário
        // ou seja o indice 1 é alterado para o numero da outra lista, e o indice 2 volta a zero
        // (notar que a lista 0 está desenhada ao contrário logo indice 0 é o da direita)
        while (idx_2 <= 5 && seeds != -1 && total_seeds != 0) {
            ++playerBoard[idx_1][idx_2];
            --seeds;
            ++idx_2;
            if (idx_2 > 5) {
            idx_1 = (idx_1 + 1) % 2;
            idx_2 = 0;
            }
        }
        playerBoard[put_a_zeros][i] = 0; // casa da letra escolhida fica a zero
        keep_positions = idx_2 - 1; // variável utilizada para guardar o indice da casa onde
        // foi colocada a ultima semente, uma vez que a variavel idx_2 é alterada
        if (keep_positions < 0) {
            keep_positions = 5;
            idx_1 = (idx_1+1)%2;
        }
    }
}

void capture()
{   // quando ultima casa que recebe sementes fica com 2 ou 3,
    // sementes sao capturadas e todas imediatamente anteriores com o estes numeros tambem
    list = idx_1;
    if((playernum == 1 && list == 1)||(playernum == 2  && list == 0)){
            while ((playerBoard[list][keep_positions] == 2 || playerBoard[list][keep_positions] == 3)) {
                //se a casa onde parou tem 2 ou 3, aumenta o score
                //e verifica se a casa anterior tambem tem 2 ou 3
                if (playernum == 1 && keep_positions >= 0){
                    score_1 = score_1 + playerBoard[list][keep_positions];
                    playerBoard[list][keep_positions] =0 ;
                    --keep_positions;
                }
                if(playernum == 2 && keep_positions >=0){
                    score_2 = score_2 + playerBoard[list][keep_positions];
                    playerBoard[list][keep_positions] = 0;
                    --keep_positions;
                }
                if(keep_positions < 0)
                {// se a variavel for menor que zero, passou para a lista do proprio jogador,
                    // logo não captura esta semente
                    break;
                }
            }
    }
}

void logic()
{   // função que contém as funções que movem sementes e capturam sementes, se a letra escolhida for válida
    idx_1 = playernum - 1; // indice da lista do jogador
    wrong_list = true; // variavel para verificar se a letra escolhida pertence à lista do jogador em questao
    empty_letter = true; // variavel para verificar se a letra escolhida está vazia
    validPlay = false; // se até ao final da função logica a jogada for alterada, a jogada foi válida
    for (idx_2 = 0; idx_2 < 6; ++idx_2) {
        if (ref == refBoard[idx_1][idx_2])
        {   //se a letra escolhida pertence ao quadro de letras do jogador, está na lista correta
            // logo variavel wrong_list fica falsa
            wrong_list = false;
            if(playerBoard[idx_1][idx_2]!=0) {
                // se a casa escolhida contem sementes, variavel altera-se
                empty_letter = false;
                moves();
                if (playerBoard[idx_1][keep_positions] == 2 || playerBoard[idx_1][keep_positions]== 3){
                    capture();
                }
            }
        }
    }
}

void validation()
{   //função utilizada para aumentar a variavel que troca o número do jogador,
    // caso até aqui todos os parametros indicarem que a jogada foi válida.
    // caso contrario, o numero do jogador vai manter-se igual, para ele escolher outra letra ate ser valida a jogada
    if(wrong_list || empty_letter){
        validPlay = false;
    }
    if (validPlay){
        ++counter;
    }
}
int singlePlayer()
{   // função para jogada do computador
    // todas as jogadas possíveis seguintes e escolhe a que o faz ficar com maior pontuação
    // em caso de haver mais que uma casa cujo movimento das sementes resulta na mesma pontuação,
    // escolhe a casa através da função random, e caso haja um acasa com 10 ou mais sementes, escolhe essa casa
    int index_0, index_1, index_2, seed, bigger_score=0, score=0;
    int fakeBoard[2][6], keeping;

    for (index_2 = 0; index_2 < 6; ++index_2){
        index_1 = index_2;
        index_0=1;
        seed = playerBoard[index_0][index_1];
        if (seed >= 12) {
            seed = seed + (seed/6 - 1);
        }
        for (int i = 0; i < 2; ++i) {
            for (int j = 0; j < 6; ++j)
            {   //fakeboard tem de ser igual ao tabuleiro de numeros do jogador no teste de cada um os índices
                fakeBoard[i][j] = playerBoard[i][j];
            }
        }

        // -- faz o mesmo que a função moves() --
        while (index_1 <= 5 && seed != -1)
        {   ++fakeBoard[index_0][index_1];
            --seed;
            ++index_1;
            if (index_1 > 5) {
                index_0 = (index_0 + 1) % 2;
                index_1 = 0;
            }
        }
        fakeBoard[1][index_2] = 0; // casa da letra escolhida fica a zero
        keeping= index_1 - 1;
        if (keeping < 0) {
            keeping = 5;
            index_0 = (index_0 + 1) % 2;
        }

        // -- faz o mesmo que a função capture() --
        if (index_0 == 0){
            score = 0;
            while ((fakeBoard[0][keeping] == 2 || fakeBoard[0][keeping] == 3)) {
                score = score + playerBoard[0][keeping];
                --keeping;
                if(keeping < 0){
                    break;
                }
            }
        }

        // -- escolhe o índice com base na maior pontuação --
        if (score >= bigger_score && playerBoard[1][index_2]!=0){
            if (score == bigger_score){
                int r = rand() % 2;
                if (r == 0){
                    idx_final = index_2;
                }
                if (playerBoard[1][index_2]>10){
                    idx_final = index_2;
                }
            }
            else
            {
                bigger_score = score;
                idx_final = index_2;
            }
        }
    }
    // variável correspondente à letra escolhida, com base no índice escolhido anteriormente
    ref = refBoard[1][idx_final];
}

int Input()
{   // função que recebe as letras escolhidas
    // e informa se a jogada foi inválida com base nas variaveis que estão veradadeiras
    setcolor(15);
    playernum = (counter + 1) % 2 + 1;
    if (empty_array) {
        cout << "choose a letter that allows your opponent to play: move seeds to his board" << endl;
        if (single_player && playernum == 2){
            singlePlayer();
        }
    }
    else if(empty_letter && !wrong_list){
        cout << "this place has no seeds to move, choose a letter that contains at least one seed" << endl;

    }
    if(wrong_list){
        cout << "you're choosing a letter that doesn't belong to your board" << endl;
    }
    if (playernum == 1) {
        cout << player_1 << ", enter a letter (lowercase):" << endl;
        cin >> input;
    }
    if(playernum == 2 && !single_player) {
        cout << player_2 << ", enter a letter (uppercase):" << endl;
        cin >> input;
    }
    if (input == "end"){
        gameOver = true;
    }
    if (playernum == 2 && single_player){
        cout << "the computer is playing..." << endl;
        ref = refBoard[1][idx_final];
        _sleep(5000);
        cout << "letter:" << ref << endl;
    }
    if (input.length() != 1 || input == "end"){
        validPlay = false;
    }
    else if (!single_player || playernum != 2) {
        ref = input[0];
        cout << "letter: " << ref << endl;
    }
}

 int main()
 {  start();
    if (!gameOver) {
    board();
        while(true) {

            test();
            if (gameOver) {
                start();
                if (gameOver) {
                    return 0;
                }
            }
            if (single_player && playernum == 1) {
                // o playernum só é aumentado na função input() logo aqui utiliza-se num = 1
                singlePlayer();
            }
            Input();
            if (input == "end") {
                return 0;
            }
            logic();
            if (validPlay) {
                //system("cls");
                board();
            }
            gameover();
            validation();
            if (gameOver) {
                return 0;
            }
        }
    }
 }
