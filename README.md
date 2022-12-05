# `<EExplorando modelo de GAN para gerar imagens de rostros emocionais>`
# `<Exploring GAN model to generate images of emotional faces>`

## Apresentação

O presente projeto foi originado no contexto das atividades da disciplina de pós-graduação *IA930 - Computação Afetiva*, 
oferecida no segundo semestre de 2022, na Unicamp, sob supervisão da Profa. Dra. Paula Dornhofer Paro Costa, do Departamento de Engenharia de Computação e Automação (DCA) da Faculdade de Engenharia Elétrica e de Computação (FEEC).

> Incluir nome RA e foco de especialização de cada membro do grupo. Os grupos devem ter no máximo três integrantes.
> |Nome  | RA | Especialização|
> |--|--|--|
> | Renan Yamaguti  | 262731  | Eng. em Computação|

## Descrição Resumida do Projeto

Modelos generativos como GAN (Generative Adversial NetworkS), VAE, baseados em fluxo mostraram grande sucesso na geração de amostras de alta qualidade, mas possuem algumas limitações próprias. 
Os modelos GAN e VAE são conhecidos pelo treinamento potencialmente instável e menor diversidade na geração devido à sua natureza adversarial.
Neste projeto, vamos explorar um modelo de GAN com o objetivo de gerar imagens faciais que expressam uma emoção afim de exemplificar as dificuldades e instabilidades criados pelos modelso de GAN. Para isso, foi utilizada uma base de dados que contêm imagens faciais que foram etiquetadas com uma emoção específica. Em conclusão, o projeto estará focado na geração condicional de rostos humanos emocionais usando uma GAN. 


## Metodologia Proposta

A metodologia do projeto será explicada nas seguintes seções.

### Bases de dados

Visando gerar rostos de pessoas que expressam alguma emoção, precisamos de bases de dados etiquetadas usando modelos de emoções. Como primeira perspectiva do projeto, vamos utilizar a bases de dados: FER2013 Dataset [1], que está definida abaixo:

#### FER2013 Dataset 

Esta base de dados tem sido amplamente utilizada para reconhecimento de emoções faciais, e está disponível publicamente em Kaggle. FER2013 contem 35887 imagens normalizadas a 48x48 pixeis em escala de cinza, que expressam 7 emoções: raiva, nojo, medo, felicidade, tristeza, surpresa, neutro. 

Enlace do repositório: https://www.kaggle.com/c/challenges-in-representation-learning-facial-expression-recognition-challenge/data

<p align="center">
	<img src="https://github.com/kgrosero/ia930/blob/main/2022.2/emo-diffusion/figures/fer2013.png" align="middle" width="700">
	  <figcaption>
  	Figura 1: Amostras da base de dados FER2013.
  	</figcaption>
</p>


### Modelos GAN


Os modelos generativos são uma classe de métodos de aprendizado de máquina que aprendem uma representação dos dados em que são treinados e modelam os próprios dados. Eles geralmente são baseados em redes neurais profundas.  Os modelos generativos permitem sintetizar novos dados que são diferentes dos dados reais, mas ainda parecem tão realistas. É assim que se treinarmos um modelo generativo em imagens de carros, a modelo seria capaz de gerar imagens de novos carros com aparências diferentes.


<p align="center">
	<img src="https://github.com/kgrosero/ia930/blob/main/2022.2/emo-diffusion/figures/diff2.png" align="middle" width="700">
	  <figcaption>
  	Figura 2: Modelos GAN
  	</figcaption>
</p>

Modelos baseados em redes adversariais (GANs) tem-se tornado a classe mais popular de modelos generativos. Esses modelos são constituídos por dois componentes: um gerador e um discriminador, que disputam um jogo de "min-max" na qual o gerador gera uma imagem que tenta se passar por uma imagem real para o discriminador. Os modelos de GAN tem ampla aplicação na geraçãod de imagem e fala.

Um modelo de GAN é conhecido como DC-GAN[2], ou Deep Convolutional GAN, tem sido utilizada na geração de imagens com características similares, esse tipo de GAN tem algumas características especiais na sua concepção: a utilização da normalização de batches em ambos gerador e discriminador; substituição de todas as camadas de pooling por camadas convolucionais; remoção de camadas totalmente conectadas; utilização da função de ativação ReLU em todas as camadas (excerto camdas que tem como ativação a tangente hiberbólica) e por fim a utilização de LeakyReLU  em todas as camadas do discriminador.
O gerador da DCGAN criada pode visto pela figura abaixo:

<p align="center">
	<img src="https://github.com/kgrosero/ia930/blob/main/2022.2/emo-diffusion/figures/diff2.png" align="middle" width="700">
	  <figcaption>
  	Figura 3: Discriminador da DCGAN
  	</figcaption>
</p>


### Ferramentas 

Para este projeto será utilizado Google Colab Pro. O código base dos modelos de difusão foi desenvolvido em PyTorch e Keras e está disponível neste repositório: https://github.com/eriklindernoren/PyTorch-GAN/blob/master/implementations/dcgan/

### Resultados
Implementação do modelo de DCGAN com sentimentos ('neutral', 'fear', 'angry', 'surprise', 'disgust') e o processo de geração através das épocas pode ser visto pelo GIF abaixo:



<p align="center">
	<img src="https://github.com/kgrosero/ia930/blob/main/2022.2/emo-diffusion/figures/results.png" align="middle" width="700">
	  <figcaption>
  	Figura 3: GIF da geração de rostos com emoções
  	</figcaption>
</p>


### Avaliação


Segundo [3], uma maneira de avaliar as imagens geradas é através métrica IS(Insception Score) que mede o quão diverso e fiéis à base de treinamento as imagens são, o IS pode ser calculado pela equação abaixo:

<p align="center">
	<img src="https://github.com/kgrosero/ia930/blob/main/2022.2/emo-diffusion/figures/results.png" align="middle" width="700">
	  <figcaption>
  	Figura 5: Interpretation score
  	</figcaption>
</p>

Foi calculado o IS para diferentes números de épocas e assim avaliar o desempenho da geração de imagens humanas com emoções.
|  Epochs | IS  |
|---|---|
| 25 | 0.15283 | 
| 50 | 0.15732 |
| 75 | 0.3594 |
| 100 | 0.3791 |

## Conclusões

Apesar da criação de Através da observação desse projeto pode-se notar as maiores desvantagens das GANs na geração de imagens e entender o motivo pelos os quais elas estão sendo abandonadas pelas publicações mais recentes em favor de algoritmos como os de difussão: GANs não são algoritmos que convergem, são altamente voláteis e tendem a colapsar com poucos dados e com poucas etapas de treinamento. Também são muito afetadas por alterações em parâmetros.

Portanto, apesar das popularidades das GANs esse trabalho veio também explorar e constatar o motivo pelos os quais o estado da arte tem se favorecido de modelos diferentes, como os de difusão e os baseados em fluxo. 

## Projetos Futuros
Baseando-se nesses resultados e nas conclusões obtidas, pretende-se aplicar uma avialiação subjetiva que não houve tempo para ser aplicada durante esse projeto e a implementação de algoritmos de difussão para a comparação com o que a GAN gerou.

## Cronograma

A continuação detalhamos as atividades a serem desenvolvidas nas próximas semanas até o dia da apresentação final do projeto.

<b> 24/10 </b>: Processamento de base de dados 1

<b> 31/10 </b>: Processamento de base de dados 2

<b> 7/11 </b>: Escolha e estudo das GAN

<b> 14/11, 21/11 </b>: Implementação da GAN

<b> 28/11 </b>: Avaliação dos resultados e escrita do relatório do projeto.

<b> 5/12 </b>: Apresentação do projeto




## Referências Bibliográficas

[1] Khaireddin, Yousif, and Zhuofa Chen. "Facial emotion recognition: State of the art performance on FER2013." arXiv preprint arXiv:2105.03588 (2021).

[2] Radford, Alec, Luke Metz, and Soumith Chintala. "Unsupervised representation learning with deep convolutional generative adversarial networks." arXiv preprint arXiv:1511.06434 (2015).

[3] Obukhov, Artem, and Mikhail Krasnyanskiy. "Quality assessment method for GAN based on modified metrics inception score and Fréchet inception distance." Proceedings of the Computational Methods in Systems and Software. Springer, Cham, 2020.

Odena, Augustus, et al. "Is generator conditioning causally related to GAN performance?." International conference on machine learning. PMLR, 2018.
Voynov, Andrey, and Artem Babenko. "Unsupervised discovery of interpretable directions in the gan latent space." International conference on machine learning. PMLR, 2020.
Qiao, Fengchun, et al. "Geometry-contrastive gan for facial expression transfer." arXiv preprint arXiv:1802.01822 (2018).
Luo, Yun, Li-Zhen Zhu, and Bao-Liang Lu. "A GAN-based data augmentation method for multimodal emotion recognition." International Symposium on Neural Networks. Springer, Cham, 2019.
