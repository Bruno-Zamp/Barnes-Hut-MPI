# Barnes-Hut-MPI
Trabalho de paralelização através de MPI, do algoritmo Barnes Hut.

## Motivação
O objetivo consiste na adaptação do algoritmo Barnes Hut, utilizado na 14th Marathon of Parallel Programming para um modelo que explore as possibilidades de paralelismo existentes.

## Definição
A implementação consistiu em: 
* Criar dois vetores auxiliares (force_xEnvia, force_yEnvia) para os vetores force_x e force_y, com tamanho correspondente ao tamanho desses vetores divididos pela quantidade de processos estabelecidos; 
* Primeiramente os cálculos são realizados nos vetores force_x e force_y, utilizando como índice uma lógica onde o processo inicia o índice na sua parte correspondente. (Ex.: rank 0: [0, 1, 2, 3, 4 ]  rank 1: [5, 6, 7, 8, 9]...); 
* Após isso, o valor calculado nos vetores originais é atribuído aos vetor auxiliares (force_xEnvia, force_yEnvia) com o índice incremental inciado em zero em todos processos, para posteriormente ser utilizado no MPI_Gather;
* Utiliza-se o MPI_Gather para juntar o vetores calculados em cada processo, para o processo 0. Após, utiliza-se o MPI_Bcast para enviar o vetor juntado no processo 0 para os demais processos.

## Rodando o algoritmo
```
time mpirun -np (n) ./barnes_hut 100 galaxy.in
```
Onde (n) é igual o número de processos selecionados

## Resultados obtidos em 5 repetições:
* Tempo médio sequencial: 4m57,942s / Speedup: 1 / Eficiência: 1
* Tempo médio 2 processos: 2m57,632s / Speedup: 1,77 / Eficiência: 0,88
* Tempo médio 4 processos: 1m55,758s / Speedup: 2,94 / Eficiência: 0,73
* Tempo médio 8 processos: 1m59,936s / Speedup: 2,87 / Eficiência: 0,35

## Hardware utilizado nos testes
CPU utilizada nos resultados: Intel(R) Core(TM) i5-8250U 1.60 GHz (Turbo Max 3.40 GHz), 3210 MHZ; 4 núcleos; 8 threads; 6 MB de cache. Memória: 8 GB, DDR4, 2666MHz.

## Conclusão
O paralelismo não pôde ser utilizado de forma significativa, pelas limitações do hardware utilizado e pelo uso de apenas uma máquina, porém através do resultado obtido, foi possível identificar uma boa redução no tempo de processamento na abordagem paralela. O melhor tempo ocorreu quando foram selecionados 4 processos para a execução, isso se deve pela limitação da CPU, a qual possui apenas 4 núcleos físicos.
