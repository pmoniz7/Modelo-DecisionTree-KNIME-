**Construção de um Modelo preditivo Árvore de Decisão utilizando a plataforma analítica KNIME**

Neste Projeto  sobre  Machine  Learning (aprendizado de máquina supervisionado) farei  uma demonstração usando a plataforma analítica KNIME, utilizando alguns dos muitos recursos dessa plataforma.
Estarei criando um fluxo de trabalho KNIME que utiliza Decision Tree –  Árvore de Decisão -  para treinar um modelo no conjunto de  dados chamado **combinado** ( Este arquivo será detalhado mais abaixo).<br/>                                
**Objetivo**<br/> 

 A empresa fictícia é proprietária de um jogo on-line que é disponibilizado na Internet gratuitamente e pode ser jogado por qualquer jogador no planeta utilizando qualquer plataforma on line, durante o jogo são exibidos anúncios de diversos itens que podem ou não ser comprados pelos jogadores. Da participação dos jogadores e a eventual compra ou não compra dos itens  são gerados dados que são gravados em tabela em banco de dados relacional,Essas tabelas foram convertidas em arquivos para o Excel com extensão.csv para  uso nesse Projeto.<br/>

 O objetivo do Projeto é construir um modelo preditivo utilizando Árvore de Decisão com método de poda (MDL) classificando os compradores (jogadores) em **("HighRollers")** ou **("PennyPinchers")**. Os compradores que adquirem itens acima de  5  dólares são considerados como **("HighRollers")** e os  compradores que  compram itens até 5 dólares são considerados como **("PennyPinchers")**.<br/>

 A classificação correta dos compradores possibilitará a empresa proprietária do game tomar decisões em relação ao panorama apresentado e assim possa , de forma segura, melhorar o faturamento em relação aos (2) dois grupos de compradores, baseadas nas recomendações feitas pelos responsáveis pela área de inteligência de dados.<br/>

**Rápida descrição do Game**.<br/>
Cada usuário é membro de no máximo uma equipe. Quando um novo usuário começa a jogar o jogo, ela está em uma equipe sozinho para o primeiro nível, ou seja, no primeiro nível o usuário sempre será um jogador solitário e estará em uma equipe própria, e pode se juntar , quando convidado,  a outra  equipe em níveis subseqüentes ou permanecer em sua equipe indefinidamente.
Quando um usuário está em uma equipe jogando, ele tem uma única sessão de usuário que começa quando ele começa a jogar e termina quando ele para de jogar.<br/>
Em ambos os casos, sempre que um usuário esta em uma equipe, é gerado uma linha na tabela team-assignment. 
Sempre que o usuário está em uma equipe (jogando ou não), ele pode subir de nível sempre que a equipe termina jogando naquele nível.  Quando a equipe muda de nível todos os jogadores mudam de nível.<br/>

**Geração dos dados**<br/>

Para melhor entendimento dos relacionamentos existentes entre esses dados , segue abaixo o DER [diagrama de entidades e relacionamentos](https://github.com/pmoniz7/Modelo-DecisionTree-KNIME-/blob/master/Modelo-DER.PNG) dessas tabelas.<br/>
      						
Vale lembrar que as principais tabelas convertidas em  arquivos  abrangidas nesse trabalho são :<br/>
* **users.csv**<br/>
* **team.csv**<br/>
* **team-assignments.csv**<br/>
* **user-session.csv**<br/>
* **buy-clicks.csv**<br/>
* **ad-clicks.csv**<br/>
* **game-clicks.csv**<br/>

Segue abaixo a descrição detalhada de cada arquivo:<br/>



**users.csv** - Este arquivo contém uma linha para cada usuário do 
jogo.<br/>
**Obs :** Esta linha é criada quando o jogador começa pela primeira vez o jogo no nível um.<br/>
* Timestamp: quando o usuário primeiro Jogou o jogo<br/>
* UserId: o ID de usuário atribuído ao usuário<br/>
* Nick:     o nickname escolhido pelo  utilizador<br/>
* Twitter: o identificador do twitter do jogador<br/>
* Dob: a data de nascimento do usuário<br/>
* Country: o código de duas letras do país onde o usuário vive<br/>

**team.csv** - Este arquivo contém uma linha para cada equipe quando termina o jogo.<br/>
* TeamId: o id da equipe<br/>
* Name: o nome da equipe<br/>
* TimeCreationTime: o Timestamp quando a equipe foi criada<br/>
* TeamEndTime: o timestamp quando o último membro deixou a equipe<br/>
* strength : uma medida de Força da equipe, correspondendo aproximadamente ao sucesso de uma equipe<br/>
* CurrentLevel: o nível atual da equipe<br/>

**user-session.csv** - Cada linha neste arquivo escreve uma sessão de usuário, que denota quando um usuário inicia e  para de jogar. Além disso, quando uma equipe vai para o próximo nível no jogo, a sessão é terminada para cada usuário no game e inicia uma nova.<br/>
* Timestamp: um timestamp denotando quando o evento ocorreu<br/>
* UserSessionId: um ID exclusivo para a sessão<br/>
* UserId: o ID do usuário atual<br/>
* TeamId: a equipe do usuário atual<br/>
* AssignmentId:  o Id de atribuição do usuário para a equipe<br/>
* SessionType: se o evento é o início ou o fim de uma sessão<br/>
* TeamLevel: o nível da equipe durante esta sessão<br/>
* PlatformType: o tipo de Plataforma do usuário durante esta sessão<br/>

**buy-clicks.csv** - Uma linha é adicionada a este arquivo quando um jogador faz uma Compra no App do game.<br/>
* Timestamp: quando a compra foi feito<br/>
* TxId: um id único (dentro de buyclicks. Log) para a compra<br/>
* UserSessionId: o id do usuário Sessão para o usuário que fez a compra<br/>
* Team: o id da equipe atual do Usuário que fez a compra<br/>
* UserId: o ID de usuário do usuário que fez a compra<br/>
* BuyId: o id do item comprado<br/>
* Price: o preço do item comprado<br/>

**ad-clicks.csv** - Uma linha é adicionada a este arquivo quando um jogador clica em um
Anúncio no game<br/>
* Timestamp: quando o clique ocorreu.<br/>
* TxId: um id exclusivo (dentro de adclicks.Log) para o clique<br/>
* UserSessionid: o id do usuário na Sessão para o usuário que fez a clique<br/>
* Teamid: o id da equipe atual do usuário que fez o clique<br/>
* Userid: o id do usuário quem fez o clique<br/>
* AdId: o ID do anúncio clicado<br/> 
* AdCategory: a categoria / tipo de anúncio clicado<br/> 

**game-clicks.cs**- Uma linha é adicionada a este arquivo cada vez que uma equipe
termina um nível no jogo<br/>
* Timestamp: quando o evento ocorreu.<br/>
* EventId: um ID exclusivo para o evento<br/>
* TeamId: o id da equipe <br/>
* teamLevel: o nível iniciado ou concluído<br/>
* EventType: o tipo de evento, quer começar ou terminar<br/>
 
Embora feita a descrição de todos os arquivos, somente utilizaremos para a análise três (03) arquivos para a classificação com modelo  Árvore de Decisão, são eles :<br/> 
* **ad-clicks.csv** 
* **game-clicks.csv**
* **buy-clicks.csv**<br/>

**FLUXO  PARA A CRIAÇÃO DO ARQUIVO COMBINADO (AGREGADO)**<br/>
O modelo em questão irá utilizar um arquivo  chamado **combinado.csv**, este arquivo será um agregado dos três (03) arquivos mencionados acima.
Inicialmente vale explicar como é gerado esse arquivo (combinado.csv) através do fluxo do KNIME.
Conforme a [figura](https://github.com/pmoniz7/Modelo-DecisionTree-KNIME-/blob/master/Fluxo%20Prepara%C3%A7%C3%A3o.PNG), o fluxo foi divido em quatro módulos  que foram nomeados como módulos **1, 2, 3 e 4.**<br/>

**Módulo 1.**<br/>
Neste módulo  o arquivo  **buy-clicks.csv** é lido e os valores de price(mean)  e buyId (count) são agrupados por userSessionId e userId.  Em seguida  é  gerado  um  arquivo  com o nome **AgregBuyClicks.table** que ficará disponível para leitura no **módulo 4.**<br/>

**Módulo 2.**<br/>
* **2.1** Neste módulo  o arquivo  **game-clicks.csv** é lido e os valores de clickId(count)  e isHit(sum)  são agrupados por  userId, userSessionId e teamLevel.  Em seguida  é  gerado  um  arquivo  com o nome **AgregGameClicks.table** que ficará disponível  para leitura no
**Módulo 3**.<br/>
 * **2.2** Neste módulo  o arquivo  **user-session.csv** é lido e é selecionada a plataforma utilizada pelo jogador em uma determinada sesão.  Em seguida  é  gerado  um  arquivo  com o nome **AgregUserSession.table** que ficará disponível para leitura no **módulo 3.**<br/>
 
**Módulo 3.**<br/>
Neste módulo  os arquivos  **AgregUserSession.table / AgregGameClicks.table** são lidos pelo **nó** **“Joiner”** e é feito um Join entre estes dois arquivos  como **KEY** userId, userSessionId e teamLevel .  Em seguida  é  gerado  um  arquivo  com o nome **JoinGameSession.table** que ficará disponibilizado para leitura no **módulo 4.**<br/>

**Módulo 4.**<br/>
Neste módulo  os arquivos  **AgregBuyClicks.table / JoinGameSession.table** são lidos pelo nó **“Joiner”** e é feito um Join dos  dois arquivos utilizando como **KEY**  userId, userSessionId.  Em seguida  é  gerado  um  arquivo  com o nome **combinado.csv** , com 4619 linhas,  e que será utilizado no próximo fluxo para gerar a Árvore de Decisão.<br/>

**OBS:**  A  sequência  para  execução  dos  módulos  pode  ser  **1,  2.1,  2.2,  3  e  4** ou                                               **2.1,  2.2,  1 ,  3 e  4**<br/>
 
Após a execução do fluxo acima , o arquivo **combinado.csv**  foi gerado no **módulo 4** e será usado no próximo [fluxo para a criação do modelo preditivo](https://github.com/pmoniz7/Modelo-DecisionTree-KNIME-/blob/master/Fluxo%20Decision%20Tree.PNG) , esse arquivo  tem a seguinte estrutura:<br/>

**combinado.csv**<br/> 
* userId: o ID de usuário<br/>
* userSessionId: o ID do usuário na  Sessão<br/> 
* teamLevel : o nível da equipe durante esta sessão<br/>
* platformType: plataforma usada pelo usuário<br/>
* count_gameclicks  : número total de game clicks por usuário  na sessão<br/>
* count_hits:  número total de game hits por usuário na sessão<br/>
* count_buyId: número total de compras por usuário na sessão<br/>
* avg_price: Preço médio de compra por usuário  para sessão<br/>

Neste fluxo utilizarei as operações de preparação,
limpeza e manipulação no conjunto de dados **combinado.csv** que foi gerado com 4619 linhas.<br/>

Procurei identificar cada **Nó** em seus rótulos de forma que seja dispensável entrar em detalhes sobre a função de cada **Nó** do fluxo , no entanto, vale observar que após  o processamento do **Nó** **“Row Filter”** as linhas com valores nulos na coluna **avg_price** são descartadas , já após o processamento do **Nó** **“Column Filter”**  as colunas Userid e UserSessionid  serão retiradas do arquivo , pois elas são desnecessárias  e somente ficarão as restantes e  necessárias para o treinamamento do modelo.<br/>

É importante ressaltar que após os **Nós**  **“Row Filter”**  e **“Column Filter”** serem processados ,  apenas 1411 linhas  seguiram para o **Nó** **"Partitioning"**.

Após a execução do fluxo,  já teremos  a correta classificação do conjunto de dados e as informações  produzidas pelo modelo  necessárias para fazer as devidas recomendações à empresa proprietária do game para tomada de decisão. 
O **Nó** **“Scorer”** nos dará as informações necessárias sobre o treinamento e o aproveitamento do modelo.<br/>
 
Segue a [Confusion Matrix – Matriz de Confusão](https://github.com/pmoniz7/Modelo-DecisionTree-KNIME-/blob/master/Confusion%20Matrix.PNG) e a [Accuracy estatistics- Estatísticas Acurácia](https://github.com/pmoniz7/Modelo-DecisionTree-KNIME-/blob/master/Statistics%20acur%C3%A1cia.PNG) geradas  pelo **Nó** **“Scorer”**. 

Por fim, segue a [Decision Tree - Àrvore de Decision](https://github.com/pmoniz7/Modelo-DecisionTree-KNIME-/blob/master/Decision%20Tree%20Grafic.PNG) e a [Decision Tree simple - Àrvore de Decision simples](https://github.com/pmoniz7/Modelo-DecisionTree-KNIME-/blob/master/Decision%20Tree%20Simple.PNG) geradas pelo **Nó** **"Decision Tree Predictor"** e algumas recomendações feitas pela área de inteligência nos dados à área de negócios da empresa.<br/>

Vale ressaltar que as recomendações foram feitas baseadas nos 2 tipos de compradores (**("HighRollers")** / **("PennyPinchers")** e no uso das respectivas plataformas utilizadas por eles. 

**Algumas recomendações propostas  para aumentar a receita da empresa.**

* Fazer  promoções oferecendo descontos especiais aos compradores (PennyPinchers) para comprar itens que custam em média mais de US $ 5.<br/>
 **- Justificativa -** Pode-se observar independente do tipo de plataforma utilizada pelos compradores (100 %), que maioria (59,2 %) são compradores classificados como **("PennyPinchers")**

* Fazer uma campanha que ofereça **iphone** a compradores **Penny Pinchers** com a intenção de aumentar as vendas de itens.<br/>
  **- Justificativa -** observa-se que grande volume de vendas dos compradores classificados como **("HighRollers")** se deu através dessa plataforma. Assim, pode ser interessante que se ofereça Iphones  a compradores classificados como **Penny Pinchers**.<br/>   


* Intensificar uma campanha para usuários da plataforma Windows.<br/>

  **- Justificativa -** Observa-se que o terceiro maior grupo de usuários está concentrado na plataforma Windows. Assim seria interessante concentrar uma campanha para atingir este grupo,
principalmente os compradores **("HighRollers")** que nesta plataforma representa somente (11,8 %) em relação aos compradores **Penny Pinchers** que representam (88,2 %) concentrados nessa plataforma.<br/>


* Deve haver alguma campanha especial para atrair usuários da plataforma Mac.

  **- Justificativa -** O grupo com menor proporção de usuários é o da plataforma Mac que representa somente (3,2 %) do total de todos os compradores. Nessa campanha especial seria interessante buscar atingir os dois grupos de compradores nessa plataforma.  






