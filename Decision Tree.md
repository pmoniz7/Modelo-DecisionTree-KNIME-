##Construção de um Modelo preditivo Árvore de Decisão utilizando a plataforma analítica KNIME##
Neste trabalho sobre Machine Learning (aprendizado de máquina supervisionado) farei uma demonstração usando a plataforma analítica KNIME, utilizando alguns dos muitos recursos dessa plataforma.
Neste trabalho estou criando um fluxo de trabalho KNIME que utiliza Decision Tree – Árvore de Decisão - para treinar um modelo no conjunto de dados chamado combinado     ( Este arquivo será detalhado mais abaixo).
                              **OBJETIVO**
O objetivo do trabalho é construir um modelo preditivo utilizando Árvore de Decisão com método de poda classificando os compradores (jogadores) em ("HighRollers") ou ("PennyPinchers"). Os compradores que adquirem itens acima de 5 dolares são considerados como ("HighRollers") e compradores que compram itens até 5 dolares são considerados como ("PennyPinchers"). A classificação correta dos compradores possibilitará a empresa proprietária do game que tome decisões em relação ao panorama apresentado e assim possa , de forma segura, tomar decisões que busquem melhorar o faturamento em relação aos (2) dois grupos de compradores, baseadas nas recomendações feitas pelos responsáveis pela área de inteligência de dados.
Rápida descrição do Game.
Cada usuário é membro de no máximo uma equipe. Quando um novo usuário começa a jogar o jogo, ela está em uma equipe sozinha para o primeiro nível, ou seja, no primeiro nível o usuário sempre será um jogador solitário e estará em uma equipe própria, e pode se juntar , quando convidado,  a outra  equipe em níveis subseqüentes ou permanecer em sua equipe indefinidamente.
Quando um usuário está em uma equipe jogando, eles têm uma única sessão de usuário que começa quando eles começam a jogar e termina quando eles param de jogar.
Em ambos os casos, sempre que um usuário estiver em uma equipe, eles terão um uma linha da tabela teamassignment. 
Sempre que o usuário está em uma equipe (jogando ou não), eles podem subir de nível sempre que a equipe termina jogando naquele nível.  Quando a equipe muda de nível todos os jogadores mudam de nível.
Geração dos dados
A empresa fictícia é proprietária de um jogo on-line que é disponibilizado na Internet gratuitamente e pode ser jogado por qualquer jogador em qualquer parte do mundo.
Durante o jogo são exibidos anúncios de diversos itens que podem ou não ser comprados pelos jogadores. Da participação dos jogadores e a eventual compra ou não compra dos itens  são gerados dados que são gravados em tabela em banco de dados relacional.
Essas tabelas foram convertidas em arquivos para o Excel com extensão .csv para  uso nesse trabalho.
Para melhor entendimento dos relacionamentos existentes entre esses dados , segue abaixo o DER [diagrama de entidades e relacionamentos](https://github.com/pmoniz7/Modelo-DecisionTree-KNIME-/blob/master/Modelo-DER.PNG) dessas tabelas.



      						**OBJETIVO**

O objetivo do trabalho é construir um modelo preditivo utilizando Árvore de Decisão com método de poda classificando os compradores (jogadores) em ("HighRollers") ou ("PennyPinchers"). Os compradores que adquirem itens acima de 5 dolares são considerados como ("HighRollers") e compradores que compram itens até 5 dolares são considerados como ("PennyPinchers"). A classificação correta dos compradores possibilitará a empresa proprietária do game que tome decisões em relação ao panorama apresentado e assim possa , de forma segura, tomar decisões que busquem melhorar o faturamento em relação aos (2) dois grupos de compradores, baseadas nas recomendações feitas pelos responsáveis pela área de inteligência de dados.

