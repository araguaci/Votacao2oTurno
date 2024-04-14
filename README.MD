# Análise das eleições para presidente do 2o turno

Datasets inclusos

Maiores informações na thread abaixo:

https://twitter.com/leonardodias/status/1592001220817870851

Caso queira rodar o notebook, pode rodá-lo em sua máquina. Para isso basta instalar o Anaconda e rodá-lo. 

https://www.anaconda.com

Também é possível instalar usando pip. Veja o site oficial do Jupyter. Tanto faz rodar o Jupyter, mais leve, ou o JupyterLab, que tem mais extensões e módulos.

https://jupyter.org/

Caso queira rodar em cloud, recomendo o Google Colab, que é bem simples, e permite que você armazene os datasets no Google Drive e acesse por lá.

https://colab.research.google.com

Também é possível criar um cluster EMR e usar o EMR Studio. Porém a criação de um cluster demanda tempo e tem custos mais altos. Mas é possível criar um para agregar ainda mais dados e aumentar o tamanho do dataset.

https://aws.amazon.com/pt/emr/features/studio/

Também será necessário instalar o pyspark, seaborn, pandas e mais alguns outros. Dependendo do pacote que você instala, alguns já vêm instalados. Mas geralmente pode ser necessário instalar o pyspark. Você pode usar os comandos abaixo:

pip install pyspark seaborn pandas
conda install pyspark

No caso do conda, pode ser necessário utilizar os pacots do conda_forge. Basta visitar a página desses projetos para verificar o comando utilizado.

Existem diversos serviços de cloud para rodar notebooks além do Google Colab e EMR Studio. Veja o artigo abaixo para mais opções:

https://www.dataschool.io/cloud-services-for-jupyter-notebook/


## Subscribe

[@leonardodias](//twitter.com/leonardodias)

[Nov 14, 2022](https://twitter.com/leonardodias/status/1592001220817870851 "Read on X") • 24 tweets • 11 min read • [Read on X](https://twitter.com/leonardodias/status/1592001220817870851)

 

Gostam de dados, números, manipulação de dados e programação? Que tal uma thread educativa sobre o assunto usando os dados oficiais do sufrágio?  
  
Sigam o fio 🧵🧵🧵

Nessa thread vamos falar de:  
  
1) Engenharia de dados  
2) Enriquecimento de dados  
3) Qualidade de dados  
4) Ciência de dados  
5) Análise exploratória  
  
Tudo isso usando Spark SQL, Spark ML, Pandas, Seaborn e Python. Bora?

Primeiro os downloads: todos os datasets foram obtidos do portal [dadosabertos.tse.jus.br](http://dadosabertos.tse.jus.br). Basta buscar por 2002 e todos os datasets estarão lá. Depois é só descomprimir e utilizar à vontade. Vamos lá!

Usei um notebook Jupyter para a análise. Primeiro importamos as bibliotecas. Você pode precisar instalar algumas delas. Sugiro conda ou pip. [![Image](https://pbs.twimg.com/media/Fhfl40bXkAciXm0.jpg)](https://pbs.twimg.com/media/Fhfl40bXkAciXm0.jpg)

Leitura dos datasets e criação dos dataframes: [![Image](https://pbs.twimg.com/media/FhfmAxtXgAEns_m.jpg)](https://pbs.twimg.com/media/FhfmAxtXgAEns_m.jpg)

Vamos examinar os schemas dos datasets. É possível ver alguns problemas, como campos data como string, por exemplo. [![Image](https://pbs.twimg.com/media/FhfmQJ6X0AYggJE.png)](https://pbs.twimg.com/media/FhfmQJ6X0AYggJE.png)[![Image](https://pbs.twimg.com/media/FhfmUZFWAAIgtzP.jpg)](https://pbs.twimg.com/media/FhfmUZFWAAIgtzP.jpg)[![Image](https://pbs.twimg.com/media/FhfmXd2XwAI3s_A.jpg)](https://pbs.twimg.com/media/FhfmXd2XwAI3s_A.jpg)

Na engenharia de dados nos preocupamos com qualidade dos dados, conversão de campos para timestamp e criação das chaves urna\_id e zona\_id que serão utilizadas para fazer joins e tornar os dados verticalizados originais em horizontalizados para análises diversas. [![Image](https://pbs.twimg.com/media/Fhfmlg7XwAYeGeW.png)](https://pbs.twimg.com/media/Fhfmlg7XwAYeGeW.png)[![Image](https://pbs.twimg.com/media/FhfmrWLWYAAwTiu.jpg)](https://pbs.twimg.com/media/FhfmrWLWYAAwTiu.jpg)[![Image](https://pbs.twimg.com/media/Fhfmwq7XwAY3IZk.jpg)](https://pbs.twimg.com/media/Fhfmwq7XwAY3IZk.jpg)[![Image](https://pbs.twimg.com/media/Fhfm172WQAEAK9M.jpg)](https://pbs.twimg.com/media/Fhfm172WQAEAK9M.jpg)

Para juntar todos os dataframes e agrupá-los por urna\_id, foi necessário um grande comando SQL cheio de funções matemáticas. Com isso criamos diversos campos úteis para a análise de dados. [![Image](/images/1px.png)](https://pbs.twimg.com/media/FhfnKL2XEAIUnGz.jpg)

Com o objetivo de juntar o máximo de dados possíveis, resolvi juntar também o dataset de mesários. A informação mais relevante que encontrei é se eram voluntários ou não e o total de mesários de cada zona. O número é por zona, não por seção. Daí a necessidade do zona\_id. [![Image](/images/1px.png)](https://pbs.twimg.com/media/FhfnfF_X0AIn_VQ.jpg)[![Image](/images/1px.png)](https://pbs.twimg.com/media/FhfnlaVXoAEM7r0.jpg)

Com isso criamos o dataframe vm, que agora contém dados de 3 datasets diferentes. [![Image](/images/1px.png)](https://pbs.twimg.com/media/FhfnuuDWYAAVVBq.jpg)[![Image](/images/1px.png)](https://pbs.twimg.com/media/FhfnzO5WYAEWb9U.jpg)

Como há muitas variáveis e é difícil para um ser humano ficar analisando variável por variável, resolvi fazer uma análise usando machine learning. Escolhi uma técnica de machine learning não supervisionado de agrupamento (clustering). Usei k=4 após testar com outros números. [![Image](/images/1px.png)](https://pbs.twimg.com/media/FhfoWF_X0AAf7JI.png)[![Image](/images/1px.png)](https://pbs.twimg.com/media/FhfobmhWIAAiaXT.jpg)

Para tentar entender como o algoritmo dividiu os dados, fiz algumas análises com pivot usando o método crosstab no dataframe predictions. Dá para ver algumas coisas interessantes sobre como os dados ficaram separados. [![Image](/images/1px.png)](https://pbs.twimg.com/media/Fhfou6AWYAEVgSL.jpg)[![Image](/images/1px.png)](https://pbs.twimg.com/media/Fhfo-voXkAMaUxg.jpg)[![Image](/images/1px.png)](https://pbs.twimg.com/media/FhfpEGqXEAIR-Ur.jpg)

Fizemos então a análise gráfica. Aquele mesmo gráfico de bolinhas. Cada bolinha é uma seção. Os eixos são os mesmos. Só que agora estamos filtrando cada cluster descoberto pelo algoritmo de inteligência artificial utilizado. [![Image](/images/1px.png)](https://pbs.twimg.com/media/FhfpRnJX0AAPVpQ.png)

Como ficou o cluster 0, o mais intrigante por ter mais variabilidade. Vejam os gráficos.  
  
Nos eixos superior temos a concentração de frequência na barra, E acima e ao lado direito, temos a distribuição de frequência de cada um dos itens coloridos pelo gráfico. [![Image](/images/1px.png)](https://pbs.twimg.com/media/FhfpiFuXEAIHVrg.png)[![Image](/images/1px.png)](https://pbs.twimg.com/media/Fhfpt5tXgAEvHNe.png)[![Image](/images/1px.png)](https://pbs.twimg.com/media/FhfpwYXXgAAlsCD.png)[![Image](/images/1px.png)](https://pbs.twimg.com/media/FhfpzcFXgAMLdEw.png)

Último gráfico do cluster 0: [![Image](/images/1px.png)](https://pbs.twimg.com/media/Fhfp7o1XgAEzwfS.png)

Como ficou o cluster 1. Muito interessante o caráter bimodal da frequência das urnas que não são 2020, que foram todas agrupadas nesse grupo 1. [![Image](/images/1px.png)](https://pbs.twimg.com/media/FhfqDvGWAAAW-u0.png)[![Image](/images/1px.png)](https://pbs.twimg.com/media/FhfqHFaWIAAvpPz.png)[![Image](/images/1px.png)](https://pbs.twimg.com/media/FhfqPMqWAAAfKfD.png)

Grupos 2 e 3 Vejam como ficou. Grupo 2, primeiros 2 gráficos, grupo 3, 2 últimos. [![Image](/images/1px.png)](https://pbs.twimg.com/media/FhfqWxKXwAEcDes.png)[![Image](/images/1px.png)](https://pbs.twimg.com/media/FhfqZunX0AESvjs.png)[![Image](/images/1px.png)](https://pbs.twimg.com/media/Fhfqd9MXEAEusqk.png)[![Image](/images/1px.png)](https://pbs.twimg.com/media/Fhfqkk_X0AAgnlj.png)

Por último, fiz algumas análises por algumas variáveis considerando a diferença entre eles. Vejam que interessante: [![Image](/images/1px.png)](https://pbs.twimg.com/media/FhfqvwkXkAIpEka.jpg)[![Image](/images/1px.png)](https://pbs.twimg.com/media/Fhfq2zlWYAEgznz.jpg)[![Image](/images/1px.png)](https://pbs.twimg.com/media/Fhfq8LlWIAc-OL8.jpg)[![Image](/images/1px.png)](https://pbs.twimg.com/media/FhfrBd2XkAEUvVm.jpg)

Continuando: [![Image](/images/1px.png)](https://pbs.twimg.com/media/FhfrGNDXwAEF2QJ.jpg)

Outra análise que fiz foi procurar zonas onde havia mais de um tipo de urna. Encontrei algumas para verificar se há mesmo diferenças entre essas seções e se é significativa.  
  
Escolhi a cidade de Registro, em SP, por ter vários modelos de urna numa mesma zona. [![Image](/images/1px.png)](https://pbs.twimg.com/media/Fhfrq1TXkAEmkHJ.jpg)

O resultado é que há mesmo diferença de resultados entre as seções com determinados modelos. Mesmo sendo de uma mesma zona de uma mesma cidade. No entanto há diferenças entre o número de seções com cada modelo. Várias coisas podem explicar, mas é curioso de qualquer maneira. [![Image](/images/1px.png)](https://pbs.twimg.com/media/Fhfr-81XkAAmXcD.png)

Há muitas possibilidades de analisar esses dados e também de agregar mais dados, como geolocalização, dados de clima e outros fatores interessantes.  
  
Que outros dados você agregaria? E o que gostaria de saber desses dados? Essa thread agora vai ser complementada por você!

Espero, com essa thread educativa, inspirar muitos de vocês a trabalharem com dados, números, estatísticas, matemática, big data, machine learning, ciência e engenharia de dados.  
  
Vamos? Obrigado por ler até aqui e espero que tenha gostado!

Link para o repositório no GitHub:  
  
[github.com/leonardodiasda…](https://github.com/leonardodiasdatascientist/Votacao2oTurno)

[![](https://opengraph.githubassets.com/b5fcff31b859cbb7a8bb525f3320a5762058778baa7de606158cb4c6d6139342/leonardodiasdatascientist/Votacao2oTurno)](https://github.com/leonardodiasdatascientist/Votacao2oTurno)

[**GitHub - leonardodiasdatascientist/Votacao2oTurno** Contribute to leonardodiasdatascientist/Votacao2oTurno development by creating an account on GitHub.](https://github.com/leonardodiasdatascientist/Votacao2oTurno) [https://github.com/leonardodiasdatascientist/Votacao2oTurno](https://github.com/leonardodiasdatascientist/Votacao2oTurno)

