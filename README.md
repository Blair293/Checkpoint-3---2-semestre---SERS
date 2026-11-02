# Checkpoint 3 --- Clustering aplicado à energia

## Integrantes

-   **Integrante 1:** \[Nome completo\] --- **RM:** \[Número de RM\]
-   **Integrante 2:** \[Nome completo\] --- **RM:** \[Número de RM\]
-   **Turma:** \[Identificação da turma\]

> Antes da entrega, substitua os campos entre colchetes pelos dados dos
> integrantes e da turma.

## Objetivo da atividade

Esta atividade tem como objetivo aplicar técnicas de aprendizado de
máquina não supervisionado para identificar diferentes perfis de consumo
de energia entre unidades consumidoras. Foi utilizado o algoritmo
**K-Means**, com apoio do método **Elbow** e do **Silhouette Score**,
para definir a quantidade adequada de grupos (*clusters*).

A análise considera três características: consumo mensal de energia,
demanda máxima e percentual de consumo noturno. A partir dos grupos
encontrados, são descritos os perfis e propostas ações de acompanhamento
e gestão de energia.

## Descrição da base de dados

A base contém **120 unidades consumidoras fictícias**, geradas
previamente com NumPy e semente aleatória 303 para fins didáticos. Os
dados estão incorporados ao próprio notebook em formato CSV; portanto,
não é necessário baixar uma base externa.

A base possui quatro colunas:

  -----------------------------------------------------------------------
  Coluna                  Descrição               Unidade
  ----------------------- ----------------------- -----------------------
  `id_unidade`            Identificador de cada   Identificação
                          unidade consumidora     

  `consumo_mensal_kwh`    Energia total consumida kWh
                          no mês                  

  `demanda_maxima_kw`     Maior demanda de        kW
                          potência média          
                          registrada em uma       
                          janela de 15 minutos no 
                          mês                     

  `consumo_noturno_pct`   Parcela da energia      \%
                          mensal consumida entre  
                          18h e 6h                
  -----------------------------------------------------------------------

O identificador `id_unidade` é utilizado apenas para identificar os
registros e não participa do agrupamento. As três variáveis numéricas
são utilizadas no modelo.

**Observação:** os dados são simulados e não foram coletados de
medidores, empresas ou bases públicas. Assim, os resultados são
exclusivamente didáticos e não devem ser interpretados como uma
representação comprovada de consumidores reais.

## Tecnologias e bibliotecas

-   Python 3
-   Jupyter Notebook
-   pandas
-   NumPy
-   Matplotlib
-   scikit-learn

## Como executar o notebook

1.  Faça o download ou clone deste repositório.

2.  Abra o arquivo `Checkpoint_3_Clustering_Energia_preenchido.ipynb` no
    Jupyter Notebook, JupyterLab ou Google Colab.

3.  Caso necessário, instale as bibliotecas utilizadas:

    ``` bash
    pip install numpy pandas matplotlib scikit-learn jupyter
    ```

4.  Abra o notebook e execute as células na ordem, de cima para baixo.
    No Jupyter, também é possível selecionar **Run All**.

5.  A base de dados já está incorporada ao notebook, então não é
    necessário configurar caminhos ou arquivos externos.

6.  Confira as tabelas, os gráficos e as interpretações geradas pelas
    células.

## Etapas realizadas

1.  **Exploração dos dados:** verificação das dimensões, tipos de dados,
    valores ausentes, identificadores e estatísticas descritivas.
2.  **Preparação:** seleção das três variáveis numéricas e padronização
    com `StandardScaler`.
3.  **Método Elbow:** execução do K-Means para valores de k entre 1 e 8
    e análise da inércia.
4.  **Silhouette Score:** comparação dos agrupamentos para valores de k
    entre 2 e 8.
5.  **Modelo final:** escolha de k, treinamento do K-Means e inclusão
    dos rótulos dos grupos no DataFrame de resultados.
6.  **Interpretação:** visualização dos grupos e comparação das médias
    das variáveis originais para propor ações de gestão de energia.

## Principais resultados

### Escolha da quantidade de clusters

O método Elbow indicou uma região de cotovelo próxima de **k = 3**. A
inércia diminuiu de 360,00 para 184,70 entre k = 1 e k = 2, e para 74,25
em k = 3. A partir desse ponto, as reduções passaram a ser menores.

O maior Silhouette Score entre os valores testados também ocorreu em **k
= 3**, com resultado de **0,6031**. Como os dois métodos apontaram para
a mesma quantidade de grupos, foi escolhido um modelo final com três
clusters.

### Distribuição e perfis dos grupos

O modelo final agrupou as 120 unidades igualmente: **40 unidades em cada
cluster**.

  -----------------------------------------------------------------------
  Cluster         Quantidade Consumo mensal Demanda máxima        Consumo
                                      médio          média  noturno médio
  ----------- -------------- -------------- -------------- --------------
  0                       40     876,50 kWh        6,87 kW         25,49%

  1                       40     245,09 kWh        2,58 kW         60,82%

  2                       40   1.433,93 kWh        6,15 kW         67,37%
  -----------------------------------------------------------------------

Os resultados permitem resumir os perfis da seguinte forma:

-   **Cluster 0:** consumo mensal intermediário, maior demanda máxima
    média entre os grupos e menor participação de consumo noturno. Uma
    ação possível é investigar os horários de pico e avaliar
    oportunidades de deslocamento de cargas, conforme a rotina da
    unidade.
-   **Cluster 1:** menor consumo mensal e menor demanda máxima média,
    mas participação noturna relativamente alta. Recomenda-se acompanhar
    o consumo noturno e identificar quais equipamentos permanecem
    ligados nesse período.
-   **Cluster 2:** maior consumo mensal médio e maior participação
    noturna, com demanda máxima média inferior à do cluster 0.
    Recomenda-se priorizar a investigação do perfil horário e avaliar
    oportunidades de eficiência e gestão das cargas noturnas.

As propostas são pontos de partida para investigação. Consumo elevado,
isoladamente, não comprova desperdício. Recomendações sobre tarifas ou
instalação de geração solar exigiriam informações adicionais, como
contrato, horários detalhados, localização e análise de viabilidade.

## Conclusão

O agrupamento permitiu organizar as unidades consumidoras em três perfis
distintos considerando consumo mensal, demanda máxima e participação do
consumo noturno. Essa segmentação pode ajudar uma equipe de gestão de
energia a direcionar o acompanhamento e investigar necessidades
diferentes em cada grupo.

A escolha de três clusters foi sustentada pela região de cotovelo
observada no gráfico Elbow e pelo maior Silhouette Score obtido no
intervalo analisado (**0,6031**). O Silhouette Score avalia a coesão e a
separação dos grupos, não a acurácia, pois a base não possui rótulos
verdadeiros de classificação.

Como limitações, a base é simulada e contém somente 120 unidades. Além
disso, a análise utiliza três variáveis agregadas e o K-Means depende de
distâncias, podendo ser sensível a valores extremos e à escolha de k.
Para aprofundar o estudo, seria importante obter dados reais de consumo
por hora, histórico de vários meses, dias da semana, tipo de atividade,
equipamentos, localização, tarifas e contratos de demanda.

## Arquivo principal

-   `Checkpoint_3_Clustering_Energia_preenchido.ipynb` --- notebook com
    o código, as análises, as tabelas, os gráficos e as conclusões da
    atividade.
