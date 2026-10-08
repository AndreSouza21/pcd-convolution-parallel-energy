# Desempenho e sustentabilidade de convoluções paralelas

Este repositório reúne o notebook, o código gerado por ele e os resultados de um estudo experimental sobre o desempenho de filtros de convolução 2D paralelizados em memória compartilhada. O objetivo é observar como o tamanho do kernel e a estratégia de paralelização afetam o tempo de execução e métricas relacionadas à eficiência energética.

## O que é avaliado

O benchmark compara quatro caminhos de execução para filtros de imagem RGB:

- versão sequencial, usada como referência;
- Pthreads, com divisão explícita das linhas de saída;
- OpenMP por linhas;
- OpenMP com decomposição em blocos 2D.

São avaliados o filtro Sobel 3×3 e filtros Gaussiano e Box com kernels de 3×3, 7×7, 11×11 e 15×15. As imagens de entrada são geradas no notebook em diferentes resoluções. Cada variante paralela é validada contra a referência sequencial pixel a pixel.

## Conteúdo do repositório

- `colab_codigo.ipynb` — notebook que gera os arquivos-fonte C, compila o benchmark, executa os experimentos e exporta os resultados;
- `results_colab.csv` — resultados detalhados por configuração;
- `tabela_colab.csv` — tabela resumida para consulta;
- `ambiente.txt` — informações registradas sobre o ambiente experimental.

## Como executar

1. Abra [`colab_codigo.ipynb`](https://colab.research.google.com/github/AndreSouza21/pcd-convolution-parallel-energy/blob/main/colab_codigo.ipynb) no Google Colab.
2. Selecione **Ambiente de execução → Executar tudo**.
3. Ao final, consulte ou baixe o CSV produzido pelo notebook.

Os parâmetros do experimento — como resoluções, tamanhos de kernel, número de repetições e aquecimentos — podem ser ajustados no bloco de configuração do notebook.

## Resultados e interpretação

A versão atual reúne 189 configurações executadas, com as saídas registradas como validadas. Nos dados e na análise do experimento, o speedup observado com duas threads cresce para os kernels maiores, chegando a aproximadamente 1,19 no kernel 15×15. Esse resultado descreve a execução no ambiente usado e não deve ser generalizado automaticamente para outros processadores.

**A energia não foi medida diretamente.** Como os contadores RAPL não estavam disponíveis no ambiente, o benchmark usou uma estimativa baseada no TDP e na ocupação de CPU (`TDP_ESTIMATE`). Portanto, energia, potência, EDP e MFLOPS/W derivados desse modelo são estimativas e não medições físicas; não permitem concluir qual configuração consumiria menos energia real.

## Limitações e reprodutibilidade

Os experimentos foram executados no Google Colab, em uma máquina virtual com duas vCPUs. A virtualização e a variação de carga podem afetar os tempos, e os resultados caracterizam essa execução específica. Para conclusões mais fortes sobre energia ou escalabilidade em hardware multicore, é necessário repetir os testes em uma máquina controlada, com medição direta de energia e registro completo da configuração de hardware e software.

**Nota sobre os metadados:** o notebook configura 10 repetições e 2 aquecimentos, enquanto o `ambiente.txt` atualmente registra 3 repetições e 1 aquecimento. Confira e sincronize esse arquivo de ambiente com a execução final antes de usar os metadados como registro definitivo.
