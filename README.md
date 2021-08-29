# Auditoria no cluster Hadoop.

[![NPM](https://img.shields.io/npm/l/react)](https://github.com/pand-eX/Auditoria_cluster_Hadoop/blob/main/LICENSE) 

# About the project

O objetivo desse projeto é mostrar a configuração de auditoria no cluster Hadoop seus conceitos e vamos discutir e compreender um pouco sobre outras ferramenta de segurança como Apache Knox, Apache Range e Apache Sentry suas diferenças e qualidades.



## Porque? 

Porque esse é um ponto importante nós dados que muitas empresas não implementa auditoria dos dados e suas complexidades das ferramenta do ecossistema Hadoop.

-Este projeto faz parte do meu portfólio pessoal, então ficarei feliz se você puder me fornecer qualquer feedback sobre o projeto, código, estrutura ou qualquer coisa que você possa relatar que possa me fazer um melhor engenheiro de dados!

Email-me: henricao_7@yahoo.com.br

Connect with me at [LinkedIn](https://www.linkedin.com/in/henrique-castro-484269203//).

## Iniciando o Projeto

A auditoria interna tem um papel importante na revisão das operações desenvolvidas pelas organizações, a fim de assegurar a administração de que as atividades estão sendo feitas conforme as metas estabelecidas, essa revisão é fundamental para que a empresa consiga melhorar seus processos adquirindo confiabilidade a sua marca e credibilidade com seus clientes. 
Para habilitar auditoria eu tenho que configurar alguns arquivos é reiniciar o cluster. Quando habilitarmos auditoria uma grande quantidade de arquivos de log e gerado então precisa tomar cuidado para não deixar habilitado esse modo. Você faz auditoria e depois desliga ela é uma ferramenta temporária você habilita olha e observa o que está acontecendo em um período e desabilita se não vai causar um overhead desnecessário no ambiente 
No arquivo hadoop-env.sh nós precisamos habilitar o debug é só descomentar. Assim eu vou conseguir fazer a leitura do que está acontecendo no momento de conexão

![1](https://github.com/pand-eX/Auditoria_cluster_Hadoop/blob/main/Implementando%20Auditoria%20no%20Hadoop/1.png)

Embora eu esteja mostrando isso em modo de debug isso é muito útil para auditoria porque eu posso ter usuário tentando conectar diretamente no seu cluster habilitando esse recurso então você pode monitorar os arquivos de log para saber quem está tentando fazer a conexão, endereço ip de origem usuário que está tentando fazer a conexão e a partir daí tomar as medidas de segurança que forem necessárias. Administradores gosta de deixar habilitado por alguns dias ou semanas para saber ou ter uma ideia de quem está tentando conectar ao seu cluster isso é para habilitar conexão no cluster

![2](https://github.com/pand-eX/Auditoria_cluster_Hadoop/blob/main/Implementando%20Auditoria%20no%20Hadoop/2.png)

você descomentando essa linha você está habilitando o debug para JVM. O Kerberos faz a autenticação do hadoop então o hadoop será acionado em algum momento e por conta disso e como o hadoop utiliza a JVM eu consigo fazer a leitura de tudo que for processado pela JVM em detalhes. E Claro que aqui é um ambiente de teste nós não vamos ficar fazendo um monte de conexões, mas em um ambiente de empresa, cliente e etc... Portanto, habilita a auditoria deixa por um período e depois desativa se não os arquivos ficaram muito grandes.
Agora precisamos habilitar o log4j que está no mesmo diretório do hadoop-env.sh o arquivo é o log4j.properties. Esse arquivo tem recursos importantíssimos você pode habilitar uma série de configurações de auditoria, segurança e debug para você visualizar e ter uma ideia do que está acontecendo por padrão isso não vem configurado você precisa fazer as configurações.

Podendo trazer mais informações incluindo o MapRedunce então quando você executa um Job todo o processo de execução do Job você pode trazer mais detalhes para o log para saber por exemplo sobre performance, sobre consumo de processamento de suas máquinas, consumo de memória entre outras coisas consulte a documentação para configurar e testar alguns parâmetro

![3](https://github.com/pand-eX/Auditoria_cluster_Hadoop/blob/main/Implementando%20Auditoria%20no%20Hadoop/3.png)

Esse é o parâmetro que queremos mudar repare que substitui o parâmetro INFO para Error assim nós teremos condições de passar os detalhes do erro para nossos arquivos de log repare que cada arquivo terá um tamanho de 256MB então quando chegar nesse tamanho ele cria uma cópia do arquivo e inicia um novo arquivo a partir do zero então dependendo do volume do acesso ao seu cluster do Data Lake você claro você rapidamente pode ocupar muito espaço em disco.

![4](https://github.com/pand-eX/Auditoria_cluster_Hadoop/blob/main/Implementando%20Auditoria%20no%20Hadoop/4.png)

Aqui você vai obter detalhes sobre como os seus usuários estão usando o HDFS por exemplo se eles estão digitando muito comando incorretos se os comandos estão levando muito tempo para ser executados se tem muita mensagem de erro cada vez que executa o comando quais usuários estão usando comando e assim por diante. Uma coisa que vale apena mencionar é como nós habilitamos a segurança via Kerberos somente os usuários que tiverem o User Principals farão acesso isso por si só já acaba sendo uma camada de auditória. Você só vai criar um User para aqueles que realmente precisam acessar o seu Data Lake e isso facilita muito porque se você tiver um administrador de rede e ele acessar lá o servidor e tentar conectar no HDFS ele não vai conseguir porque ele não tem o User Principals por conta disso você já acaba tendo uma camada de segurança se ainda assim você achar necessário você vem na camada do HDFS e altera as opções de auditoria para que você possa registrar qualquer tipo de informação e depois investigar principalmente se os usuários estiverem puxando muitos dados do HDFS não necessariamente é um problema porque as vezes os cientista de dados tem que puxar os arquivos organizar e levar para um banco NoSQL por exemplo, mas se isso estiver acontecendo com muita frequência é o caso de investigar isso e realmente necessário? Quem está fazendo? E por que? E então e aí que entra em cena a questão ligadas a auditoria. Descendo mais um pouco nesse arquivo você tem mais informação sobre os DataNode e assim por diante. Nesse arquivo Log4j você encontra todos os detalhes necessários para habilitar auditória para os serviços que estão rodando com o Apache Hadoop e então levar infomação para o Log e depois fazer análise.
Agora é só fazer a cópia para o outro NameNode é reiniciar os serviços do cluster.

![5](https://github.com/pand-eX/Auditoria_cluster_Hadoop/blob/main/Implementando%20Auditoria%20no%20Hadoop/5.png)

Repare que essa configuração que é apenas no ambiente Apache Hadoop com o acesso especificamente no HDFS se por acabo você segurança no Hbase, Hive, Apache Pig ou em outro sistema do ecossistema tem que configurar o Kerberos para cada um dos componentes Não é tão trabalhoso porque você não precisa criar outro KDC a partir do zero podemos usar o mesmo KDC basta modificar os arquivos de configurações do produto do ecossistema para então fazer a autenticação via Kerberos mas tem outra alternativa um pouco mais simples o Apache Knox.
Arquitetura geral do Apache Knox.

![6](https://github.com/pand-eX/Auditoria_cluster_Hadoop/blob/main/Implementando%20Auditoria%20no%20Hadoop/6.png)

Veja que o Apache Knox fornece basicamente 3 serviços o de Proxy, Authentication Services e fornece ainda o SDK para que você possa desenvolver aplicações de autenticação para os produtos do ecossistema hadoop.

O Apache Knox é um Gateway de aplicação que funciona com as APIs REST e as interfaces do usuário de um grande número dos projetos de Big Data mais populares. Pode ser conveniente impor que os clientes de REST ou navegador interajam com o Apache Knox em vez de diferentes componentes de um cluster do Apache Hadoop, por exemplo. Em particular, o Apache Knox suporta uma ampla game de mecanismo diferentes para proteger o acesso ao cluster backend.
Ao invés de ter a interação e autenticação do usuário com cada serviço do Ecossistema você coloca um Proxy, ou seja, você coloca um serviço na frente.

![7](https://github.com/pand-eX/Auditoria_cluster_Hadoop/blob/main/Implementando%20Auditoria%20no%20Hadoop/7.png)

O Apache Knox é um sistema que fornece um único ponto de autenticação e acesso para serviços do Apache Hadoop em um cluster.
O Apache Knox simplifica a segurança do Hadoop para usuários que acessam os dados do cluster e executam tarefas e operadores que controlam o acesso e gerenciam o cluster. O gateway é executado como um servidor ou um cluster de servidores, fornecendo acesso centralizado a um ou mais clusters do Hadoop.

![8](https://github.com/pand-eX/Auditoria_cluster_Hadoop/blob/main/Implementando%20Auditoria%20no%20Hadoop/8.png)

Veja que eu tenho um tráfego público na barra azul e em baixo eu tenho tráfego privado na rede privada da empresa nós temos o Nodes Trabalhadores esses nodes é que tem os dados você tem o Node Master que é o gerente que controla os Metadados, mas os dados ficam nos workers por conta disso os Node ficam em uma rede Privada dentro da rede privada nós temos os MasterNodes só que com uma diferença eles tem uma interface para rede pública e privada com isso o usuário vai fazer acesso pelo Apache Knox fez a autenticação e o apache knox direciona para o que são Master o usuário por sua vez ele não tem acesso direto exatamente aos Node Trabalhadores que é a parte mais crítica do cluster isso adiciona uma camada de segurança.

## Serviço de Proxy


O principal objetivo do projeto Apache Knox é fornecer acesso ao Apache Hadoop por meio de intermediação de Recursos HTTP.
O Apache Knox fornece um meio de comunicação com o ecossistema hadoop através de protocolo http utilizando por exemplo uma Rest API por conta disso nem todos os serviços do ecossistema Hadoop pode ser autenticado via Apache Knox só aqueles que oferece algum tipo de interface via HTTP.

## Serviço de Autenticação 


Autenticação para acesso à API REST, bem como fluxo WebSSO para UIs. LDAP / AD, Kerberos, SAML, OAuth são todas as opções disponíveis.
## Serviço para Clientes
O desenvolvimento do cliente pode ser feito com scripts através de DSL ou usando as classes Knox Shell diretamente como SDK.
SDK é uma plataforma para desenvolvimento. Caso a empresa não encontre no Apache Knox algo que esteja pronto para uso a empresa pode desenvolver e tem um SDK disponível para isso com Apache Knox.

O Apache Knox suporta LDAP, Active Directory, SSO, SAML, bem como outros sistemas de autenticação. 
Então não é obrigatório se uma empresa acha o Kerberos complexo para ser implementado a empresa pode retirar o Kerberos do processo e faz autenticação via Apache Knox funciona tanto em cluster hadoop com ou sem segurança habilitada via Kerberos.
Você pode usar o Apache Knox com cluster Hadoop não protegidos e com clusters com segurança Kerberos habilitada. Se você usa clusters seguros do Kerberos no seu sistema corporativo, o Apache Knox fornece uma solução de segurança que inclua os seguintes recursos:
- Integra-se com soluções de gerenciamento de identidade corporativa.
- Protege os detalhes da implementação do cluster do Hadoop para que os hosts e as portas fiquem ocultos aos usuários finais.
- Simplifica o número de serviços com os quais um cliente deve interagir.

Portanto se a empresa trabalha com HDFS, Hbase, Hive e outros componentes fica muito mais fácil implementar o Apache Knox na infraestrutura. O usuário acessa somente um local e ele recebe a autenticação e vai para o serviço que ele precisa.
E importante considerar que nem todos os serviços do Ecossistema Hadoop são suportados pelo Apache Knox


## Os seguintes serviços do ecossistema Apache Hadoop têm integrações com o Apache Knox:
- Ambari
- WebHDFS(HDFS)
- Yarn RM
- Apache Hbase
- Apache Oozie
- Apache Hive/JDBC
- Apach Storm
- Apache SOLR
- Apache Livy (Spark REST Service)
- Kafka REST Proxy
UIs são páginas Web onde você consegue visualizar informações sobre aquele componente interagir com o knox.

## As seguintes UIs do ecossistema Apache Hadoop têm integrações com o Apache Knox:

- Name Node UI
- Job History UI
- Yarn UI
- Apache Oozie UI
- Apache HBase UI
- Apache Ambari UI
- Apache Ranger Admin Console
- Apache Zeppelin 
- Apache NiFi
- Hue

Quando você configura o Apache Knox primeiro você define como será a autenticação utilizando um dos muitos métodos disponíveis com o Knox depois você configura uma ACL para definir se o usuário, grupo ou IP tem acesso a cada produto do ecossistema tudo isso é configurado uma única vez lá no Apache Knox então o usuário já está acessando aquele produto ou serviço na sequência podemos habilitar Auditoria também com o Apache Knox e ele utiliza com o Log4j


![9](https://github.com/pand-eX/Auditoria_cluster_Hadoop/blob/main/Implementando%20Auditoria%20no%20Hadoop/9.png)

Apache Ranger Audit Framework que serve para construção de um Framework de auditoria para o ecossistema Hadoop.
O Apache Ranger é um Framework para ativar, monitorar e gerenciar a segurança de dados na plataforma Hadoop. Atualmente, ele fornece uma administração de segurança centralizada, controle de acesso e auditoria detalhada para o acesso do usuário nos componentes do Hadoop, incluindo Hive, Hbase e outros.
O Apache Ranger e o Apache Knox são produtos quase concorrentes o Ranger utiliza um processo de políticas de segurança para autenticação, auditoria e etc...
## O Objetivo do Framework:
- Fornecer administração de segurança para gerenciar todas as tarefas em uma UI central ou usando APIs REST>
- Autorização detalhada para executar uma ação e uma operação específicas com o componente / ferramenta do Hadoop e gerenciada por meio de uma ferramenta de administração central.
- Padronizar o método de autorização em todos os componentes do Hadoop.
- Suporte aprimorado para vários métodos de autorização, como controle de acesso baseado em função e controle de acesso baseado em atributo.
- Fornecer a auditoria de acesso ao usuário e ações administrativas em todos os componentes do Hadoop.

![10](https://github.com/pand-eX/Auditoria_cluster_Hadoop/blob/main/Implementando%20Auditoria%20no%20Hadoop/10.png)

![11](https://github.com/pand-eX/Auditoria_cluster_Hadoop/blob/main/Implementando%20Auditoria%20no%20Hadoop/11.png)

## Apache Sentry
o Apache Sentry que basicamente tem a mesma função do Apache Knox e o Apache Ranger.
A ideia por traz do Apache Sentry é oferecer autenticação e autorização para produto do ecossistema hadoop. Ele não tem todos os recursos de auditoria presente principalmente no Apache Ranger entre esses 3 o Range é o mais forte em questão de auditoria, mas o Apache Sentry oferece um método de autorização também baseada em políticas muito similar do Apache Ranger. Você precisa instalar um Plug-in na máquina onde está rodando os componentes seja o HDFS, Hive e outros para que haja o sincronismo da política e os metadados são gravados diretamente no servidor onde você instalou o Sentry.

![12](https://github.com/pand-eX/Auditoria_cluster_Hadoop/blob/main/Implementando%20Auditoria%20no%20Hadoop/12.png)

![13](https://github.com/pand-eX/Auditoria_cluster_Hadoop/blob/main/Implementando%20Auditoria%20no%20Hadoop/13.png)

Nesse diagrama basicamente você encontra o objetivo veja que nós temos o Sentry no centro.
Entre esses 3 produtos o ideal é o Apache Ranger para quem quer fazer auditoria. O Apache Knox o ideal para quem quer apenas um Gateway de comunicação principalmente a partir de uma aplicação caso os usuários não for acessar diretamente os componentes mas vão acessar via aplicação. Entretanto, o Apache Sentry oferece suporte para alguns componentes que o Ranger é o Knox não oferece por exemplo um banco de dados Impala e você quer configurar autorização de acesso então você precisa usar o Sentry. Não existe uma única solução que atende toda a necessidade nada impede que você tenha o Apache Range é o Sentry no mesmo ambiente só vai aumentar a complexidade ambos precisam de um Plug-in um deamon instalado que vai rodar em cada servidor onde você tem um componente Hadoop no knox não há necessidade por que na verdade um gateway

