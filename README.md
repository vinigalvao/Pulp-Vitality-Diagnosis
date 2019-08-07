# Inteligência artificial no auxílio do diagnóstico da vitalidade pulpar

No cotidiano da clínica odontológica, a informação da vitalidade da polpa
dental é de grande relevância para que o paciente venha a ter o direcionamento do
tratamento mais adequado. 

As técnicas de diagnóstico utilizadas atualmente para a
realização da análise da vitalidade do tecido pulpar nas clínicas odontológicas são
várias, desde exames clínicos como palpação, testes de percussão horizontal e
vertical a exames complementares como os de sensibilidade e vitalidade pulpar que,
embora eficientes, na sua maioria, apresentam considerados índices de erros e
desconforto ao paciente. 

Dessa maneira, esse projeto propõe um novo método de
diagnóstico utilizando a espectroscopia de refletância difusa das radiações na região
espectral do infravermelho próximo, avaliando se esta técnica permite discriminar
polpas dentais diagnosticadas como não-vital, ou seja, que apresentam morte
pulpar, das polpas que exibem o diagnóstico de polpas vitais com pulpite, além das
polpas com status de sadias

A figura abaixo apresenta o passo a passo da metodologia utilizada para realizar esse projeto.

![](https://github.com/viniciusgalvaoia/Pulp-Vitality-Diagnosis/blob/master/Imagens/passo%20a%20passo.PNG)

Os espectros foram coletados por duas dentistas durante seus mestrados em
engenharia biomédica. 

Parte dos espectros foram coletados dentro de clínicas
odontológicas durante a realização de um procedimento terapêutico de endodontia.
Após a abertura da câmara pulpar pela face oclusal, a ponta do cabo de fibras
ópticas é posicionado sobre a polpa dental do paciente, ainda durante o ato
cirúrgico, e então coletados os espectros das polpas necrosadas e com pulpite, onde
testes clínicos usuais como os de sensibilidade, além de exames radiográficos foram
realizados para a conclusão do diagnóstico pulpar. 

Por outro lado, a coleta dos
espectros das polpas sadias foi realizada através de um procedimento ex-vivo. O
elemento dental a ser explorado foi extraído do paciente em procedimento cirúrgico
e armazenado em recipiente de baixa temperatura para ser transferido até o
laboratório de pesquisa, onde a polpa foi extraída do dente com o uso de um motor
de baixa rotação e instrumental adequado, em seguida, coletados os espectros.
Esses dentes foram extraídosde pacientes que apresentavam má oclusão da classe
II completa, que causa uma relação inadequada entre a maxila e a mandíbula. O
tratamento ortodôntico indica a extração de dentes pré-molares para promover
espaço na arcádia dentária e utilização de aparelho para promover o avanço
mandibular. 

A imagem abaixo apresenta os espectros coletados que formam a base de dados.
As diferenças observadas entre os grupos podem ser explicadas como possíveis
consequências da degradação pulpar, que leva a polpa a um processo de
compressão física e a um estrangulamento do fluxo vascular.

![](https://github.com/viniciusgalvaoia/Pulp-Vitality-Diagnosis/blob/master/Imagens/espectros.PNG)

Com isso, o dataset é formado pela entrada de 504 atributos, onde cada atributo
corresponde a intensidade de um comprimento de onda, e a saída correspondendo
a classe da amostra, composta por 43 espectros, sendo 17 possuindo status de
pulpite, 13 de necrose, e 13 classificados como saudáveis.

## Pré-processamento

Nesta etapa, foram testadas algumas técnicas de balanceamento de
instâncias, redução de dimensionalidade, normalizações e seleção de atributos.

### Balanceamento de Instâncias

Uma das técnicas mais populares para aliviar o problema das classes
desbalanceadas baseia-se na modificação do conjunto de dados originais. Tais
técnicas são conhecidas como reamostragem de dados Estudos têm mostrado que
para vários algoritmos de classificação, um conjunto balanceado proporciona uma
melhoria significativa no desempenho das classificações comparadas com um
conjunto de dados desbalanceados.

Como trata-se de um conjunto de dados desbalanceado, foram testados duas
técnicas de balanceamento de instâncias, Resample e SMOTE. A primeira cria
novas instâncias da classe minoritária repetindo amostras já existentes, já a
segunda, também cria novas instâncias da classe minoritária, porém, realizando uma
interpolação entre duas amostras já existentes na base de dados, de forma que as
todas classes fiquem com a mesma quantidade de instâncias.

### Redução de Dimensionalidade

A fim de facilitar o processo de classificação, foram testadas duas técnicas de
redução de dimensionalidade:

- A análise dos componentes principais (PCA), que
realoca o eixo x e y de forma que fiquem posicionados na direção de maior variância
da sua base de dados. Além disso gera um sub-espaço vetorial computacional
manipulável, descartando-se as componentes principais que apresentam as
menores variâncias de modo a minimizar o erro de reconstrução.

- Em contraste da PCA, que não leva em conta nenhuma diferença nas
classes, a Análise de Discriminantes Lineares (LDA) tenta encontrar uma
transformação linear através da maximização da distância inter-classes e da
minimização da distância intra-classes, ou seja, a determinação de uma base
vetorial que maximize a razão entre o determinante da matriz inter-classe e
a matriz intra-classe.

### Normalização de Dados

A concentração de água no sangue, bastante presente
em polpas inflamadas (pulpite), é de 60%, o que significa dizer para a análise do
sangue a água apresenta uma importante contribuição na absorção e
consequentemente na reflexão do tecido pulpar inflamado. 

A água apresenta um
forte poder de absorção na região espectral antes de 300nm e após 1.000nm e uma
absorção relativamente baixa entre 300nm e 1.000nm.

Com isso, foi realizada a normalização dos espectros pela intensidade do pico da banda
em torno de 1300 nm, pois a literatura destaca a menor absorção das radiações
infravermelhas tanto por parte da água quanto por parte dos tecidos moles.  Além
disso, essa é a região menos afetada no processo de degradação pulpar, onde
nenhuma variação espectral fica claramente evidenciada, exceto pelas diferentes intensidades de reflexão.

### Seleção de Atributos

O método de seleção de atributos testado foi o Univariate Statistcs, que
determina se há uma relação estatística significante entre cada atributo e saída,
baseado no test F, que estima o grau de dependência linear entre duas variáveis
aleatórias.

# Testes dos classificadores

Foram testados as técnicas test-split (10% das amostras
para testes) e cross-validation (5-folds), onde a base de dados é dividida em 5
subconjuntos e são utilizados 4 desses subconjuntos para o treinamento e o último
para teste, é feito isso para cada um dos subconjuntos.

Foram testados os modelos: 

- K-Nearest Neighbor (KNN),  variando o parâmetro do número de vizinhos k, de 1 a 10;
- Decision Tree,  variando o parâmetro da profundidade da árvore de 2 a 10;
- Random Forest,  variando o número de árvores de decisão entre 20, 50, 80 e 100;
- Support Vector Machine (SVM), variando o parâmetro do kernel entre rbf, linear, grau 2, 3, 4 e 5.  Por se tratar de um
problema multiclasse, foi utilizado o esquema um contra todos, onde é ajustado um classificador por classe, de forma que para cada classificador, a classe é ajustada contra todas as outras classes;
- Naive Bayes (GauissanNB);
- Rede neural artificial Perceptron, variando a quantidade de camadas, o número de neurônios por camada e a quantidade máxima de iterações, utilizando uma taxa de aprendizagem de 0,01 e a função de ativação sigmoidal. 

# Avaliação do Desempenho dos Classificadores

Foram utilizadas a acurácia, a estatística kappa e a matriz de confusão para avaliar o desempenho dos classificadores e comparar as melhores técnicas de pré-processamento para essa base de dados.

# Análise e Interpretação dos Resultados

Os resultados descritos refletem o maior desempenho obtido
resultantes de testes efetuados por meio de variação dos parâmetros de
configuração de cada classificador. 

A imagem abaixo apresenta o resultado da PCA sobre esse conjunto de dados.

![](https://github.com/viniciusgalvaoia/Pulp-Vitality-Diagnosis/blob/master/Imagens/PCA.png)

A PCA não produziu os resultados esperados, sendo pouco eficiente para
separar as amostras, consequentemente, resultando em piores resultados em
relação ao desempenho dos classificadores.

Por outro lado, a LDA mostrou-se bastante eficiente em separar as amostras
desse conjunto de dados, como mostra a figura abaixo, e consequentemente, foi
a técnica que resultou nos melhores resultados em relação ao desempenho dos
classificadores.

![](https://github.com/viniciusgalvaoia/Pulp-Vitality-Diagnosis/blob/master/Imagens/LDA.png)

- K-Nearest Neighbor (KNN)

Dentre os modelos testados, o classificador KNN que obteve melhor
desempenho foi construído utilizando a técnica de balanceamento SMOTE e ainda a
técnica de seleção de atributos Univariate Statistics, alcançando uma acurácia de
aproximadamente 96% para K igual a 2, 5, 6, 8 e 9.

As figuras abaixo apresentam as acurácias do modelo em função do número de vizinhos, K e a matriz de confusão do melhor resultado.

![](https://github.com/viniciusgalvaoia/Pulp-Vitality-Diagnosis/blob/master/Imagens/KNN1.png)

![](https://github.com/viniciusgalvaoia/Pulp-Vitality-Diagnosis/blob/master/Imagens/KNN.JPG)

- Decision Tree

Dentre os modelos testados, o classificador Decision Tree que obteve melhor
desempenho foi construído utilizando técnica de balanceamento Resample
alcançando uma acurácia de aproximadamente 92% para uma profundidade de 2 e
3.

As figuras abaixo apresenta as acurácias para cada modelo testado e a matriz de confusão do melhor resultado.

![](https://github.com/viniciusgalvaoia/Pulp-Vitality-Diagnosis/blob/master/Imagens/%C3%81rvores%20de%20decis%C3%A3o.png)

![](https://github.com/viniciusgalvaoia/Pulp-Vitality-Diagnosis/blob/master/Imagens/Tree.JPG)

A próxima imagem apresenta a estrutura da árvore de decisão utilizada para classificar as
amostras. Os comprimentos de onda de maior relevância foram 960.5 nm, 1382.3
nm, 1412 nm e 1518.9 nm.

![](https://github.com/viniciusgalvaoia/Pulp-Vitality-Diagnosis/blob/master/Imagens/dentetree.png)

A figura abaixo apresenta os atributos mais importantes utilizados pela árvore de
decisão e seu índice de relevância, sendo representado em uma escala de 0 à 1. 

![](https://github.com/viniciusgalvaoia/Pulp-Vitality-Diagnosis/blob/master/Imagens/feature_importances.png)

- Random Forest

Dentre os modelos testados, o classificador Decision Tree que obteve melhor
desempenho foi construído utilizando a técnica de balanceamento SMOTE e ainda a
técnica de seleção de atributos Univariate Statistics, alcançando uma acurácia de
aproximadamente 96% com 100 árvores.

As figuras abaixo apresenta as acurácias para cada modelo testado e a matriz de confusão do melhor resultado.

![](https://github.com/viniciusgalvaoia/Pulp-Vitality-Diagnosis/blob/master/Imagens/RF_nova.png)

![](https://github.com/viniciusgalvaoia/Pulp-Vitality-Diagnosis/blob/master/Imagens/Matriz_RF_certa.png)

- Support Vector Machine (SVM)

Dentre os modelos testados, o classificador SVC que obteve melhor
desempenho foi construído utilizando o kernel rbf, a técnica de balanceamento
SMOTE e ainda a técnica de seleção de atributos Univariate Statistics, alcançando
uma acurácia de aproximadamente 98%.

As figuras abaixo apresenta as acurácias para cada modelo testado e a matriz de confusão do melhor resultado.

![](https://github.com/viniciusgalvaoia/Pulp-Vitality-Diagnosis/blob/master/Imagens/SVM_nova.png)

![](https://github.com/viniciusgalvaoia/Pulp-Vitality-Diagnosis/blob/master/Imagens/NB.JPG)

- Naive Bayes

Dentre os modelos testados, o classificador Naive Bayes que obteve melhor
desempenho foi construído utilizando a técnica de balanceamento SMOTE e ainda a
técnica de seleção de atributos Univariate Statistics, alcançando uma acurácia de
aproximadamente 98%.

As figuras abaixo apresenta as acurácias para cada modelo testado e a matriz de confusão do melhor resultado.

![](https://github.com/viniciusgalvaoia/Pulp-Vitality-Diagnosis/blob/master/Imagens/NB_nova.png)

![](https://github.com/viniciusgalvaoia/Pulp-Vitality-Diagnosis/blob/master/Imagens/NB.JPG)

- MLPClassifier

Dentre os modelos testados, o classificador MLPClassifier que obteve melhor
desempenho foi construído utilizando a técnica de balanceamento SMOTE e ainda a
técnica de seleção de atributos Univariate Statistics, alcançando uma acurácia de
aproximadamente 98%. 

As figuras abaixo apresenta as acurácias para cada modelo testado e a matriz de confusão do melhor resultado.

![](https://github.com/viniciusgalvaoia/Pulp-Vitality-Diagnosis/blob/master/Imagens/MLP_nova.png)

![](https://github.com/viniciusgalvaoia/Pulp-Vitality-Diagnosis/blob/master/Imagens/mlp.JPG)

# Melhores resultados

Na sua maioria, os classificadores mostraram um ótimo desempenho
utilizando a técnica de redimensionamento LDA, a técnica de balanceamento de
instâncias SMOTE e a a técnica de seleção de atributos Univariate Statistics
configurado para selecionar 30% dos atributos de entrada, ou seja 30% dos
comprimentos de onda.

![](https://github.com/viniciusgalvaoia/Pulp-Vitality-Diagnosis/blob/master/Imagens/tabela.PNG)

# Conclusão

A metodologia aplicada para realização da coleta dos dados e discriminação
do status pulpar através das técnicas e classificadores testados, com o intuito de
alcançar os objetivos do projeto, foi bem sucedida, tornando possível a aquisição e
discriminação dos espectros no NIR em polpas dentais humanas. 
