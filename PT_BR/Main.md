# POX


* [Instalando o POX](#inst)
  * [Requisitos](#req)
  * [Adquirindo o Código](#ac)
  * [Selecionando um Branch / Versão](#sb)
  * [PyPy Supporte](#pypy)
    
   <br>
   
* [Chamando o POX](#cham)
<br>	
* [Componentes do POX](#comp)
  * Componentes padrão
       * py
       * forwarding.l2_learning
       * forwarding.l2_pairs
       * forwarding.l3_learning
       * forwarding.l2_multi
       * openflow.spanning_tree
       * web.webcore
       * messenger
       * openflow.of_01
       * openflow.discovery
       * openflow.debug
       * openflow.keepalive
       * misc.pong
       * misc.arp_responder
       * misc.packet_dump
       * misc.dns_spy
       * misc.of_tutorial
       * misc.mac_blocker
       * log
         * Desabilitando o console de Log
         * Formatação de Log
         * Saída de Log
       * log.color
       * log.level
       * samples.pretty_log
       * tk
    
  * [Desenvolvendo seus próprios componentes](#dev)
       * O diretório "ext"
       * A função de "envocar"
          * Um exemplo simples
          * Envocação múltipla
          
<br>

* [API's do POX](#api)
  * Trabalhando com o POX: O objeto POX Core
       * Registrando componentes
  * Trabalhando com endereços: pox.lib.addresses
  * O sistema de eventos: pox.lib.revent
       * Tratando eventos
          * Tratadores de eventos
          * Escutando um evento
          * Setando escutadores automáticos
       * Criando seu próprio tipo de evento
       * Levantando eventos
  * Trabalhando com pacotes: pox.lib.packet
       * Ethernet (ethernet)
       * IP versão 4 (ipv4)
       * TCP (tcp)
          * tcp_opt class
       * Exemplo: Mensagens ARP
  * Threads, Tasks, e Timers: pox.lib.recoco
       * Executando códigos no futuro com um Timer
  * Trabalhando com sockets: ioworker
  
  <br>
  
* [OpenFlow no POX](#of)
  * Eventos OpenFlow : Respondendo aos Switches
       * ConnectionUp
       * ConnectionDown
       * PortStatus
       * FlowRemoved
       * Statistics Events
       * PacketIn
       * ErrorIn
       * BarrierIn
  * Mensagens OpenFlow
       * ofp_packet_out - Enviando pacotes apartir do switch
       * ofp_flow_mod - Modificação da tabela de fluxo
          * Exemplo: Instalando uma entrada na tabela
          * Exemplo: Limpando tableas em todos os switches
       * ofp_stats_request - Requisitando estatísticas de todos os switches
          * Exemplo - Estatísticas de fluxo Web
  * Estrutura de combinação
       * Definir uma combinação apartir de um pacote existente
  * Ações OpenFlow
       * Output
       * Enqueue
       * Setar ID de VLAN
       * Setar prioridade de VLAN
       * Setar endereço Ethernet de origem ou destino
       * Setar endereço IP de origem ou destino
       * Setar tipo de serviço IP
       * Setar porta de origem ou destino TCP/UDP
  * Comunicando-se com um Datapath (Switch)
       * Objetos Connection
       * Exemplo: Enviando um FlowMod
       * Exemplo: Enviando um PacketOut
       
<br>

* [Ferramentas de terceiros, Tutoriais, Etc.](#etc)
   * POXDesk: Uma interface gráfica web para o POX
   * Tutorial OpenFlow
   * Tutorial Switch OpenFlow
   * Exemplo de Coletor de estatísticas de fluxo
* [Convenções de codificação](#code)

<br>

* [FAQs](#faq)
   * Como posso mudar a porta Openflow 6633?
   * Como posso ter algum componente iniciando automaticamente toda vez que rodar o POX?
   * Como posso fazer os switches enviarem um pacote com payload completo para o controlador?
   * Como posso estabelecer comunicação entre os componentes?
   * Como posso usar o POX com o Mininet?
   * Meu código não está funcionando! Podem me ajudar?


<a id = "inst"/>
##Instalando o POX
</a>
<a id = "rec"></a>
### Requisitos


POX requer Python 2.7.  Na prática, ele também pode rodar com 2.6, mas ninguém está atualmente dando suporte a isso.


POX oficialmente suporta Windows, Mac OS, e Linux (contudo ele tem sido usado em outros sistemas também).  Muito desenvolvimento ocorre no Mac OS, então ele quase sempre funciona no Mac OS.  Ocasionalmente ocorrem falhas em outros Sistemas operacionais. O tempo gasto para se solucionar esses problemas é ligado a quão rápido os mesmos são reportados. Em geral, problemas no Linux são encontrados bem rápido (especialmente problemas grandes) enquanto no Windows isso é mais lento. Se você perceber algo não funcionando ou que pareça estranho, por favor, submeta a ocorrência no tracker do github ou envie uma mensagem para a lista de email do pox-dev para que possa ser solucionado!


POX pode ser usado com o interpretador "padrão" do Python (CPython), mas também suporta PyPy (veja abaixo).


Adquirindo o Código


A melhor maneira para trabalhar com o POX é utilizar um repositório git. Você também pode adquirir em arquivo tar ou zip, porém o código de controle geralmente é melhor.


POX está hospedado no github. Se pretende fazer modificações no POX em si, você pode criar seu repositório próprio no github atraves da página do repositório POX. Se apenas deseja obter rapidamente para executar ou só brincar com, você pode simplesmente criar uma cópia local.


`$ git clone http://github.com/noxrepo/pox
$ cd pox
`

 
Selecionando um branch/ versão


O repositório POX possui várias versões. Especificamente, tem ao menos uma versão de lançamento e uma versão ativa. A versão padrão (adquirida atraves dos comandos citados acima) será a versão mais recentemente lançada. Versões de lançamento podem ter algumas atualizações, mas não estão sendo mais desenvolvidas. Por outro lado as versões ativas recentes estão sendo desenvolvidas mais ativamente. Em teoria branches/versões de lançamento tendem a ser mais estáveis ( Queremos dizer que se está trabalhando em algo, não vamos quebrar isso). Em contrapartida os branchs/versões ativas conterá melhorias (conserto de bugs, novas funcionalidades, etc.) Se você irá basear seu trabalho em um ou em outro irá depender de suas necessidades. Uma coisa pode ser um fator em sua decisão é que se você estiver utilizando uma versão/branch ativa, você provavelmente vai receber um melhor suporte nas listas de discurssão.   (muitas respostas começam com "upgrade para o ramo ativo").
No momento em que este é escrito, o ramo ativo é chamado betta. Para usar o ramo de betta, basta dar uma olhada após a clonagem do repositório:


`~$ git clone http://github.com/noxrepo/pox
~$ cd pox
~/pox$ git checkout betta
`

Suporte PyPy
Embora não seja tão fortemente testado como o interpretador Python normal, é um objetivo do POX rodar bem em tempo de execução do Python PyPy. Há duas vantagens nisso. Primeiro, PyPy geralmente é um pouco mais rápido que CPython. Segundo, é facilmente portátil - voce pode facilmente empacotar POX e PyPy em uma única tarball e te-los pronto para rodar.
Você pode é claro, baixar, instalar e invocar PyPy do modo usual. No Mac OS e no Linux, contudo, POX também suporta um método muito simples: Baixe a última tarball PyPy para o seu SO, e descompactá-lo em uma pasta chamada "PyPy" ao lado pox.py. Depois é só executar pox.py como de costume (./pox.py), e ele deve usar PyPy em vez de CPython.

<a id = "cham"></a>
##Chamando o  POX


POX é chamado executando pox.py. POX tem alguns argumentos opcionais que podem ser passados via linha de comando, que podem ser usados no começo da linha de comando:


Opção               Significado
--verbose           Mostra informações extras (especialmente útil para debugar problemas na inicialização)
--no-cli                Não inicia o shell interativo (não se aplica mais desde o beta)
--no-openflow    Não inicia esperando por conexões OpenFlow


Mas rodar o POX somente não faz muita coisa - as funcionalidades do POX são providas pelos seus componentes (o POX vem com componentes úteis, mas o público alvo do POX são as pessoas que querem desenvolver elas mesmas seus componentes). Os componentes são especificados na linha de comando após todas as opções listadas acima. Um exemplo de um componente POX é o forwarding.l2_learning. Este componente cria um switch OpenFlow que se comporta de forma similar a um learning switch L2. Para rodar esse componente, você precisa simplesmente precisa colocar o seu nome na linha de comando após todas as opções do POX:


`./pox.py --no-cli forwarding.l2_learning
`

Você pode especificar vários componentes na linha de comando. Nem todos os componentes trabalham bem juntos, mas alguns conseguem. Ate mais que isso, alguns componentes dependem de outros componentes, dessa forma, é necessário que você especifique mais de um componentes. Por exemplo, você pode rodar o servidor web POX juntamente com o l2_learning:


`./pox.py --no-cli forwarding.l2_learning web.webcore
`

Alguns componentes possuem argumentos. Esses devem ser especificados após o nome do componente e (como os argumentos do POX) precedidos de dois hiféns. Por exemplo, l2_learning tem um modo chamado "transparent" onde os switchs vão rotear ate mesmos os pacotes que seriam usualmente descartados (como as mensagens LLDP), e o número da porta do servidor web pode ser alteradas do padrão (8000) para uma porta qualquer. Por exemplo:


`./pox.py --no-cli forwarding.l2_learning --transparent web.webcore --port=8888
`

(Se você está começando a pensar que as linhas de comando podem ficar um pouco longas e complexas, há uma solução: escreva um componente simples responsável apenas por chamar outros componentes.)

<a id = "comp"></a>
##Componentes no POX

Quando falamos sobre os componentes dentro do POX, o que realmente queremos dizer é algo que podemos colocar na linha de comando ao chamar o POX, como descrito em “Chamando o  POX”. Nas seções seguintes discutiremos alguns componentes que vem junto com o POX e como você deve proceder para criar o seu próprio.

Componentes embutidos
O POX vem com alguns componentes embutidos. Alguns deles proveem funcionalidades básicas, algumas proveem funcionalidades convenientes e outras são somente exemplos. Abaixo são listadas algumas:

py
Este componente faz com que o POX inicialize um interpretador interativo de Python, que pode ser útil para debugar e testes interativos. Antes do beta, este era era executado por padrão (a não ser quando era desabilitado com a opção --no-cli). Outros componentes podem adicionar funções / valores ao namespace deste interpretador.



forwarding.l2_learning
Este componente cria switchs OpenFlow que agem como um learning switch L2. Este funciona de forma similar ao exemplo “pyswitch” do NOX, no entanto, a implementação é um pouco diferente. Enquanto esse componente aprende os endereços L2, os fluxos que ele cria coincidem com o maior número de campos possíveis. Por exemplo, diferentes conexões TCP resultam em diferentes fluxos.

forwarding.l2_pairs
Como o l2_learning, este componente também cria switchs OpenFlow que agem como um learning switch L2. No entanto, este é provavelmente a forma mais simples de fazer isso de forma correta. Diferente do l2_learning, o l2_pairs cria regras baseadas puramente no endereço MAC.

forwarding.l3_learning
Este componente não é exatamente um roteador, mas definitivamente também não é um switch L2. Ele é um “coisa” que faz switch em L3. Talvez o aspecto mais útil deste componente é que ele serve como um bom exemplo de uso da biblioteca de pacotes do POX para examinar e construir ARP resquests e replies.

forwarding.l2_multi
Este componente também pode ser visto como um learning switch, mas este é mais flexível comparado aos demais. Os outros learning switchs “aprendem” por switch, ou seja, tomam decisões como se cada switch só tivesse a informação local. O l2_multi utiliza o openflow.discovery para aprender a topologia da rede como um todo, permitindo que assim que um switch saiba onde se encontra um endereço MAC, todos também saibam.


Note que isto não tem muita relação com o protocolo Spanning Tree. Eles tem objetivos similares, mas fazem isso de forma bem diferente.

O componente samples.spanning_tree demonstra o uso desse módulo carregando o mesmo e um dos vários componentes de roteamento.

web.webcore
O componente webcore inicia um servidor web dentro do processo do POX. Outros componentes podem se comunicar com esse módulo para disponibilizar conteúdo estático e dinâmico.

messenger
O componente messenger disponibiliza uma interface para o POX interagir com processos externos via mensagens bidirecionais baseadas em JSON. O messenger é na realidade somente uma API, a comunicação propriamente é implementada por transporte. Atualmente, esse transporte existe para sockets TCP e para HTTP.

openflow.of_01
Esse componente comunica com switchs OpenFlow 1.0 (protocolo físico 0x01). E normalmente iniciado por padrão (a não ser que seja especificada a opção --no-openflow via linha de comando), mas as vezes você deseja iniciar-lo manualmente, para permitir que você troque suas opções (por exemplo, para definir qual interface ou porta ele está escutando).

openflow.discovery
Este componente envia mensagens LLDP especialmente construidas para fora dos switchs OpenFlow, permitindo que este avalie a topologia da rede. Este gera eventos (os quais você pode capturar) quando os links levantam ou caem.


openflow.debug
Carregar esse componente irá fazer com que o POX crie capturas em pcap contendo as mensagens OpenFlow, que você pode depois abrir com o Wireshark para analisar. Todos os cabeçalhos são sintéticos, logo isso não é um substituto completo para o tcpdump ou Wireshark. No entanto, ele tem uma propriedade legal que é ter exatamente uma mensagem OpenFlow por frame (o que torna a leitura bem mais fácil).

openflow.keepalive
Este componente faz com que o POX gere echo requests periódicos para os switchs conectados. Alguns dos switchs vão assumir que uma conexão de controle em estado oscioso indica perda de conectividade para o controlador e por isso, irá desconectar após algum período de silêncio (frequentemente não muito longo), e este componente pode ser usado para prevenir isso. (Alguém pode facilmente argumentar que se o switch quer disconectar uma conexão osciosa, é de responsabilidade dele enviar echo requests, mas argumentar não irá corrigir o firmware.)


misc.pong
O componente pong é um tipo de exemplo bobo que simplesmente espera por ICMP echo requests e responde os mesmos. Se você executar esse componente, todos os pings irão reportar que foram bem sucedidos! Este componente serve como um simples exemplo de como monitorar e enviar pacotes, bem como trabalhar com ICMP.


misc.arp_responder
Um utilitário ARP que permite aprender e servir de proxy ARP, também pode responder pedidos usando uma lista estática de entradas.

misc.packet_dump
Um simples componente que imprime a informação packet_in no log. Similar a rodar um tcpdump em um switch.

misc.dns_spy
Este componente monitora respostas do DNS e armazena seus resultados. Outros componentes podem examinar esses resultados acessando core.DNSSpy.ip_to_name[<endereço ip>] e core.DNSSpy.name_to_ip[<nome do domínio>].

misc.of_tutorial
Este componente é para ser utilizado com o tutorial do OpenFlow. Ele age como um simples hub, mas pode ser modificado para agir como um learning switch L2.

misc.mac_blocker
Este componente foi feito para ser utilizado juntamente com outra aplicação de roteamento reativo, como l2_learning e l2_pairs. Ele cria um pop up gráfico baseado em Tkinter que permite você bloquear um endereço MAC. Ele demonstra como utilizar eventos de alta prioridade para bloquear PacketIns e como criar elementos gráficos baseados em Tkinter dentro do POX.


log
O POX utiliza o sistema de log do Python, e o módulo de log permite que você configure uma boa parte disso utilizando a linha de comando. Por exemplo, você pode enviar o log para um arquivo, mudar o formato das mensagens de log para incluir data, etc.


Desabilitando o console de log
Você pode desabilitar o log “normal” do POX utilizando:

```
./pox.py log --no-default
```

Formatando o log
Por favor, veja a documentação dos atributos do LogRecord do Python para detalhes sobre como formatar o log. Um exemplo rápido, você pode adicionar marcações de tempo nos seus logs da seguinte maneira:

```
./pox.py log --format="%(asctime)s: %(message)s"
```

Ou com marcações de tempo mais simples:

```
./pox.py log --format="[%(asctime)s] %(message)s" --datefmt="%H:%M:%S"
```

Para outro exemplo, veja o component samples.pretty_log (particularmente, para um exemplo que utiliza a extensão de log colorido do POX ).

Saída do log
Mensagens de log são processadas por vários gerenciadores que imprimem o log na tela, salvam-o em um arquivo, enviam-o pela rede, etc. Você pode escrever o seu próprio, mas o Python já vem com alguns, que estão documentados no manual do Python na parte de gerenciadores de log. O POX permite que você configure vários dos gerenciadores embutidos do Python diretamente pela linha de comando; você deve buscar no manual do Python as referências para os argumentos, mas especificamente, o POX permite que você os configure.hon reference for the arguments, but specifically, POX lets you configure:


Nome     Tipo
stderr     StreamHandler para a saída stderr
stdout    StreamHandler para a saída stdout
File    FileHandler para o arquivo a definido como argumento
WatchedFile    WatchedFileHandler
RotatingFile    RotatingFileHandler
TimedRotatingFile    TimedRotatingFileHandler
Socket    SocketHandler - Envia para o socket TCP
Datagram    DatagramHandler - Envia utilizando UDP
SysLog    SysLogHandler - Gera a saída utilizando o serviço do syslog
HTTP    HTTPHandler - Gera a saída para um servidor web utilizando GET ou POST
Para utilizar esses argumentos, basta especificar o nome do mesmo seguido pelos argumentos do tipo de saída separados por vírgula. Por exemplo, FileHandler necessita do nome do arquivo, e opcionalmente o modo de abertura do arquivo (que abre por padrão como append), então, você deve utilizar:

```
./pox.py log --file=pox.log
```

Ou, se você quiser que o arquivo seja sobrescrito toda vez:

```
./pox.py log --file=pox.log,w
```

Você também pode utilizar os argumentos nomeados colocando antes do nome do mesmo um ‘*’ e então utilizando uma lista separada por vírgula de pares chave=valor. Por exemplo:

```
./pox.py log --*TimedRotatingFile=filename=foo.log,when=D,backupCount=5
```

log.color
O módulo log.color colore o log quando possível. Isto é bem legal, mas para tirar melhor proveito desse recurso, é necessário um pouco de configuração - sugere-se que você olhe o samples.pretty_log.

Colorir o log deve funcionar bem no Mac OS, Linux e outros ambientes com um real conceito de terminal. No Windows, você precisará de um colorizador como o colorama.

log.level
O POX utiliza a infraestrutura de log do Python. Cada componente tem seus próprios loggers, o nome de cada é mostrado como parte da mensagem de log. Os loggers atualmente formam uma hierarquia - você tem o logger “foo” com o sub-logger “bar”, que juntos serão conhecidos como “foo.bar”. Adicionalmente, cada mensagem de log tem um “nível” associado, que corresponde que determina o quão importante (ou severa) a mensagem é. O componente log.level permite que você configure qual o nível de detalhe de log de cada logger. Os níveis de de log do mais severo para o menos severo são:

CRITICAL
ERROR
WARNING
INFO
DEBUG

O nivel de log padrão é o INFO. Para definir um nível diferente do padrão (exemplo, um nível diferente para o “root” da hierarquia dos loggers):

```
./pox.py log.level --WARNING
```

Se você estiver tentando debugar um problema com as conexões OpenFlow, sugere-se que você aumente o nível de detalhe dos logs relacionados com o OpenFlow. Você pode ajustar todas as mensagens de log relacionadas com o OpenFlow da seguinte maneira:

```
./pox.py log.level --WARNING --openflow=DEBUG
```

Se isto deixar você com muitas mensagens do nível de DEBUG do openflow.discovery, as quais você não está interessado, você pode desligar-las especificamente:

```
./pox.py log.level --WARNING --openflow=DEBUG --openflow.discovery=INFO
```

samples.pretty_log
Este simples módulo utiliza o log.color e um formato customizado de log para fornecer uma boa e funcional saída do log no console.

tk
Este componente tem o objetivo de auxiliar a construção de interfaces gráficas dentro do POX baseadas em Tk, incluindo as simples caixas do dialog. Este recurso ainda é experimental.


<a id = "dev"></a>
##Desenvolvendo seus próprios componentes


Essa sessão tem por objetivo lhe orientar no desenvolvimento de suas próprias aplicações para o POX. Em alguns casos você pode encontrar algum componente que faz exatamente o que você quer. Em outros, você começará fazendo uma cópia desse componente, começando a trabalhar a partir daí.

###O diretório "ext"

Como discutido,os componentes POX são somente módulos desenvolvidos em Python. Sendo que você pode salvar seu código em qualquer diretório deste que o POX possa encontrá-lo (por exemplo, no diretório especificado pela variável de ambiente: PYTHONPATH).O diretório superior do POX é chamado de “ext”, este diretório é um local conveniente para a construção dos nossos componentes, já que o POX é automaticamente adicionando ao path de pesquisa do Python (ou seja é visível aos outros módulos existentes). 

Sendo assim uma forma usual para se desenvolver um novo modulo POX é copiar um modulo existente (por exemplo: forwarding/l2_learning.py) para o diretório ext (por exemplo ext/my_component.py). E então assim realizar as modificações.

###Função de partida 

Ainda que um módulo do Python possar ser carregado através da linha de comando, um componente POX   adequado deve possuir uma função de inicialização. De uma forma genérica, uma função de partida chama o POX e inicializa o componente em questão. Essa função funciona como argumentos se argumentos fosses passados na linha de comando.

####Um exemplo Simples 

O POX em linha de comando, como mencionado anteriormente, contem os módulos que são necessários para o carregamento dos componentes. Após o nome do módulo vem em seguida  um conjunto de paramentos opcionais para o modulo. Por exemplo, uma possível linha de comando seria:

	./pox.py foo --bar=3 --baz --spam=disabled
	
Sendo no exemplo acima o nome do modulo “foo”, teremos um diretório chamado “foo” em algum lugar na qual o POX possa localizar que contenha um __init__.py ou simplesmente possua uma arquivo foo.py que o POX possa encontrar (por exemplo, no diretório ext), de forma mínima a função ficaria semelhante a:

```
def launch (bar, baz = "eggs", spam = True):
  print "foo:", bar, baz, spam
```
  

note que “bar” não possuem um valor padrão, fazendo assim dele um paramento não opcional, ao tentar executar ./pox.py foo sem argumentos irá retorna um erro de falta de um valor para o argumento bar, observe que no exemplo dado, “bar” recebe como string o valor  “3” se por algum motivo for necessário a utilização de algum tipo de dados fica a cargo do usuário a responsabilidade de converte os argumentos para o tipo desejado já que todos os argumentos serão passados como uma cadeia de caractere.
Note que o o valor padrão para o argumento “spam” é true. Para se alterar o valor deste atributo poderíamos realizar algo como –spam=False, mas isso iria passar uma argumento to tipo string (“False”), sendo esse um casso na qual a conversão de tipo explicita seria usada para um tipo apropriado. Para realizar a conversão do tipos poderíamos utilizar os mecanismos internos oferecidos pelo Python, built-in, int() ou float(), para valores booleanos, poderia escrever seu próprio procedimento ou utilizar a função pox.lib.str_to_bool(), que identifica padrões como “on”, “true” ou “enabled” como verdadeiro e qualquer outra entrada como falso.
Invocação múltipla

Agora ao executar a seguinte linha:

```
./pox.py foo --bar=1 foo --bar=2
```

Seria esperado o seguinte resultado:

```
foo: 1 eggs True
foo: 2 eggs True
```

Em vez deste comportamento, no entanto, é gerado um exceção. Por padrão é permitida apenas que seja executada somente uma única chamada a um componente, No entanto uma simples alteração na função de inicialização é suficiente para que uma invocação múltipla seja permitida. 

```
def launch (bar, baz = "eggs", spam = True, __INSTANCE__ = None):
  print "foo:", bar, baz, spam
```

após realizar as modificação mostrada acima, e tentar novamente realizar uma chamada múltipla, essa tentativa deve funcionar. Adicionando o parâmetro __INSTANCE__  sinaliza que a função aceita múltipla invocações, além de passar alguma informações para procedimentos que são chamados diversas vezes, os possíveis valores para o parâmetro: 

* O numero de instâncias (0...n-1)
* Numero total de instancias para esse modulo 
* True se for a última instância ou False caso não seja

podemos deseja, por exemplo, que o seu componente faça parte da inicialização somente uma única vez, mesmo se o se componente seja especificado varias vezes. Isso pode ser realizado facilmente, apenas realizando um teste se a ultima tupla for true. 


<a id = "api"></a>
##API’s do POX


POX contém um número de APIs para ajudá-lo a desenvolver aplicações de controle de rede. Aqui, nós tentamos descrever algumas delas. Isso certamente não é exaustivo, então sinta-se à vontade para contribuir.


Trabalhando com POX: O objeto POX Core


POX tem um objeto chamado “core”  que server como ponto central para muitas apis do POX. Algumas das funções que ele provê são só wrappers convenientes sobre outros funcionalidades e outras são únicas. Contudo, um outro principal propósito do core object é prover interoperabilidade entre componentes.  Frequentemente, ao invés de usar declarações de import para que um componente importe outro componente para que eles possam interagir, , componentes “registram” a si mesmos em core object e outros objetos requisitam o core object.
A maior vantagem dessa abordagem é que as dependendências entre os componentes são codificadas de forma fixa e diferentes componentes que possuem a mesma interface podem ser facilmente intercambiados. Pondo de outro modo, isso provê uma alternativa ao módulo de namespace normal do Python, o que torna mais fácil de manusiar.


Muitos módulos no POX vão querer acessar o core object. Por convensão, isso é feito importando core object, so seguinte modo:

```
from pox.core import core
```

TODO: Write more about this!


Registrando componentes


como mencionado acima, pode ser conveniente para um componente “registrar” um objeto de provimento de API no core object. Um exemplo disso pode ser encontrado na implementação do OpenFlow - O componente POX OpenFlow por padrão registra uma instância de OpenFlowNexus como core.openflow, e outras aplicações pode então acessar muitas funcionalidades do OpenFlow apartir daí. Há basicamente dois modos de se registrar um componente - usando core.register() e usando core.registerNew(). O último é de fato só um wrapper conveniente sobre em torno do primeiro. 


core.register() recebe dois argumento. O segundo é o objeto que queremos no core. O primeiro é o nome que queremos usar para isso. Aqui temos um exemplo de um componente simples com uma função launch() que registra esse componente como core.thing:
 
```
  class MyComponent (object):
  def __init__ (self, an_arg):
        self.arg = an_arg
        print "MyComponent instance registered with arg:", self.arg


  def foo (self):
        print "MyComponent with arg:", self.arg


def launch ():
  component = MyComponent("spam")
  core.register("thing", component)
  core.thing.foo() # prints "MyComponent with arg: spam"
  
```  

No caso de, por exemplo, funções de execução que podem ser invocadas múltiplas vezes, você pode querer só registrar um objeto uma vez. Você pode simplesmente checar se se ocomponente já foi registrado (usando core.hasComponent()), mas isso pode ser feito também com core.registerNew(). Enquanto você passa um objeto específico para core.register(),  você passa uma classe para core.refisterNew(). Se o componente em questão já foi registrado, registerNew() não faz nada.


registerNew() geralmente recebe um único parâmetro- a classe qeu você deseja instanciar. Se o métido __init)) dessa classe recebe argumentos, voce pode passá-los como parâmetros adicionais para registerNew(). Por exemplo, podemos mudar a função de execução acima para: 

```
def launch ():
  core.registerNew(MyComponent, "spam")
  core.MyComponent.foo() # prints "MyComponent with arg: spam"
```

Note que registerNew() registra automaticamente o objeto recebido usando o nome de sua classe (isto é, ele agora é “MComponent” ao invéz de “thing”). Isso pode ser sobreescrito fornecendo um atributo _core_name ao objeto:

```
class MyComponent (object):
  _core_name = "thing"


  def __init__ (self, an_arg):
        self.arg = an_arg
        print "MyComponent instance registered with arg:", self.arg


  def foo (self):
        print "MyComponent with arg:", self.arg
```

Trabalhando com endereços: pox.lib.addresses


Endereços IP e Ethernet no POX são representados por classes IPAddr e EthAddr de pox.lib.addresses. Em alguns casos, outros formatos de endereços podem tmabém funcionar (e.g., endereços IP em quádrupla separada por pontos), mas usar classes de endereçamento deve sempre funcionar. 


Por exemplo, quando trabalhamos com endereços IP:

```
from pox.lib.addresses import IPAddr, EthAddr


ip = IPAddr("192.168.1.1")
print str(ip) # Prints "192.168.1.1"
print ip.toUnsignedN() # Converte em  formato inteiro sem sinal -- 16885952
print ip.toRaw() # Retorna um objeto de tamanho de quatro bytes(uma string de quatro bytes, mais ou menos)


ip = IPAddr(16885952,networkOrder=True)
print str(ip) # Também imprime "192.168.1.1" !
```

O sistema de eventos: pox.lib.revent


Tratamento de eventos no POX se encaixa dentro do paradigma inscrição e publicação (publish/subscribe). Certos objetos publicam eventos (em linguajar da revent, esse é um evento de “raising”; às vezes também denominado “sourcing”, “firing” ou “dispatching” um evento). Podemos nossubescrever à um evento específico desses objetos (em linguajar da revent, chamado “listening to”; também “handling”, ou “sinking”); o que queremos dizer é que quandp eventos ocorrem, nós gostaríamos que uma parte específica de código seja chamada (um “event handler” ou às vezes um “event listener”). (Se há algo que podemos dizer sobre eventos, é que não há uma terminologia simplista ou encurtada.)


A biblioteca revent pode na verdade fazer algumas coisas estranhas. POX somente usa apenas uma porção de subconjunto de coisas não estranhas dessa funcionalidade, e particularmente usa só um subconjunto desse subconjunto! O que é descrito nessa seção é o subconjunto que o POX mais pesadamente. 
Eventos no POX são todas instancias de subclasses de reventEvent. Uma classe que levanta um evento (uma fonte de evento) herda de revent.EventMixin, e declara quais eventos ela levanta em um uma variével de classe denominada _eventMixin_Events. Aqui está um exemplo de uma classe que levanta dois eventos:     

```
class Chef (EventMixin):
  """
  Class modeling a world class chef


  This chef only knows how to prepare spam, but we assume does it really well.
  """
  _eventMixin_events = set([
        SpamStarted,
        SpamFinished,
  ])
```

Tratando eventos


Então talvez seu programa possua um objeto de uma classe Chef chamada chef. Você sabe que ele gera um par de eventos. Talvez você esteja interessado quando seu delicioso spam esteja pronto, então você gostaria de escutar o evento SpamFinished.
  


Tratadores de eventos
Primeiramente, vejamos exatamente como um evento escutador se parece. Resimindo: é uma função (ou um método ou alguma coisa que é invocável). Eles quase sempre recebem somente um argumento- o objeto do evento em si (contuto isso nem sempre é o caso- uma classe de evento pode mudar seu comportamento, em todo caso, essa documentação deve mencionar isso!). Assumindo SpamFinished como um evento típico, ele pode ter um tratador do tipo:

```
def spam_ready (event):
  print "Spam is ready!  Smells delicious!"
```

Listening To an Event


Agora precisamos setar de fato nossa função spam_ready para ser uma escutadora do evento SpamFinished:
 
chef.addListener(SpamFinished, spam_ready)


Às vezes você pode não ter a classe do evento (e.g., SpamFinished) no escopo. Você pode a importar caso você queira, mas você pode também usar o método addListenerByName()  alternativamente:


chef.addListenerByName("SpamFinished", spam_ready)


Setando escutadores automaticamente
Frequentemente, seu evento escutador é um método de uma classe. Tmabém, você está frequentemente interessado em escutar múltiplos eventos de um mesmo objeto fonte. revent provê um atalho para essa situação: addListeners().

```
class HungryPerson (object):
  """ Models a person that loves to eat spam """


  def __init__ (self):
        chef.addListeners(self)


  def _handle_SpamStarted (self, event):
        print "I can't wait to eat!"


  def _handle_SpamFinished (self, event):
        print "Spam is ready!  Smells delicious!"
```

Quando você chama foo.addListeners(bar), ele procura nos eventos de foo, e se encontra um método em bar um nome como  _handle_EventName, ele seta esse método como escutador.


Em alguns caso, você pode querer apenas que uma única classe escute eventos de múltiplas fontes. Às vezes é importante que você distingua ambos entre si. Para esse propósito, você pode também usar um “prefixo” que é inserido aos nomes dos tradores:

```
class VeryHungryPerson (object):
  """ Models a person that is hungry enough to need two chefs """


  def __init__ (self):
        master_chef.addListeners(self, prefix="master")
        backup_chef.addListeners(self, prefix="secondary")


  def _handle_master_SpamFinished (self, event):
        print "Spam is ready!  Smells delicious!"


  def _handle_secondary_SpamFinished (self, event):
        print "Backup spam is ready.  Smells slightly less delicious."
```

Criando seus próprios tipos de eventos
Como visto acima, eventos são subclasses de revent.Event. Então para criar um evento, simplesmente crie uma subclasse de Event. Você pode adicionar qualquer atributo extra ou métodos que você queira. Continuando nosso exemplo:

```
class SpamStarted (Event):
  def __init__ (self, brand = "Hormel"):
        Event.__init__(self)
        self.brand = brand


  @property
  def genuine (self):
        # If it's not Hormel, it's just canned spiced ham!
        return self.brand == "Hormel"

```

Note que você deve explicitamente chamar o método __init__() da superclase! (Você pode fazer isso como acima, ou fazer do jeito mais novo super(MyEvent, self).__init__().)


Voila! Você pode agora gerar novas instancias do seu evento!


Perceba que em nossos tratadores para os eventos de SpamStarted, poderíamos ter acessado os atributos brand ou genuine no objeto do evento que é passado ao tratador.  


Levantando eventos
Para levantar um evento de tal modo que os escutadores sejam notificados, você precisa chamar raiseEvent n oobjeto que publica o evento:


`# Uma forma de fazer isso
chef.raiseEvent(SpamStarted("Generic"))
`

`# Outra forma (ligeiramente preferível)
chef.raiseEvent(SpamStarted, "Generic")
`

(A segunda maneira é levemente preferível por que se não ouver qualquer escutar, ela impede a criação do objeto do evento)




Frequentemente, uma classe vai levantar eventos em si mesma (self.raiseEvent(...)), mas como você pode ver no exemplo acima, esse não é necessariamente o caso.


Trabalando com pacotes: pox.lib.packet
Muitas aplicações no POX interagem com pacotes (e.g., você pode querer construir pacotes e enviálos de um switch, ou você pode recebê-los de um switch por meio da mensagem OpenFlow ofp_packet_in). Para facilitar isso, POX tem uma bilibioteca para parsing e criação de pacotes. Essa bibioteca tem suporte a um número de diferentes tipos de pacotes.


Muitos pacotes têm algum tipo de cabeçalho e um payload. U payload é frequentemente outro tipo de pacote. Por exemplo, no POX geralmente trabalha-se com pacotes ethernet que contêm pacotes ipv4 (que geralemnte contém pacotes tcp...). Alguns tipos de pacotes suportados pelo POX são:  


ethernet
ARP
IPv4
ICMP
TCP
UDP
DHCP
DNS
LLDP
VLAN


Todos as classes de pacotes do POX estão localizadas em pox/lib/packet. Por convenção, você pode importar a biblioteca de pacotes do POX do seguinte modo:


import pox.lib.packet as pkt
Pode-se navegar pelos pacotes encapsulador de duas formas: usando o atributo de payload do objeto de pacote, ou usando o método find(). Por exemplo, aqui é mostrado como passar uma mensagem ICMP usando o atributo de payload:

```
def parseICMP(packet):
        if eth_packet.type == ethernet.IP_TYPE:
            ip_packet = eth_packet.payload
            if ip_packet.protocol = ipv4.ICMP_PROTOCOL
                icmp_packet = ip_packet.payload
...
```

Essa provavelmente não é a melhor forma de se navegar por um pacote, mas ilustra a estrutura de um cabeçalho de pacote no POX. Em cada nível de encapsulamento os valores do cabeçalho do pacote podem ser obtidos. Por exemplo, o endereço de IP de origem do pacote 
...
```
src_ip = ip_packet.srcip
icmp_sequence = icmp_packet.seq
```

E similarmente para outros cabeçalhos de pacotes. Consulte o código específico do pacote para outros cabeçalhos. 


Um método find de algum objeto pode ser usado para encontrar um pacote encapsulado específico atravéz do nome desejado (e.g., “icmp”) ou sua classe (e.g., pkt.ICMP). Se opacote não encapsula o tipo de pacote requisitado, find() retorna None. Por exemplo:

```
def handle_IP_packet (packet):
  ip = packet.find('ipv4')
  if ip is None:
        # This packet isn't IP!
        return
  print "Source IP:", ip.srcip
```

A seguinte sessão detalha alguns atributos/métodos/constantes úteis de alguns tipos de pacotes suportados.


Ethernet (ethernet)
Attributos:


dst (EthAddr)
src (EthAddr)
type (int) - O tipo ethernet ou o comprimento do campo ethernet.  Ele será  0x8100 para quadros com tags de VLAN
effective_ethertype (int) - O tipo ethernet ou o comprimento do campo ethernet. Para quadros com tags de VLAN, ele será o tipo referenciado pelo cabeçalho de VLAN


Constantes:


IP_TYPE, ARP_TYPE, RARP_TYPE, VLAN_TYPE, LLDP_TYPE, JUMBO_TYPE, QINQ_TYPE - Uma variedade de tipos ethernet


IP version 4 (ipv4)
Atributos:


srcip (IPAddr)
dstip (IPAddr)
tos (int) - 8 bits do Type Of Service / DSCP+ECN
id (int) - campo de identificação
flags (int)
frag (int) - offset de fragmentação
ttl (int)
protocol (int) - Número do protocolo IP do payload
csum (int) - checksum


Constantes:


ICMP_PROTOCOL, TCP_PROTOCOL, UDP_PROTOCOL - Vérios números de protocolo IP
DF_FLAG - Flag de não fragmentação
MF_FLAG - Mais flags de bit de fragmentação


TCP (tcp)
Atributos:


srcport (int) - Porta TCP de origem
dstport (int) - Porta TCP de destino
seq (int) - Número de sequência
ack (int) - Número de ACK
off (int) - offset
flags (int) - Flags como campos de bits (flags em caixa alta mais fáceis de serem usadas)
csum (int) - Checksum
options (lista de objetos tcp_opt)
win (int) - tamanho da janela 
urg (int) - ponteiro de urgência
FIN (bool) - Verdadeiro quando a flag FIN está setada
SYN (bool) - Verdadeiro quando a flag SYN está setada
RST (bool) - Verdadeiro quando a flag RST está setada
PSH (bool) - Verdadeiro quando a flag PSH está setada
ACK (bool) - Verdadeiro quando a flag ACK está setada
URG (bool) - Verdadeiro quando a flag URG está setada
ECN (bool) - Verdadeiro quando a flag ECN está setada
CWR (bool) - Verdadeiro quando a flag CWR está setada


Constantes:


FIN_flag, SYN_flag, etc. - Bits correspondo as flags


classe tcp_opt 
Atributos:


type (int) - Opção de ID de TCP(provavelmente correspondendo às constantes abaixo)
val (varies) - Valor de Opção


Constantes:


EOL, NOP, MSS, WSOPT, SACKPERM, SACK, TSOPT - IDs de tipo de Opção
Exemplo: Mensagens de ARP


Você provavelmente pode querer que o cotrolador sirva de proxy para requisiões de ARP ap invéz de inundá-los todos na rede dependendo do fato de você já conhecer que o endereço MAC da máquina que a requisição ARP está procurando. Para lidar com pacotes ARP você deve ter um escutador de eventos programado para receber pacotes que chegam como mostrado:  

```
def _handle_PacketIn (self, event):
        packet = event.parsed
        if packet.type == packet.ARP_TYPE:
            if packet.payload.opcode == arp.REQUEST:
                arp_reply = arp()
                arp_reply.hwsrc = <requested mac address>
                arp_reply.hwdst = packet.src
                arp_reply.opcode = arp.REPLY
                arp_reply.protosrc = <IP of requested mac-associated machine>
                arp_reply.protodst = packet.next.protosrc
                ether = ethernet()
                ether.type = ethernet.ARP_TYPE
                ether.dst = packet.src
                ether.src = <requested mac address>
                ether.payload = arp_reply
                #envie esse pacote para o switch
                #veja a sessão abaixo acerca desse tócico.
            elif packet.payload.opcode == arp.REPLY:
                print "It's a reply; do something cool"
            else:
                print "Some other ARP opcode, probably do something smart here"
```

Veja o componente l3_learning para um exemplo completo do uso so controlador para tratar requisições ARP e gerar respostas.


Threads, Tasks, and Timers: pox.lib.recoco


Esse é um grande tópico, e muito pode ser dito. Sinta-se à vontade para adicionar algo!


Há um pequeno número de exemplos em pox/lib/recoco/examples.py


Executabdo código no futuro usando um Timer


É frequantemente útil ter um trecho de código que execute de tempos em tempos. Por exemplo, você pode querer examinar os bytes tranferidos de um fluxo específico a cada 10 segundos. Também é um caso comum quando você quer qeu algo ocorra em um tempo específico no futuro; por exemplo, se você envia uma barreira, você pode querer desconectar o switch se 5 segundos se passam sem que a resposta à barreira apareça. Essa é um tipo de tarefa que a classe  pox.lib.recoco.Timer é designada a tratar.- executando um trecho de código em um único ou recorrente tempo no futuro
  
Note: O método callDelayed() do objeto POX core é uma forma mais fácil de setar um simples temporizador. (veja exemplo abaixo)  


Argumentos do construtor de Timer


Argumento
	Tipo- padrão
	Significado
	timeToWake
	Números (segundos)
	Quantidade te tempo à se esperar antes de invocar o callback (absoluteTime=False), ou tempo específico para chamar o callback (absoluteTime=False)
	callback
	callable (i.e., função)
	Uma função para chamar quando o tempo acaba
	absoluteTime
	Boolean- False
	quando falso, timeToWake é um número em segundos no futuro. Quando verdadeiro, timeToWake é um tempo específico no futuro (e.g., um número de segundos desde o tempo reportado por meio de time.time()). Note que absoluteTime=True não pode ser usado em intervalos recorrentes.
	recurring
	Boolean- False
	Quando falso o temporizador online dispara uma vez- timeTowake segundos desde o começo. Quando verdadeiro, o temporizador dispara todo  timeToWake segundos. 
	args, kw
	sequence, dict - vazio
	Esses são argumentos de palavras chave passados para o callback
	scheduler
	Scheduler - None
	O scheduler com que esse temporizador é executado. None significa usar o padrão (você quer isso).
	started
	Boolean- True
	Se verdadeiro o temporizador começa automaticamente 
	selfStoppable
	Boolean- True
	Se verdadeiro, o callback de um temporizador recorrente pode retornar False para cancelar o temporizador.
	

Métodos de Timer


Nome 
	Argumentos
	Significado
	calcel
	Nenhum
	para o temporizador (não chama o callback novamente)
	

***Exemplo - Temporizador único***

```
from pox.lib.recoco import Timer


def handle_timer_elapse (message):
  print "I was told to tell you:", message


Timer(10, handle_timer_elapse, args = ["Hello"])


# Imprime "I was told to tell you: Hello" em 10 segundos


# Modo alternativo para temporizadores simples:
from pox.core import core # Many components already do this
core.callDelayed(10, handler_timer_elapse, "Hello") # You can just tack on args and kwargs.

```


***Exemplo - Temporizador Recorrente***

```
# Simula uma longa viagem


from pox.lib.recoco import Timer


we_are_there = False


def are_we_there_yet ():
  if we_are_there: return False # Cancels timer (see selfStoppable)
  print "Are we there yet?"


Timer(30, are_we_there_yet, recurring = True)
```

###Trabalhando com sockets: ioworker


pox.lib.ioworker contém uma API de alto nível para trabalhar com sockets assincronos em POX. envios são dispare-e-esqueça (fire-and-forget), dados recebidos são buferizados e um callback é disparado quando há algo disponível, etc. 


TODO: Documentation and samples



<a id = "of"></a>
##OpenFlow no POX

Uma das principais intenções do uso do POX é o desenvolvimento de aplicações de controle OpenFlow. Neste capítulo, descrevemos algumas das características e interfaces do POX que facilitam isso.   

O componente do POX que realmente se comunica com os interruptores de OpenFlow  é o openflow.of_01 (o 01 refere-se ao facto de que este componente se comunica pelo protocolo físico OpenFlow 0x01). Meu padrão, of_01 registra um OpenFlow "nexus" sobre o objeto do núcleo como "OpenFlow". Esta é a principal interface para trabalhar com OpenFlow em POX - você pode usar este nexo OpenFlow para enviar comandos para interruptores e receber mensagens de interruptores (na forma de eventos levanta - consulta a subseção abaixo).  

Sempre quando um switch se conecta ao POX, também há um objeto de conexão associado. Existe muita sobreposição entre as conexões e o Nexus. Qualquer um deles pode ser usado para enviar uma mensagem a um switch, e a maioria dos eventos são gerados em ambos. Às vezes é mais conveniente usar um ou o outro. Se a sua aplicação está aplicada à eventos de todos os switches, faz sentido ouvir ao Nexus, o qual gera eventos para todos os switches. Se você somente está interessado em um único switch,  pode fazer sentido ouvir a uma conexão específica.  

Existem três principais formas de se obter uma referência a um objeto de conexão:  

1. Você pode ouvir aos eventosConnectionUp sobre o nexus - isto passa o novo objeto de conexão à frente 
2. Você pode usar o método getConnection método do nexus (<DPID>) para encontrar uma conexão por DPID do switch
3. Você pode enumerar todas as conexões do nexo de sua propriedade através de ligações (por exemplo, para con em core.openflow.connections) Eventos 

###OpenFlow: Respondendo a Switches 

Nota: Para mais informação sobre o sistema de eventos em POX, consulte a secção relevante deste manual.

A maioria dos eventos relacionados OpenFlow são criados em resposta direta a uma mensagem recebida de um interruptor. Como orientação geral, eventos OpenFlow relacionadas têm os três atributos seguintes: 

Descrição do tipo de atributo
Conexão com o switch relevante (por exemplo, que enviou a mensagem a qual  evento corresponde a)
dpid
ID Datapath longo do switch relevante (usar dpid_to_str para o formato)
ofp 
subclasse ofp_header
 objeto de mensagem OpenFlow que causou este evento. Veja Mensagens OpenFlow para obter informações sobre esses objetos.

No restante desta seção, descrevemos alguns dos eventos fornecidos pelo módulo OpenFlow e módulo de topologia. Para começar, aqui está uma forma muito simples componente POX que ouve os eventos ConnectionUp de todas as opções, e registra uma mensagem quando ele ocorre. Você pode colocar isso em um arquivo (por exemplo, ext / connection_watcher.py) e executá-lo (com. ./pox.py connection_watcher) e assistir as conexões dos switches.   

```
from pox.core import core 
from pox.lib.util import dpid_to_str 

log = core.getLogger()  

class MyComponent (object):
  def __init__ (self): 
    core.openflow.addListeners(self)

  def _handle_ConnectionUp (self, event): 
    log.debug("Switch %s has come up.", dpid_to_str(event.dpid))  
  
  def launch ():  
    core.registerNew(MyComponent) 
```

####ConnectionUp

Definição da classe do evento:

```
  class ConnectionUp (Event):
  def __init__ (self, connection, ofp):
    Event.__init__(self)
    self.connection = connection
    self.dpid = connection.dpid
    self.ofp = ofp
```

Evento levantado quando a conexão a um switch OpenFlow tenha sido estabelecida.

Examinando os atributos:  notas do tipo de atributo:
Conexão

Este objeto pode ser usado para comunicação ao switch. Por exemplo, você pode usar isto para  enviar comandos OpenFlow, ou você pode ouvir aos eventos criados pelo próprio objeto de conexão disposto para mensagens aleatórias enviadas pelo switch	

dpid integer/long
Este é o DPID associado ao switch. Use util.dpidToStr() para mostrar isto.
 ofp_switch_features OFP Contém informações sobre o switch, por exemplo, tipos de ação suportados (por exemplo, se o campo para  reescrever está disponível), e as informações de porta (por exemplo, endereços MAC e nomes). ( Isto também está disponível no atributo de características conexão.)

Este evento pode ser tratado como mostrado a seguir: 

```
def _handle_ConnectionUp (self, event):
    print "Switch %s has come up." % event.dpid
```

####ConnectionDown

Definição da classe do evento:  

```
class ConnectionDown (Event):
  def __init__ (self, connection, ofp):
    Event.__init__(self)
    self.connection = connection
    self.dpid = connection.dpid
```

Event raised when the connection to an OpenFlow switch has been lost.  

####PortStatus 

Definição da classe do evento: 

```
 class PortStatus (Event):
  def __init__ (self, connection, ofp):
    Event.__init__(self)
    self.connection = connection
    self.dpid = connection.dpid
    self.ofp = ofp
    self.modified = ofp.reason == of.OFPPR_MODIFY
    self.added = ofp.reason == of.OFPPR_ADD
    self.deleted = ofp.reason == of.OFPPR_DELETE
    self.port = ofp.desc.port_no
```

Fired in response to port status changes and the code to handle looks like this: 

```
def _handle_PortStatus (self, event):
    if event.added:
        action = "added"
    elif event.deleted:
        action = "removed"
    else:
        action = "modified"
    print "Port %s on Switch %s has been %s." % (event.port, event.dpid, action)
```

####FlowRemoved

Definição da classe do evento:  

```
class FlowRemoved (Event):
  def __init__ (self, connection, ofp):
    Event.__init__(self)
    self.connection = connection
    self.dpid = connection.dpid
    self.ofp = ofp
    self.idleTimeout = False
    self.hardTimeout = False
    self.deleted = False
    self.timeout = False
    if ofp.reason == of.OFPRR_IDLE_TIMEOUT:
      self.timeout = True
      self.idleTimeout = True
    elif ofp.reason == of.OFPRR_HARD_TIMEOUT:
      self.timeout = True
      self.hardTimeout = True
    elif ofp.reason == of.OFPRR_DELETE:
      self.deleted = True
```

Gerado quando uma entrada de fluxo foi removida a partir de uma mesa de fluxo. Isto pode até ser devido a um tempo de espera ou porque foi removido explicitamente. Essas notificações são enviadas quando o fluxo foi instalado com o sinalizador OFPFF_SEND_FLOW_REM. Veja a especificação OpenFlow para mais detalhes.

idleTimeout (bool) – True se expirada por causa de ociosidade 
hardTimeout (bool) - True se expirada por causa do tempo limite rígido 
timeout (bool) – True se qualquer um dos dois for True 
deleted (bool) - True se deletado explicitamente 

####Statistics Events 

```
class RawStatsReply (Event):
  def __init__ (self, connection, ofp):
    self.connection = connection
    self.ofp = ofp     # Raw ofp message(s)
 
class StatsReply (Event):
  def __init__ (self, connection, ofp, stats):
    Event.__init__(self)
    self.connection = connection
    self.ofp = ofp     # Raw ofp message(s)
    self.stats = stats # Processed
 
class SwitchDescReceived (StatsReply):
  pass
 
class FlowStatsReceived (StatsReply):
  pass
 
class AggregateFlowStatsReceived (StatsReply):
  pass
 
class TableStatsReceived (StatsReply):
  pass
 
class PortStatsReceived (StatsReply):
  pass
 
class QueueStatsReceived (StatsReply):
  pass
```

Embora se possa escutar mensagens estatísticas primas, isso não é particularmente conveniente assim como  você pode verificar o tipo de estatística que está segurando, e você pode precisar manualmente montar grupos completos de estatísticas porque um único pedido de estatísticas  deve ser respondido em várias partes. É geralmente muito mais fácil ouvir eventos específicos que são subclasses de StatsReply, por exemplo, FlowStatsReceived. Ao manusear esses eventos StatsReply base, a propriedade estatísticas irá conter um conjunto completo de estatísticas (por exemplo, uma matriz de ofp_flow_stats). Veja a seção sobre ofp_stats_request para mais informações.
 
####PacketIn 
Definição do evento:  

```
class PacketIn (Event):
  def __init__ (self, connection, ofp):
    Event.__init__(self)
    self.connection = connection
    self.ofp = ofp
    self.port = ofp.in_port
    self.data = ofp.data
    self._parsed = None
    self.dpid = connection.dpid
```

Fired in response to PacketIn events 

port (int) – número da porta de qual o pacote veio 
data (bytes) – dados do pacote brutos 
parsed (packet subclasses) – Versão analisada do pox.lib.packet 
ofp (ofp_packet_in) – Mensagem OpenFlow que causou este evento 
connection – Conexão para o switch a partir do qual este evento se originou

####ErrorIn
Definição do evento: 

```
class BarrierIn (Event):
  def __init__ (self, connection, ofp):
    Event.__init__(self)
    self.connection = connection
    self.ofp = ofp
    self.dpid = connection.dpid
    self.xid = ofp.xid
```

Disparado respondendo a uma resposta de barreira. 

###Mensagens OpenFlow

Mensagens OpenFlow são o como como os switches se comunicam com controladores. As mensagens são definidas na especificação do OpenFlow. Há múltiplas versões dessa especificação; POX atualmente suporta a versão 1.0.0 do OpenFlow (versão de protocolo cabeado 0x01).


O POX cotém classes e constantes correspondendo a elementos do protocolo OpenFlow, e estão definidos no arquivo pox/openflow/libopenflow_01.py (o 01 refere-se a versão do protocolo). Na maior parte da vezes, os nomes são os mesmos que estão na especificação. Em algumas instâncias, POX tem nomes que nós julgamos ser melhores. Adicionalmente, POX define algumas classes que não correspondem a estruturas específicas da especificação (s espefificação não descreve estruturas que são somente cabeçalhos planos OpenFlow somente diferenciados pelo tipo de atributo de mensagem- O POX sim). Logo, você pode querer consultar a especificação do Openflow em adição à esse documento (e, claro, ao código do POX e a refeência do pydoc/Sphinx ) 



Um aspecto legal do da biblioteca OpenFlow do POX é que muitos campos possuem valores padrão ou podem inferir alguns valores.

Nas seguintes subsessões, nós discutiremos um subconjunto da interface OpenFlow do POX. 


TODO: Redo following sections to have tables of values/types/descriptions rather than snippets from init functions.


ofp_packet_out - Enviando pacotes apartir de um switch
O propósito principal dessa mensagem é instruir um switch a enviar um pacote (ou enfileirá-lo). Contudo, isso pode ser também útil como forma de instruir um switch para descartar um pacotes presente em seu buffer (simplesmente não especificando qualquer ação).  


<table>
<tr><th>atributo</th> <th>tipo</th> <th>padrão</th> <th>nota</tr>

<tr><th>buffer_id</th> <th>int/None</th> <th>None</th> <th>ID do buffer ao qual o pacote está armazenado no switch. Se você está não está reenviando um buffer por meio de ID, use None.</th></tr>

<tr><th>in_port</th> <th>int</th> <th>OFPP_NONE</th> <th>Prta do switch que opacote chega se reenviando um pacote.</th></tr>

<tr><th>actions</th> <th>lista de ofp_action_XXXX </th> <th>[ ]</th> <th>Se você tiver um único ítem, você tmabém pode especificar isso usando o parâmetro chamado "action" do inicializador.</th></tr>

<tr><th>data</th> <th>bytes / ethernet / ofp_packet_in</th> <th>''</th> <th>O dado a ser enviado ( ou nenhum se estiver enviando um beffer já existente pelo seu buffer_id).
Se você especificar um ofp_packet_in para isso, in_port, buffer_id e o dado será enviado corretamente- esse é o meio mais fácil de reenviar um pacote.</th></tr>
</table>

Se você receber um ofp_packet_in e deseja reenviá-lo, você pode simplesmente usar isso como o atributo do dado. 

Veja a sessão de 5.3.6 para a especificação 1.0 do OpenFlow. Essa classe é definida em pox/openflow/libopenflow_01.py. 



**ofp_flow_mod - Flow table modification**

```
class ofp_flow_mod (ofp_header):
  def __init__ (self, **kw):
        ofp_header.__init__(self)
        self.header_type = OFPT_FLOW_MOD
        if 'match' in kw:
          self.match = None
        else:
          self.match = ofp_match()
        self.cookie = 0
        self.command = OFPFC_ADD
        self.idle_timeout = OFP_FLOW_PERMANENT
        self.hard_timeout = OFP_FLOW_PERMANENT
        self.priority = OFP_DEFAULT_PRIORITY
        self.buffer_id = None
        self.out_port = OFPP_NONE
        self.flags = 0
        self.actions = []
```

* cookie (int) - identificador para essa regra de fluxo. (opcional)
* command (int) - Um dos seguintes valores:
  * OFPFC_ADD - adiciona uma regra ao switch (padrão)
  * OFPFC_MODIFY - modifica qualquer regra combinante
  * OFPFC_MODIFY_STRICT - modifica regras com valores wildcards estritos
  * OFPFC_DELETE - deleta todas as regras combinantes
  * OFPFC_DELETE_STRICT - deleta regras com valores wildcards estritos
* idle_timeout (int) - regra irá expirar se não recebe qualquer pacote qeu combine com ela em 'idle_timeout' segundos. Um valor OFP_FLOW_PERMANENT significa que não há qualquer idle_timeout (o padrão).
* hard_timeout (int) - regra expirará após'hard_timeout' segundos. Um valor OFP_FLOW_PERMANENT quer dizer que ela nunca expirará (o padrão)
* priority (int) - a prioridade com que a regra combinará, quanto maior maior a prioridade. Note: combinações exatas terão maiores prioridades.
* buffer_id (int) - um buffer no switch que a nova regra será aplicada. Use None para nenhum. Sem significado quando deleta-se regras.
* out_port (int) - esse campo é usado para combinar comando de deletar. OFPP_NONE pode ser usando para indicar que não háqualquer restrição.
* flags (int) - um dos seguintes valores:
  * OFPFF_SEND_FLOW_REM - envia uma mensagem de regra removida ao controlador quando a regra expira
  * OFPFF_CHECK_OVERLAP - checa se há sobreposição de regras ao instalar. Se ouver, um erro é enviado ao controlador
  * OFPFF_EMERG - Considere isso como uma regra emergencial e só use quando a conexão com o swuitch é perdida
* actions (list) - ações são definidas abaixo, cada objeto de açao desejado é adicionado a lista e são executados em ordem
* match (ofp_match) - a estrutura de combinação para as regras (veja abaixo)

Veja a sessão de 5.3.6 para a especificação 1.0 do OpenFlow. Essa classe é definida em pox/openflow/libopenflow_01.py linha 1831.


**Exemplo: Instalando uma entrada na tabela**

```
# Tráfego para 192.168.101.101:80 deve ser direcionado para a porta 4 do switch

# uma coisa de cada vez...

msg = of.ofp_flow_mod()
msg.priority = 42
msg.match.dl_type = 0x800
msg.match.nw_dst = IPAddr("192.168.101.101")
msg.match.tp_dst = 80
msg.actions.append(of.ofp_action_output(port = 4))
self.connection.send(msg)


# mesma coisa, mas com uma só linha...
self.connection.send( of.ofp_flow_mod( action=of.ofp_action_output( port=4 ),
                                           priority=42,
                                           match=of.ofp_match( dl_type=0x800,
                                                               nw_dst="192.168.101.101",
                                                               tp_dst=80 )))
```



**Exemplo: Limpando todas as tabelas de todos os switches**

```
# criar uma mensagem ofp_flow_mod message para deletar todas as regras
# (note que essa combinação de flow_mods combinar com todos os fluxos por padrão)
msg = of.ofp_flow_mod(command=of.OFPFC_DELETE)


# itera sobre todos os switches conectados e deleta todas as regras
for connection in core.openflow.connections: # _connections.values() before betta
  connection.send(msg)
  log.debug("Clearing all flows from %s." % (dpidToStr(connection.dpid),))
ofp_stats_request - Requesting statistics from switches
class ofp_stats_request (ofp_header):
  def __init__ (self, **kw):
        ofp_header.__init__(self)
        self.header_type = OFPT_STATS_REQUEST
        self.type = None # Try to guess
        self.flags = 0
        self.body = b''
```

* type (int) - o tipo de requisição de status (e.g., OFPST_PORT).  Padrão é tentar adicinhar baseado no corpo.
* flags (int) - Nenhuma flag é definida no OpenFlow 1.0.
* body (flexible) - O corpo da requisição de status.  Isso pode ser um objeto puro em bytes, ou uma classe de empacotamento(e.g., ofp_port_stats_request).

Veja a sessão 3.5.5 da especificação do OpenFlow para maiores informaçoes acerca dessa estrutura e de cada tipo estatística individualmente (port stats, flow stats, aggregate flow stats, table stats, etc.). Essa classe é definida em pox/openflow/libopenflow_01.py


TODO: Show some of the individual stats request/reply types?


**Exemplo - Estatísticas de fluxo Web**


Requisita a tabela de fluxo de um switch e mostra informaçoes sobre tráfego web. Esse exemplo  foi feito para rodar em conjunto, digamos, com um componente orwarding.l2_learning component. Ele pode ser colado dentro do interpretador interativo do POX.

Veja evetos de estatísticas para mais informações.

```
import pox.openflow.libopenflow_01 as of
log = core.getLogger("WebStats")


`# Quando tivermos status do fluxo, imprima tudo
`

def handle_flow_stats (event):
  web_bytes = 0
  web_flows = 0
  for f in event.stats:
        if f.match.tp_dst == 80 or f.match.tp_src == 80:
          web_bytes += f.byte_count
          web_flows += 1
  log.info("Web traffic: %s bytes over %s flows", web_bytes, web_flows)


`# Escutar status de fluxo
`

core.openflow.addListenerByName("FlowStatsReceived", handle_flow_stats)


`# Requisitar de fato status de fluxo de todos os switches
`

for con in core.openflow.connections: # make this _connections.keys() for pre-betta
  con.send(of.ofp_stats_request(body=of.ofp_flow_stats_request()))
```

###Estrutura de combinação


OpenFlow define uma estrutura de combinação – ofp_match – que permite a você definir um conjunto de cabeçalhos que serão combinados. Você pode tanto criar do zero ou usar um facory method para criar baseado em um pacote pacote já existente.


A estrutura de combinação é definida em pox/openflow/libopenflow_01.py na classe ofp_match.  Seus atributos são derivados dos membros existentes na especificação do OpenFlow, então dirija-se à ela para maiores informações. Ainda assim, ela está resumida na tabela abaixo:


Atributos de ofp_match :


Attributo    Significado
in_port    Porta do switch que o pacotes chegou
dl_src    Endereço de origem Ethernet
dl_dst    Endereço de destino Ethernet
dl_vlan    VLAN ID
dl_vlan_pcp    VLAN prioridade
dl_type    Ethertype / tamanho (e.g. 0x0800 = IPv4)
nw_tos    IP TOS/DS bits
nw_proto    IP protocolo (e.g., 6 = TCP) or lower 8 bits of ARP opcode
nw_src    Endereço de origem IP
nw_dst    Endereço de destino IP
tp_src    Porta de origem TCP/UDP
tp_dst    Porta de destino TCP/UDP


Atributos podem ser especificados tanto sobre um objeto como durante sua inicialização.  Isto é, os seguintes são equivalentes:

```
my_match = of.ofp_match(in_port = 5, dl_dst = EthAddr("01:02:03:04:05:06"))
`#.. ou ..`
my_match = of.ofp_match()
my_match.in_port = 5
my_match.dl_dst = EthAddr("01:02:03:04:05:06")
```

Campos não especificados são tratados como wildcards e vão combinar com qualquer pacote. Você pode explicitamente setar um campo para ser wildcard setando o mesmo como None.


Enquanto a estrutura ofp_match do OpenFlow é definida como tendo atributos wildcards, você provavelmente nunca precisará setar isso quando estiver usando o POX- simplesmente não atribua valores ao campo que você quer que seja wildcard (ou atribua None).
Os campos de endereço IP podem na verdade ser parcialmente wildcard, o que lhe permite combinar subredes inteiras. Há algumas formas de fazer isso. Aqui estão algumas equivalente:


my_match.nw_src = "192.168.42.0/24"
my_match.nw_src = (IPAddr("192.168.42.0"), 24)
my_match.nw_src = "192.168.42.0/255.255.255.0"


Como exemplo, o código seguinte criará uma combinação para tráfego direcionado a servidor web:

```
import pox.openflow.libopenflow_01 as of # POX convention
import pox.lib.packet as pkt # POX convention
my_match = of.ofp_match(dl_type = pkt.ethernet.IP_TYPE, nw_proto = pkt.ipv4.TCP_PROTOCOL, tp_dst = 80)
```

###Define uma combinação partindo de um pacote existente

Há um jeito simples de criar uma combinação baseada em um objeto de pacotes existe (isto é, um objeto ethernet da pox.lib.packet) ou de um ofp_packet_in existente. Isso é feito usando o factory method ofp_match.from_packet().

```
my_match = ofp_match.from_packet(packet, in_port)
```

O parâmetro de pacote é um pacote passado ou ofp_packet_in para o qual cria-se a combinação, e o parâmetro in_port é a porta do switch que você quer combinar (ou deixe não especificado para combinar com todas as portas do switch).

Note que você pode agora setar campos do objeto de combinação resultante para None caso queira uma combinação menos exata.


###Ações OpenFlow

OpenFlow actions are applied to packets that match a rule installed at the datapath. The code snippets found here can be found in libopenflow_01.py in pox/openflow.


Output
Forward packets out of a physical or virtual port. Physical ports are referenced to by their integral value, while virtual ports have symbolic names. Physical ports should have port numbers less than 0xFF00.


Structure definition:


class ofp_action_output (object):
  def __init__ (self, **kw):
        self.port = None # Purposely bad -- require specification
port (int) the output port for this packet. Value could be an actual port number or one of the following virtual ports:
OFPP_IN_PORT - Send back out the port the packet was received on.  Except possibly OFPP_NORMAL, this is the only way to send a packet back out its incoming port.
OFPP_TABLE - Perform actions specified in flowtable. Note: Only applies to ofp_packet_out messages.
OFPP_NORMAL - Process via normal L2/L3 legacy switch configuration (if available – switch dependent)
OFPP_FLOOD - output all openflow ports except the input port and those with flooding disabled via the OFPPC_NO_FLOOD port config bit (generally, this is done for STP)
OFPP_ALL -  output all openflow ports except the in port.
OFPP_CONTROLLER - Send to the controller.
OFPP_LOCAL - Output to local openflow port.
OFPP_NONE - Output to no where.
Enqueue
Forwards a packet through the designated queue to implement rudimentary QoS behavior. See section of 5.2.2 of the OpenFlow spec.


class ofp_action_enqueue (object):
  def __init__ (self, **kw):
        self.port = 0
        self.queue_id = 0
port (int) - must be a physical port
queue_id (int) - specific queue id
Note that definition of queues is not a part of OpenFlow and is switch-specific.


Set VLAN ID
If the packet doesn't have a VLAN header, this adds one and sets its ID to the specified value and its priority to 0.  If the packet already has a VLAN header, this just changes its ID.


class ofp_action_vlan_vid (object):
  def __init__ (self, **kw):
        self.vlan_vid = 0
vlan_vid (int) - the ID to set the vlan id to (< 4094, of course)
Set VLAN priority
If the packet doesn't have a VLAN header, this adds one and sets its priority to the specified value and its ID to 0.  If the packet already has a VLAN header, this just changes its priority.


class ofp_action_vlan_pcp (object):
  def __init__ (self, **kw):
        self.vlan_pcp = 0
vlan_pcp (short) - the priority to set the packet to (< 8)
Set Ethernet source or destination address
Used to set the source or destination MAC (Ethernet) address.


class ofp_action_dl_addr (object):
  @classmethod
  def set_dst (cls, dl_addr = None):
        return cls(OFPAT_SET_DL_DST, dl_addr)
  @classmethod
  def set_src (cls, dl_addr = None):
        return cls(OFPAT_SET_DL_SRC, dl_addr)


  def __init__ (self, type = None, dl_addr = None):
        self.type = type
        self.dl_addr = EMPTY_ETH
type (int) - either OFPAT_SET_DL_SRC or OFPAT_SET_DL_DST
dl_addr (EthAddr) - the mac address to set.
It may be convenient to use the two class factory methods rather than directly creating an instance of this class.  For example, to create an action to rewrite the destination MAC address, you can use:


action = ofp_action_dl_addr.set_dst(EthAddr("01:02:03:04:05:06"))
Set IP source or destination address
Used to set the source or destination IP address.


class ofp_action_nw_addr (object):
  @classmethod
  def set_dst (cls, nw_addr = None):
        return cls(OFPAT_SET_NW_DST, nw_addr)
  @classmethod
  def set_src (cls, nw_addr = None):
        return cls(OFPAT_SET_NW_SRC, nw_addr)


  def __init__ (self, type = None, nw_addr = None):
        self.type = type
        if nw_addr is not None:
          self.nw_addr = IPAddr(nw_addr)
        else:
          self.nw_addr = IPAddr(0)
type (int) - either OFPAT_SET_NW_SRC or OFPAT_SET_NW_DST
nw_addr (IPAddr) - the IP address to set
As with MAC addresses, rather than constructing an instance of this class directly, it can be convenient to use the set_src() and set_dst() factory methods:


action = ofp_action_nw_addr.set_dst(IPAddr("192.168.1.14"))
Set IP Type of Service
Set the TOS field of an IP packet.


class ofp_action_nw_tos (object):
  def __init__ (self, nw_tos = 0):
        self.nw_tos = nw_tos
nw_tos (short) - the tos of service to set.
Set TCP/UDP source or destination port
Set the source or desintation TCP or UDP port.


class ofp_action_tp_port (object):
  @classmethod
  def set_dst (cls, tp_port = None):
        return cls(OFPAT_SET_TP_DST, tp_port)
  @classmethod
  def set_src (cls, tp_port = None):
        return cls(OFPAT_SET_TP_SRC, tp_port)


  def __init__ (self, type=None, tp_port = 0):
        self.type = type
        self.tp_port = tp_port
type (int) - must be either OFPAT_SET_TP_SRC or OFPAT_SET_TP_DST
tp_port (short) - the port value to set (< 65534)
As with the MAC and IP addresses, it may be convenient to use the two factory methods (set_dst() and set_src()) rather than explicitly creating instances of this class.


###Comunicando-se com um Datapath (Switch)

Coneções aos switches ocorrem no POX sob a forma de objetos Connection. Objetos Connection tem uma variedade de eventos que vocês podem escutar para ser avisado de eventos relativos a um switch em particular. (em muitos casos, há os mesmos eventos no objeto core.openflow, exceto aqueles aventos disparados por eventos em qualquer switch). Adicionalmente, ele pode pode ser usado para envio de mensagens aos switches.


Há múltiplas formas de se acessar um objeto Connection. Uma forma comum é tomar proveito do fato de que, quando um switch se conecta, um evento ConnectionUp é disparado em core.openflow - você pode simplesmente salvar o objeto Connection em sua prória variável:


```
class MyClass (object):
        def __init__ (self):
            self.connections = set()
            core.openflow.addListeners(self)


        def _handle_ConnectionUp (self, event):
            self.connections.add(event.connection) # Ver o evento ConnectionUp

```

Se você souber o DPID do switch,você pode o extrair de core.openflow usando o método getConnection(). Por último, você pode iterar sobre core.openflow.connections, que é uma lista de todas as conexões ativas.
 

Note: se você só quer enviar uma mensagem ao swutch e você sabe o seu DPID, você pode usar core.openflow.sendToDPID(dpid, msg). Isso é similar a fazer core.openflow.getConnection(dpid).send(msg).


####Objetos Connection


Uma vez que você tenha um objeto /connection, você pode usá-lo para escutar eventos específicos daquela(e) conexão/switch, e pode usar o seu método send() para enviar mensagens OpenFlow ao switch associado.

TODO: Discuss listening to events on Connection objects and other Connection attributes (e.g., .features).

Há muitos tipos de mensagens que você pode enviar a um switch- a especificação do OpenFlow contém uam lista completa. Vamos cobrir as seguintes: FlowMods e PacketOuts- abaixo:


####Exemplo: Enviando um FlowMod

Para enviar um flow mod você precisa definir uma estrutura de combinação (discutido acima) e setar alguns parâmetros específicos de flow mod como abaixo:


```
msg = ofp_flow_mod()
msg.match = match
msg.idle_timeout = idle_timeout
msg.hard_timeout = hard_timeout
msg.actions.append(of.ofp_action_output(port = port))
msg.buffer_id = <some buffer id, if any>
connection.send(msg)
```

Usando a variável de conexão obtida pelo switch conectado, nós podemos enviar um flow mod a ele.


####Exemplo: Enviando um PacketOut

De modo similar à um flow mod, é preciso primeiro definir um pacote de saída como mostrado:

```
msg = of.ofp_packet_out(in_port=of.OFPP_NONE)
msg.actions.append(of.ofp_action_output(port = outport))
msg.buffer_id = <some buffer id, if any>
connection.send(msg)
```

inport é setado como OFPP_NONE por que o pacote foi gerado no controlador e não originado como um pacote no switch.

<a id = "etc"></a>
##Aplicações de terceiros, tutoriais, Etc.



Essa sessão tem por objetivo destacar alguns projetos que fazem uso do POX mas que não são partes integralizantes dele. (Isso pode ser movido para uma página própria ou algo assim no furuto.)


POXDesk: Uma interface gráfica web para o POX


![](http://www.noxrepo.org/wp-content/uploads/2012/09/POXDesk4.png)


Esse é um projeto paralelo do Murphy em estado bem inicial. Ele roda na versão beta do POX e provê algumas funcionalidades: um inspetor de tabela de fluxo, um visualizador de log, um visualizador simples de topologia, um terminal, um switch l2 implementado em JavaScript, etc. É implementado usando o framework Qooxdoo JavaScript no front end e um servidor web e serviço de mensagens do  POX no backend. 




Site: https://github.com/MurphyMc/poxdesk/wiki


Blog com maiores informações e screenshots: http://www.noxrepo.org/2012/09/pox-web-interfaces/


Tutorial OpenFlow 


O tutorial OpenFlow tem uma versão do POX que guia o leitor através da instalação de um ambiente de testes usando Mininet e implementado um hub e um learning switch, dentre outras coisas.


Site: http://www.openflow.org/wk/index.php/OpenFlow_Tutorial


Tutorial OpenFlow Switch 
Essas são examinações de diferentes formas de implementar tipos variados de aplicações de switch em OpenFlow com o POX. Preparado por William Emmanuel Yu.


OpenFlow Switch Tutorial (of_sw_tutorial.py) - esse é um simples módulo OpenFlow comvárias implementações de switch. As seguintes implementações estão presentes:


Dumb Hub - Nessa implementação, todos os pacotes são enviados para o controlador e então são enviados para todas as portas. Nenhuma regra é instalada. 


Pair Hub - Regras são instaladas para endereços MAC de origem e destino de pares que instruem os pacotes a serem enviados por todas as portas. 


Lazy Hub - Uma única regra é instalada para enviar todos os pacotes para todas as portas.


Bad Switch - Aqui regras são instaladas somente baseadas no endereço MAC de destino. Encontre qual é o problema nisso.


Pair Switch - Instala regras baseadas em endereço MAC de origem e destino mas encaminha os pacotes para a porta apropriada ao invéz de inundar em todas.


Ideal Pair Switch - Melhoramento da implementação acima onde tanto regras para endereço MAC de origem e destino são instaladas. Por que isso é um melhoramento com relação ao anterior?
OpenFlow Switch Interactive Tutorial (of_sw_tutorial_oo.py) - O mesmo módulo acima mas interativo. Se rodado com uma linha de comando do POX. Usuários pode dinamicamente carregar e descarregar várias implementações de switch.




Exemplo de uma sessão de switch interativo
POX> INFO:openflow.of_01:[00-00-00-00-00-01 1] connected
POX> MySwitch.list_available_listeners()
INFO:samples.of_sw_tutorial_oo:SW_BADSWITCH
INFO:samples.of_sw_tutorial_oo:SW_LAZYHUB
INFO:samples.of_sw_tutorial_oo:SW_PAIRSWITCH
INFO:samples.of_sw_tutorial_oo:SW_IDEALPAIRSWITCH
INFO:samples.of_sw_tutorial_oo:SW_DUMBHUB
INFO:samples.of_sw_tutorial_oo:SW_PAIRHUB
POX> MySwitch.clear_all_flows()
DEBUG:samples.of_sw_tutorial_oo:Clearing all flows from 00-00-00-00-00-01.
POX> MySwitch.detach_packetin_listener()
DEBUG:samples.of_sw_tutorial_oo:Detaching switch SW_IDEALPAIRSWITCH.
POX> MySwitch.attach_packetin_listener('SW_LAZYHUB')
DEBUG:samples.of_sw_tutorial_oo:Attach switch SW_LAZYHUB.
POX> MySwitch.clear_all_flows()
DEBUG:samples.of_sw_tutorial_oo:Clearing all flows from 00-00-00-00-00-01.
POX> MySwitch.detach_packetin_listener()
DEBUG:samples.of_sw_tutorial_oo:Detaching switch SW_LAZYHUB.
POX> MySwitch.attach_packetin_listener('SW_BADSWITCH')
DEBUG:samples.of_sw_tutorial_oo:Attach switch SW_BADSWITCH.




OpenFlow Switch Tutorial for Betta (of_sw_tutorial_resend.py) - Mesmo módulo acima mas tem a vantagem de reenvio de funcionalidades disponíveis na versão betta.


Exemplo de Coletor de estatísticas
Preparado por William Emmanuel Yu.


Flow Statistics Collector Exemple (flow_stats.py) - Esse módulo coleta estatísticas a cada 5 segundos usando um temporizador. Há três formas de estatísticas coletadas: estatus da porta, estatísticas de fluxo e um mostrador de amostra de estatísticas web (similar a um exemplo abaixo). As estatísticas completas de portas e fluxo requerem o módulo pox.openflow.of_json presente na versão betta do POX.


<a id = "code"></a>
##Convenções de codificação

O guia de estilo de codificação para Python (The Style Guide for Python Code (AKA PEP 8)) destaca algumas convenções para desenvolvimento em Python. É uma boa base, porém POX não objetiva conformidades estritas. Aqui há algumas diretrizes para escrita de código em POX, especialmente se você gostaria de ter algo inserido no código fonte (merged). Note que emalguns casos elas são adições ou diferenciações à PEP 8. Observe também que a recomendação mais importante é que o código seja legível. Outra coisa é que o o repositório não é interiramente conforme com as diretrizes abaixo (envio de modificações (pull requests) são muito bem vindas!).

* Dois espaços para identação 
* Comprimento de linha
  * Máximo de 79 caracteres, no entanto 80 não vai nos matar.
  * Use juntadores de linha implícitos (e.g. "hanging" parentheses ou colchetes) ao invéz de do caracter de continuação de linha (\) explícitos, ao não ser qeu esse último seja muito estranho.
  * Continue linhas abaixo do parantêse/colchete apropriado (Estilo Lisp)  quando isso funcionar bem. Quando não se pode fazer isso, “minha” preferência é identar um “espaço único”, contudo eu sei que isso deixa muitos programadores de Pythn loucos, então sou exitante ao definir uma regra específica aqui, contanto que isso esteja claro. A regra básica é que deve haver identenção mais ou menos como o usual - i.e., não use dois espaços para linha contínua. Estou cada vez mais usando quatro espaços. 
* Duas linhas em branco separam partes de alto nível de código com significância estrutural (classes, funções de mais alto nível, etc.). Você pode usar três para separar porções grandes de código (contudo, quando alguém é tentado a fazer isso, é sempre uma boa idéia se perguntar se duas partes grandes de código deveriam estar em arquivos separados). Métodos dentro de classes devem ter uma ou duas, mas devem ser consistentes dentro da classe em particular.
* Colocar um espaço entre o nome da função e o abre parênteses da lisat de parâmetros. Similarmente para classe e super clase. Isso é: 
“def foo (bar):” e não “def foo(bar):”.
* Use somente novo estilo de classes. (isso quer dizer herdar de object.)
* Docstrings:
  * Ou atenha-se à """ one line """, ou tenha os abre e fecha """ nas linhas.
* Nomeando 
  * Classes devem geralmente ter Caixa-alta no início
  * Métodos e outros atributos devem ser caixa_baixa_com_underscores. Note que isso é atualemente violado por todo lado (no entanto está ficando melhor na versão betta).
  * Membros “privados” (que você não quer que outros mexam explicitamente ) devem começar com um underscore. 
  * “constantes” devem ser CAIXA_ALTA_COM_UNDERSCORES
  * A palavras chave para variável “catch-all” é chamada kw (contrariando a convensão do Python de kwargs) 

<a id = "faq"></a>
##FAQs


* Como posso mudar a porta Openflow 6633?


Se você ligar o modo verboso, você verá que é o módulo openflow.of_01 que escuta conexões. Essa é a dica: é esse componente que precisa ser configurado. Para fazer isso, basta passar uma “porta” como argumento para esse componente na linha de comando:

```
./pox.py openflow.of_01 --port=1234 <outros argumentos de linha de comando>
```

* Como posso ter algum componente iniciando automaticamente toda vez que rodar o POX?

A resposta curta é que não há um modo suportado para fazer isso. Contudo, é bem simples somente criar um pequeno componente que lança qualquer componente que você queira.


Por exemplo, digamos que esteja cansado de toda vez ter que se lembrar da seguinte linha de comando:

```
./pox.py log.level --DEBUG samples.pretty_log openflow.keepalive --interval=15 forwarding.l2_pairs
```

Escrevendo o seguinte componente em ext/startup.py, você pode substituir o que foi feito acima por simplesmente:

```
./pox.py startup
# me coloque em ext/startup.py

def launch ():
  from log.level import launch
  launch(DEBUG=True)


  from samples.pretty_log import launch
  launch()


  from openflow.keepalive import launch
  launch(interval=15) # 15 seconds


  from forwarding.l2_pairs import launch
  launch()

```


* Como posso fazer os switches enviarem um pacote com payload completo para o controlador?

Por padrão, quando um pacotes não se encaixa em nenhuma entrada na tabela de fluxo, ele é encaminhado para o controlador. No POX, esse padrão é OFP_DEFAULT_MISS_SEND_LEN,  que é 128. Isso é provavelmente suficiente para, e.g., ethernet, IP e cabeçalhos TCP... mas provavelmente não suficiente para pacotes completos. Se você quizer inspecionar payloads de pacotes completos, você tem duas opções:


1. Instalar uma entrada na tabela. Se você instala uma regra na tabela com packet_out to OFPP_CONTROLLER, POX fará com que o switch envie o pacote completo por padrão (você pode manualmente setar números menores de bytes se você quizer).
2. Mudar o tamanho do envio. Se você setar core.openflow.miss_send_len durante a inicialização (antes de todos os switches se conectarem), os switches devem enviar essa quantidade de bytes quando um pacote não combinar com nenhuma entrada na tabela.

Dê uma olhada no componente misc.packet_dump para um exemplo.


* Como posso estabelecer comunicação entre os componentes?


Componentes são simplesmente pacotes e módulos em Python, então um modo de fazer isso é do mesmo que você comunica entre qualquer módulo em Python - importe um deles e acesse suas variáveis de alto nível e funções.


POX também tem um mecanismo alternativo, que é melhor descrito na sessão Trabalhando com POX: O objeto POX Core. 


* Como posso usar o POX com o Mininet?


Use a função de controlador remoto. Por exemplo, se você está rodando POX na mesma máquina que Mininet:

```
mn --topo=linear --mac --controller=remote
```

Uma configuração comum, é rodar o Mininet em uma máquina virtual e o POX no seu ambiente de hospedeiro. Nesse caso, aponte o Mininet para o endereço IP da máquina hospedeira. Se você estiver usando o VirtualBox e um “Host-only Adapter”, esse é o endereço atribuído ao adaptador virtual no VirtualBox (i.e., vboxnet0). Por exemplo:

```
mn --topo=linear --mac --controller=remote --ip=192.168.56.1
```

* Estou vendo múltiplas mensagens de packet_in e encaminhamento não está funcionando; o que que ouve?

Esse problema é frequentemente visto ao usar um dos componentes de encaminhamento “learning” (l2_learning, l2_multi, etc.) em uma topologia em malha ou outra com um loop. Esses componentes de encaminhamento não tentam trabalhar em uma topologia com loops. 

O componente spanning_tree tem como objetivo ser uma solução genérica para esse problema, logo, você pode tentar executá-lo também. Isso pode ser útil também para evitar broadcasts até que o discovery tenha tempo para descobrir toda a topologia. Alguns componentes (assim como l2_learning) tem a opção de forçar isso.

* Estou usando o switch de referência (ou o Pantou) e os switches ficam se diconectando. Ajuda?

A resposta curta é que você rodou o componente openflow.keepalive também. Veja a descrição desse componente acima para maiores informações.


###Meu código não está funcionando! Podem me ajudar?


Possivelmente. Há algumas coisas que podem te fornecer uma auto ajuda. Se nenhum desses ítens funcionar ou se aplicar, há algumas coisas que você pode fazer para nos ajudar a te ajudar. Aqui há algumas coisas que você pode tentar: 


1. Leia os logs. Se os logs não parecem dizem algo útil, tente os ler em um nível mais baixo, como no nível DEBUG. Dessa forma, você receberá todos as mansagens de log. Faça  isso adicionando log.level --DEBUG à sua linha de comando. Veja a sessão do componente log.level para maiores informaçõessobre ajustes de níveis de log.

2. Observe o tráfego Openflow. Se você n]ao estiver recebendo qualquer evento qeu você acha que deveria, ou acha que está enviando ,mensagens mas os switches não parecem estar respondendo à elas, ou qualquer outra coisa onde você imagine que possa haver um ruptura na comunicação entre switches e o POX, pode lhe ser útil olhar o que de fato foi “jogado nos fios”.   
Há um OpenFlow dissector para o Wireshark (você pode ir atrás no Google). Você pode tanto rodar como de costume, ou pode usar o componente POX's openflow.debug para gerar traces sintéticos que mostram exatamente o que o POX pensa ter visto- uma mensagem por “pacote” sintético (o que torna mais fácil para o Wireshark ouvir) 

3. Execute uma versão mas nova. Particularmente, se você está executando uma versão de lançamento, você pode considerar usar a versãodo branch ativo. Enquanto branches ativos podem conter novos problemas, eles também consertam problemas antigos! Veja “Selecionando um Branch/ Versão” para maiores informações.

4. Procure na lista de email. Isso vai seria de melhor ajuda se ela não fosse tão bagunçada.
Desculpe por isso. Esperamos contronar os problemas e arrumar isso o quanto antes.

Se nada disso adiantar, você pode tentar postar na lista de discussão do pox-dev (se inscreva em http://www.noxrepo.org/community/mailing-lists/). Quando fizer isso,você provavelmente vai obter melhores resultados procedendo assim:


1. Poste a linha de comando com qual você chamou o POX.

2. Poste o log. Pode ser uma boa idéia postar em nível de DEBUG. Mesmo que você não tenha visto nada no log, pode ser útil para outra pessoa. A primeira parte do log (antes da mensagem de UP) é especialmente útil, uma vez que ela diz qual sistema opracional e interpretador você está usando, e informaçoesde demais componentes. Se você não postar isso, você pode tentar ao menos incluir algumas dessas informações por conta própria. 

3. Poste qual versão do POX você está usando. Você clonou do repositório http://github.org/noxrepo/pox, ou você alternou entre branches? Você fez isso recentemente ou você pode estar usando uma versão mais antiga? 

4. Poste qual tipo de switch você está usando. Você estpá rodando POX com o Mininet? Qual versão? Quais comandos você está usadno para envocar o Mininet?
Se você está rodando em switches de hardware, qual tipo? Se você está rodando em switches de sofware, qual e qual versão?

5. Poste código que ilustre o problema. Um exemplo mínimo está ótimo, mas qualquer coisa é melhor que nada.

6. Poste o que você já tentou até agora. Com sorte você mesmo pode já ter apontado o problema. O que você já tentou e qual foi o resultado?

Fazer o que foi dito acima, torna bem mais fácil para as pessoas te ajudarem e você potencialmente economiza tempo - se você não fizer o que foi dito acima, é bem provável que as primeiras sugestões que você receberá da lista será fazer algo que foi dito anteriomente aqui! 

