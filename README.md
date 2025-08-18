# Desafio_ONE_Alura_TelecomX_parte_II_Cassiano

## :books: Sobre

Este desafio faz parte do curso de **Data Sciente** do **Programa ONE - Oracle Next Education** em parceria com a **Alura**. 

O **Challenge TelecomX - Parte II** tem como objetivo identificar os fatores que mais contribuem para o cancelamento de serviços por parte dos clientes e desenvolver modelos de Machine Learning capazes de prever a probabilidade de churn. As descobertas deste estudo visam subsidiar a empresa na implementação de estratégias proativas de retenção de clientes.

## :gear: Metodologia
A análise seguiu as etapas de um projeto de Machine Learning, incluindo:

*   **Extração e Exploração dos Dados:** Carregamento e inspeção inicial do dataset.
*   **Preparação dos Dados:** Tratamento de valores nulos, codificação de variáveis categóricas (One-Hot Encoding) e remoção de colunas irrelevantes.
*   **Análise de Correlação e Multicolinearidade:** Utilização de mapas de calor e Variance Inflation Factor (VIF) para identificar e mitigar a multicolinearidade entre as variáveis preditoras.
*   **Preparação para Modelagem:** Separação dos dados em conjuntos de treino e teste, e balanceamento da classe minoritária (Churn) utilizando SMOTE.
*   **Modelagem Preditiva:** Treinamento e avaliação de diversos modelos de classificação (Regressão Logística, Random Forest, SVC, Gradient Boosting, KNN, Decision Tree).
*   **Avaliação dos Modelos:** Comparação do desempenho dos modelos utilizando métricas como Acurácia, ROC AUC, Precision, Recall e F1-score, com foco no Recall para a classe Churn.
*   **Ajuste de Hiperparâmetros:** Otimização do modelo de Regressão Logística utilizando GridSearchCV.

## :bar_chart: Análise: Principais Fatores que Influenciam a Evasão (Churn)

<div>
  <img src="https://github.com/obaldin/Desafio_ONE_Alura_TelecomX_parte_II_Cassiano/blob/main/Imagens_geradas/heatmap_variaveis_correlacao_com_churn_yes_plot.png?raw=true">
  <img src="https://github.com/obaldin/Desafio_ONE_Alura_TelecomX_parte_II_Cassiano/blob/main/Imagens_geradas/Correlation_of_final_variables_with_churn_yes_plot.png?raw=true">
</div>

Com base na análise de correlação e na importância das variáveis nos modelos, os seguintes fatores foram identificados como os que mais influenciam a evasão de clientes:

*   **Tempo de Contrato (Tenure):** Clientes com menor tempo de contrato apresentam maior probabilidade de evasão. A correlação negativa com Churn_Yes (-0.35) indica que quanto maior o tempo como cliente, menor a chance de churn.
*   **Tipo de Contrato (Contract_Two year):** Clientes com contratos de dois anos têm uma probabilidade significativamente menor de evasão em comparação com contratos mensais ou de um ano. A correlação negativa (-0.30) reforça este ponto.
*   **Serviço de Internet (InternetService_Fiber optic):** Clientes que utilizam serviço de internet de fibra óptica apresentam uma correlação positiva com Churn_Yes (0.31), sugerindo que este tipo de serviço pode estar associado a uma maior propensão à evasão. Isso pode estar relacionado a problemas de qualidade, custo ou concorrência neste segmento.
*   **Método de Pagamento (PaymentMethod_Electronic check):** Clientes que utilizam cheque eletrônico como método de pagamento têm uma correlação positiva com Churn_Yes (0.30), indicando que este método de pagamento está associado a uma maior probabilidade de evasão.
*   **Total de Cobranças Mensais (ChargesMonthly):** Clientes com cobranças mensais mais altas tendem a ter uma maior probabilidade de evasão, embora a correlação seja moderada (0.19).
*   **Faturamento Total (ChargesTotal):** Embora removida na análise final de VIF devido à multicolinearidade, a variável `ChargesTotal` originalmente apresentava uma correlação negativa com churn, indicando que clientes com maior histórico de gastos tendem a ser mais leais. A relação entre `ChargesMonthly`, `ChargesTotal` e `Tenure` é complexa e foi tratada para evitar redundâncias no modelo final.
*   **Faturamento Diário (TotalDay):** Similar a `ChargesTotal`, `TotalDay` também apresentava correlação com churn, mas foi removida na análise de VIF.
*   **Senior Citizen:** Clientes idosos (SeniorCitizen = 1) apresentaram uma correlação positiva com Churn_Yes (0.15), sugerindo que este grupo pode ter uma propensão ligeiramente maior à evasão.
*   **Paperless Billing (PaperlessBilling_Yes):** Clientes que optam por faturamento sem papel também apresentaram uma correlação positiva com Churn_Yes (0.19).

É importante notar que outras variáveis como `OnlineSecurity_Yes`, `TechSupport_Yes`, `Partner_Yes` e `Dependents_Yes` apresentaram correlações negativas com churn, indicando que clientes com esses serviços ou características tendem a ser mais leais.

## :chart_with_upwards_trend: Desempenho dos Modelos Preditivos

Foram avaliados diversos modelos de classificação para prever o churn. A métrica **Recall (Churn)** foi considerada prioritária, pois o objetivo é identificar o maior número possível de clientes em risco para ações de retenção.

O gráfico abaixo resume o desempenho dos modelos nas principais métricas:

<div>
  <img src="https://github.com/obaldin/Desafio_ONE_Alura_TelecomX_parte_II_Cassiano/blob/main/Imagens_geradas/comparison_model_performance_metrics_optimized_plot.png?raw=true">
</div>

*   A **Regressão Logística Otimizada** apresentou o maior Recall para a classe Churn (0.81). Isso significa que o modelo foi capaz de identificar 81% dos clientes que realmente evadiram no conjunto de teste. Embora sua Acurácia (0.751) e Precision (0.52) sejam inferiores a outros modelos, seu alto Recall a torna a mais adequada para o objetivo de identificar o máximo de churners.
  
<div>
  <img src="https://github.com/obaldin/Desafio_ONE_Alura_TelecomX_parte_II_Cassiano/blob/main/Imagens_geradas/regressa_logistica_otimizada_plot.png?raw=true">
</div>

*   O **Gradient Boosting** também apresentou um bom Recall (0.68) e um alto ROC AUC (0.842), sendo uma alternativa viável.

<div>
  <img src="https://github.com/obaldin/Desafio_ONE_Alura_TelecomX_parte_II_Cassiano/blob/main/Imagens_geradas/gradient_boosting_performance_plot.png?raw=true">
</div>

*   O **Random Forest (Sem Normalização)** obteve a maior Acurácia (0.793) e uma boa Precision (0.64), mas seu Recall para Churn (0.50) é significativamente menor, o que a torna menos ideal para o objetivo de identificar a maioria dos churners.

<div>
  <img src="https://github.com/obaldin/Desafio_ONE_Alura_TelecomX_parte_II_Cassiano/blob/main/Imagens_geradas/random_forest_sem_normalizacao_performance_plot.png?raw=true">
</div>

*   Os modelos **SVC**, **KNN** e **Decision Tree** apresentaram desempenho inferior em Recall e/ou ROC AUC em comparação com a Regressão Logística e Gradient Boosting.

<div>
  <img src="https://github.com/obaldin/Desafio_ONE_Alura_TelecomX_parte_II_Cassiano/blob/main/Imagens_geradas/SVC_performance_plot.png?raw=true">
  <img src="https://github.com/obaldin/Desafio_ONE_Alura_TelecomX_parte_II_Cassiano/blob/main/Imagens_geradas/KNN_performance_plot.png?raw=true">
  <img src="https://github.com/obaldin/Desafio_ONE_Alura_TelecomX_parte_II_Cassiano/blob/main/Imagens_geradas/decision_tree_performance_plot.png?raw=true">
</div>

## :chess_pawn: Estratégias de Retenção Propostas

Com base nos fatores que mais influenciam a evasão e no desempenho do modelo de Regressão Logística Otimizada, propomos as seguintes estratégias de retenção:

*   **Foco em Clientes de Curto Prazo:** Clientes nos primeiros meses de contrato são mais propensos a evadir. Ações de engajamento e onboarding direcionadas a este grupo podem reduzir o churn inicial.
*   **Incentivo a Contratos de Longo Prazo:** Promover e incentivar a adesão a contratos de dois anos, destacando os benefícios e a estabilidade que oferecem.
*   **Avaliação do Serviço de Fibra Óptica:** Investigar as possíveis causas da maior evasão entre clientes de fibra óptica. Pode ser necessário melhorar a qualidade do serviço, rever preços ou oferecer suporte técnico mais eficiente para este segmento.
*   **Análise do Método de Pagamento (Cheque Eletrônico):** Entender por que clientes que usam cheque eletrônico evadem mais. Pode haver problemas no processo de pagamento ou insatisfação associada a este método. Oferecer alternativas de pagamento mais convenientes pode ser uma solução.
*   **Monitoramento de Clientes com Altas Cobranças Mensais:** Clientes com contas mais altas podem estar mais sensíveis a custos e buscar alternativas. Oferecer pacotes de descontos, benefícios exclusivos ou revisão de planos pode ser eficaz.
*   **Programas para Clientes Idosos:** Desenvolver programas de fidelidade, suporte técnico facilitado ou planos adaptados para clientes idosos, que demonstraram uma ligeira maior propensão à evasão.
*   **Revisão do Processo de Faturamento Sem Papel:** Investigar se há alguma insatisfação ou dificuldade para clientes que optam por faturamento sem papel que possa estar contribuindo para o churn.
*   **Fortalecer Serviços de Valor Agregado:** Destacar e promover os benefícios de serviços como segurança online, backup, proteção de dispositivos e suporte técnico, pois clientes que utilizam esses serviços tendem a ser mais leais.
*   **Utilização do Modelo Preditivo:** Implementar o modelo de Regressão Logística Otimizada para identificar proativamente os clientes com alta probabilidade de churn. A equipe de marketing e atendimento ao cliente pode então direcionar esforços de retenção para estes clientes, oferecendo ofertas personalizadas, contato proativo ou resolução de problemas.

## :rocket:  Conclusão

A análise preditiva de churn permitiu identificar os principais fatores que levam os clientes da TelecomX a cancelar seus serviços. O modelo de Regressão Logística Otimizada, com seu alto Recall para a classe Churn, é a ferramenta recomendada para identificar proativamente os clientes em risco. A implementação das estratégias de retenção propostas, baseadas nestas descobertas e na capacidade preditiva do modelo, tem o potencial de reduzir significativamente a taxa de evasão e aumentar a receita a longo prazo da TelecomX. É crucial que a empresa monitore continuamente o desempenho do modelo e adapte suas estratégias de retenção conforme necessário.

## :hammer_and_wrench: Tecnologias Utilizadas

Para este projeto, foram usados as seguintes tecnologias:

<div>
  <img src="https://img.shields.io/badge/Google%20Colab-F9AB00?style=for-the-badge&logo=googlecolab&logoColor=black">
  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white">
  <img src="https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white">
  <img src="https://img.shields.io/badge/Matplotlib-11557c?style=for-the-badge&logo=matplotlib&logoColor=white">
  <img src="https://img.shields.io/badge/Plotly_Express-3F4F75?style=for-the-badge&logo=plotly&logoColor=white">
  <img src="https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white">
  <img src="https://img.shields.io/badge/Seaborn-406180?style=for-the-badge&logo=seaborn&logoColor=white">
  <img src="https://img.shields.io/badge/imbalanced--learn-FFD43B?style=for-the-badge&logo=imbalanced-learn&logoColor=black">
  <img src="https://img.shields.io/badge/Scikit--learn-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=black">
  <img src="https://img.shields.io/badge/Statsmodels-283085?style=for-the-badge&logo=statsmodels&logoColor=white">
</div>

## :computer: Como Usar

1. Abra o arquivo **"Challenge_Alura_TelecomX_analise_evasao_de_cliente_parte_2_Cassiano.ipynb"** e clique em **Download Raw File** e salve em seu computador;

2. Após baixado, faça o upload do arquivo para o **Google Drive**.

3. No Google Drive, localize o arquivo, e dê um duplo clique para abrir no **Google Colaboratory** ou clique com o botão direito do mouse, **Abrir com**
no **Google Colaboratory**.

    **Observação:** Se "Google Colaboratory" não aparecer diretamente na lista, pode ser que você precise conectá-lo. Clique em **Conectar mais app** (ou "Connect more apps") na parte inferior do submenu "Abrir com". Na janela que se abre, pesquise por **Colaboratory** e clique em **Conectar**. Depois disso, ele deverá aparecer como uma opção.

4. Encontre o item **3.1 Extração e Exploração dos Dados** e no campo do código ```#carremento e leitura do dataset``` altere o link da variável ```url``` para ``'https://github.com/obaldin/Desafio_ONE_Alura_TelecomX_parte_II_Cassiano/blob/df4ecaf039f7d9a2b253d4e09c48323c6ef6053c/2025-08-16_dados_tratados.csv'``. O código final deverá ficar assim:

```
#carremento e leitura do dataset
url = 'https://github.com/obaldin/Desafio_ONE_Alura_TelecomX_parte_II_Cassiano/blob/df4ecaf039f7d9a2b253d4e09c48323c6ef6053c/2025-08-16_dados_tratados.csv'
df = pd.read_csv(url)
df
```

6. Em seguida, no parte do menu, clique na guia **Ambiente de Execução** e **Executar tudo** (ou utilize o atalho *CTRL + F9*).

## :mailbox: Contato

Quaisquer dúvidas ou sugestões, não hesite em entrar em contato:

<div>
<a href = "mailto:baldin.co@gmail.com"><img loading="lazy" src="https://img.shields.io/badge/Gmail-D14836?style=for-the-badge&logo=gmail&logoColor=white" target="_blank"></a>
<a href="https://www.linkedin.com/in/cassiano-baldin/" target="_blank"><img loading="lazy" src="https://img.shields.io/badge/-LinkedIn-%230077B5?style=for-the-badge&logo=linkedin&logoColor=white" target="_blank"></a>   
</div>
