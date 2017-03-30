**Construção de um Modelo preditivo Árvore de Decisão utilizando a plataforma analítica KNIME**

Neste trabalho sobre Machine Learning (aprendizado de máquina supervisionado) farei uma demonstração usando a plataforma analítica KNIME, utilizando alguns dos muitos recursos dessa plataforma.
Estarei criando um fluxo de trabalho KNIME que utiliza Decision Tree –  Árvore de Decisão -  para treinar um modelo no conjunto de dados chamado **combinado** ( Este arquivo será detalhado mais abaixo).<br/>                                
**Objetivo**<br/> 
 O objetivo do trabalho é construir um modelo preditivo utilizando Árvore de Decisão com método de poda classificando os compradores (jogadores) em **("HighRollers")** ou **("PennyPinchers")**. Os compradores que adquirem itens acima de 5 dólares são considerados como **("HighRollers")** e os compradores que compram itens até 5 dólares são considerados como **("PennyPinchers")**.<br/>
 A classificação correta dos compradores possibilitará a empresa proprietária do game que tome decisões em relação ao panorama apresentado e assim possa , de forma segura, tomar decisões que busquem melhorar o faturamento em relação aos (2) dois grupos de compradores, baseadas nas recomendações feitas pelos responsáveis pela área de inteligência de dados.<br/>

**Rápida descrição do Game**.<br/>
Cada usuário é membro de no máximo uma equipe. Quando um novo usuário começa a jogar o jogo, ela está em uma equipe sozinho para o primeiro nível, ou seja, no primeiro nível o usuário sempre será um jogador solitário e estará em uma equipe própria, e pode se juntar , quando convidado,  a outra  equipe em níveis subseqüentes ou permanecer em sua equipe indefinidamente.
Quando um usuário está em uma equipe jogando, ele tem uma única sessão de usuário que começa quando ele começa a jogar e termina quando ele para de jogar.<br/>
Em ambos os casos, sempre que um usuário esta em uma equipe, é gerado uma linha na tabela teamassignment. 
Sempre que o usuário está em uma equipe (jogando ou não), ele pode subir de nível sempre que a equipe termina jogando naquele nível.  Quando a equipe muda de nível todos os jogadores mudam de nível.<br/>

**Geração dos dados**<br/>
A empresa fictícia é proprietária de um jogo on-line que é disponibilizado na Internet gratuitamente e pode ser jogado por qualquer jogador no planeta utilizando qualquer plataforma on line,
durante o jogo são exibidos anúncios de diversos itens que podem ou não ser comprados pelos jogadores. Da participação dos jogadores e a eventual compra ou não compra dos itens  são gerados dados que são gravados em tabela em banco de dados relacional,
Essas tabelas foram convertidas em arquivos para o Excel com extensão.csv para  uso nesse trabalho.<br/>

Para melhor entendimento dos relacionamentos existentes entre esses dados , segue abaixo o DER [diagrama de entidades e relacionamentos](https://github.com/pmoniz7/Modelo-DecisionTree-KNIME-/blob/master/Modelo-DER.PNG) dessas tabelas.<br/>
      						
Vale lembrar que as principais tabelas convertidas em  arquivos  abrangidas nesse trabalho são :
users.csv, team.csv , teamAssignments.csv , user-session.csv, buy-clicks.csv,                 ad-clicks.csv, game-clicks.csv<br/>

Segue abaixo a descrição detalhada de cada arquivo.<br/>



**users.csv** - Este arquivo contém uma linha para cada usuário do 
jogo.<br/>
**Obs :** Esta linha é criada quando o jogador começa pela primeira vez o jogo no nível um.<br/>
* Timestamp: quando o usuário primeiro Jogou o jogo<br/>
* UserId: o ID de usuário atribuído ao usuário<br/>
* Nick:     o nickname escolhido pelo  utilizador<br/>
* Twitter: o identificador do twitter do jogador<br/>
* Dob: a data de nascimento do usuário<br/>
* Country:* o código de duas letras do país onde o usuário vive<br/>

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

**buy-clicks.csv** - Uma linha é adicionada a este arquivo quando um jogador faz uma Compra no App do Game.<br/>
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

**game-clicks.cs **- Uma linha é adicionada a este arquivo cada vez que uma equipe
Termina um nível no jogo<br/>
* Timestamp: quando o evento ocorreu.<br/>
* EventId: um ID exclusivo para o evento<br/>
* TeamId: o id da equipe <br/>
* teamLevel: o nível iniciado ou concluído<br/>
* EventType: o tipo de evento, quer começar ou terminar<br/>
* 
Embora feita a descrição de todos os arquivos, somente utilizaremos para a análise três (03) arquivos para a classificação com modelo  Árvore de Decisão, são eles :<br/> 
* **ad-clicks.csv** 
* **game-clicks.csv**
* **buy-clicks.csv**



