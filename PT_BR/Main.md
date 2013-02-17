# POX




* Instalando o POX
  * Requisitos
  * Adquirindo o Código
  * Selecionando um Branch / Versão
  * PyPy Supporte
    
   <br>
   
* Chamando o POX
<br>	
* Componentes do POX
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
    
  * Desenvolvendo seus próprios componentes
       * O diretório "ext"
       * A função de "envocar"
          * Um exemplo simples
          * Envocação múltipla
          
<br>

* APIs do POX
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
  
* OpenFlow no POX
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
  * comunicando com um Datapath (Switch)
       * Objetos Connection
       * Exemplo: Enviando um FlowMod
       * Exemplo: Enviando um PacketOut
       
<br>

* Ferramentas de terceiros, Tutoriais, Etc.
   * POXDesk: Uma interface gráfica web para o POX
   * Tutorial OpenFlow
   * Tutorial Switch OpenFlow
   * Exemplo de Coletor de estatísticas de fluxo
* Convenções de codificação

<br>

* FAQs
   * Como posso mudar a porta Openflow 6633?
   * Como posso ter algum componente iniciando automaticamente toda vez que rodar o POX?
   * Como posso fazer os switches enviarem um pacote com payload completo para o controlador?
   * Como posso estabelecer comunicação entre os componentes?
   * Como posso usar o POX com o Mininet?
   * Meu código não está funcionando! Podem me ajudar?



## Instalando o POX


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


##Components in POX
When we talk about components in POX, what we really mean is something that we can put on the POX command line as described in "Invoking POX".  In the following sections, we discuss some of the components that come with POX and how you can go about creating your own.


Stock Components
POX comes with a number of stock components.  Some of these provide core functionality, some provide convenient features, and some are just Exemplos.  To list a few:


py
This component causes POX to start an interactive Python interpreter that can be useful for debugging and interactive experimentation.  Before the betta branch, this was the default behavior (unless disabled with --no-cli).  Other components can add functions / values to this interpreter's namespace.


forwarding.l2_learning
This component makes OpenFlow switches act as a type of L2 learning switch.  This one operates much like NOX's "pyswitch" Exemplo, although the implementation is quite different.  While this component learns L2 addresses, the flows it installs are exact-matches on as many fields as possible.  For example, different TCP connections will result in different flows being installed.


forwarding.l2_pairs
Like l2_learning, this component also makes OpenFlow switches act like a type of L2 learning switch.  However, this one is probably just about the simplest possible way to do it correctly.  Unlike l2_learning, l2_pairs installs rules based purely on MAC addresses.


forwarding.l3_learning
This component is not quite a router, but it's also definitely not an L2 switch.  It's an L3-switchy-thing.  Perhaps the most useful aspect of it is that it serves as a pretty good example of using POX's packet library to examine and construct ARP requests and replies.


forwarding.l2_multi
This component is can still be seen as a learning switch, but it has a twist compared to the others.  The other learning switches "learn" on a switch-by-switch basis, making decisions as if each switch only had local information.  l2_multi uses openflow.discovery to learn the topology of the entire network: as soon as one switch learns where a MAC address is, they all do.


openflow.spanning_tree
This component uses the discovery component to build a view of the network topology, constructs a spanning tree, and then disables flooding on switch ports that aren't on the tree.  The result is that topologies with loops no longer turn your network into useless hot packet soup.


Note that this does not have much of a relationship to Spanning Tree Protocol.  They have similar purposes, but this is a rather different way of going about it.


The samples.spanning_tree component demonstrates this module by loading it and one of several forwarding components.


web.webcore
The webcore component starts a web server within the POX process.  Other components can interface with it to provide static and dynamic content of their own.


messenger
The messenger component provides an interface for POX to interact with external processes via bidirectional JSON-based messages.  The messenger by itself is really just an API, actual communication is implemented by transports.  Currently, transports exist for TCP sockets and for HTTP.


openflow.of_01
This component communicates with OpenFlow 1.0 (wire protocol 0x01) switches.  It is usually started by default (unless you specify the --no-openflow commandline option), but sometimes you want to invoke it manually so that you can change its options (e.g., which interface or port it listens on).


openflow.discovery
This component sends specially-crafted LLDP messages out of OpenFlow switches so that it can discover the network topology.  It raises events (which you can listen to) when links go up or down.


openflow.debug
Loading this component will cause POX to create pcap traces containing OpenFlow messages, which you can then load into Wireshark to analyze.  All the headers are synthetic so it's not totally a replacement for actually running tcpdump or Wireshark. It does, however, have the nice property that there is exactly one OpenFlow message in each frame (which makes it easier to look at!).


openflow.keepalive
This component causes POX to send periodic echo requests to connected switches.  Some switches will assume that an idle control connection indicates a loss of connectivity to the controller and will disconnect after some period of silence (often not particularly long), and this component can be used to prevent that.  (One could easily argue that if the switches want to disconnect when the connection is idle, it is their responsibility to send echo requests, but arguing won't fix the firmware.)


misc.pong
The pong component is a sort of silly example which simply watches for ICMP echo requests (pings) and replies to them.  If you run this component, all pings will seem to be successful!  It serves as a simple example of monitoring and sending packets and of working with ICMP.


misc.arp_responder
An ARP utility that can learn and proxy ARPs, and can also answer queries from a list of static entries.


misc.packet_dump
A simple component that dumps packet_in info to the log.  Sort of like running tcpdump on a switch.


misc.dns_spy
This component monitors DNS replies and stores their results.  Other components can examine them by accessing core.DNSSpy.ip_to_name[<ip address>] and core.DNSSpy.name_to_ip[<domain name>].


misc.of_tutorial
This component is for use with the OpenFlow tutorial.  It acts as a simple hub, but can be modified to act like an L2 learning switch.


misc.mac_blocker
This component is meant to be used alongside some other reactive forwarding applications, such as l2_learning and l2_pairs.  It pops up a Tkinter-based GUI that lets you block MAC addresses.  It demonstrates using higher-priority events to block PacketIns and making Tkinter-based GUIs in POX.


log
POX uses Python's logging system, and the log module allows you to configure a fair amount of this through the commandline. For example, you can send the log to a file, change the format of log messages to include the date, etc.


Disabling the Console Log
You can disable POX's "normal" log using:


`./pox.py log --no-default
`

Log Formatting
Please see the documentation on Python's LogRecord attributes for details on log formatting.  As a quick example, you can add timestamps to your log as follows:


`./pox.py log --format="%(asctime)s: %(message)s"

`Or with simpler timestamps:


`./pox.py log --format="[%(asctime)s] %(message)s" --datefmt="%H:%M:%S"`

See the samples.pretty_log component for another example (and, particularly, for an example that uses POX's color logging extension).


Log Output
Log messages are processed by various handlers which then print the log to the screen, save it to a file, send it over the network, etc.  You can write your own, but Python also comes with quite a few, which are documented in the Python reference for logging.handlers. POX lets you configure a lot of Python's built-in handlers from the commandline; you should refer to the Python reference for the arguments, but specifically, POX lets you configure:


Name    Type
stderr    StreamHandler for stderr stream
stdout    StreamHandler for stdout stream
File    FileHandler for named file
WatchedFile    WatchedFileHandler
RotatingFile    RotatingFileHandler
TimedRotatingFile    TimedRotatingFileHandler
Socket    SocketHandler - Sends to TCP socket
Datagram    DatagramHandler - Sends over UDP
SysLog    SysLogHandler - Outputs to syslog service
HTTP    HTTPHandler - Outputs to a web server via GET or POST
To use these, simply specify the Name, followed by a comma-separated list of the positional arguments for the handler Type.  For example, FileHandler takes a file name, and optionally an open mode (which defaults to append), so you could use:


`./pox.py log --file=pox.log

`
Or if you wanted to overwrite the file every time:

`./pox.py log --file=pox.log,w

`
You can also use named arguments by prefacing the entry with a * and then using a comma-separated list of key=value pairs.  For example:


`./pox.py log --*TimedRotatingFile=filename=foo.log,when=D,backupCount=5
`

log.color
The log.color module colorizes the log when possible.  This is actually pretty nice, but getting the most out of it takes a bit more configuration – you might want to take a look at samples.pretty_log.


Color logging should work fine out-of-the-box on Mac OS, Linux, and other environments with the real concept of a terminal.  On Windows, you need a colorizer such as colorama.


log.level
POX uses Python's logging infrastructure.  Different components each have their own loggers, the name of which is displayed as part of the log message.  Loggers actually form a hierarchy – you might have a "foo" logger with a "bar" sub-logger, which together would be known as "foo.bar".  Additionally, each log message has a "level" associated with it, which corresponds to how important (or severe) the message is.  The log.level component lets you configure which loggers show what level of detail.  The log levels from most to least severe are:


CRITICAL
ERROR
WARNING
INFO
DEBUG
POX's default level is INFO.  To set a different default (e.g., a different level for the "root" of the logger hierarchy):


`./pox.py log.level --WARNING
`

If you are trying to debug a problem with OpenFlow connections, however, you may want to turn up the verbosity of OpenFlow-related logs.  You can adjust all OpenFlow-related log messages like so:


`./pox.py log.level --WARNING --openflow=DEBUG
`

If this leaves you with too many DEBUG level messages from openflow.discovery which you are not interested in, you can then turn it down specifically:


`./pox.py log.level --WARNING --openflow=DEBUG --openflow.discovery=INFO
`

samples.pretty_log
This simple module uses log.color and a custom log format to provide nice, functional log output on the console.


tk
This component is meant to assist in building Tk-based GUIs in POX, including simple dialog boxes.  It is quite experimental.


Developing your own Components
This section tries to get you started developing your own components for POX.  In some cases, you might find that an existing component does almost what you want.  In these cases, you might start by making a copy of that component and working from there.


The "ext" directory
As discussed, POX components are really just Python modules.  You can put your Python code wherever you like, as long as POX can find it (e.g., it's in the PYTHONPATH environment variable).  One of the top-level directories in POX is called "ext".  This "ext" directory is a convenient place to build your own components, as POX automatically adds it to the Python search path (that is, looks inside it for additional modules), and it is excluded from the POX git repository (meaning you can easily check out your own repositories into the ext directory).


Thus, one common way to start building your own POX module is simply to copy an existing module (e.g., forwarding/l2_learning.py) into the ext directory (e.g., ext/my_component.py).  You can then modify the new file and invoke POX as ./pox.py my_component.


The launch function
While naming a loadable Python module on the commandline is enough to get POX to load it, a proper POX component should contain a launch function.  In the generic sense, a launch function is a function that POX calls to tell the component to initialize itself.  This is usually a function actually named launch, though there are exceptions.  The launch function is how commandline arguments are actually passed to the component.


A Simple Exemplo
The POX commandline, as mentioned above, contains the modules you want to load.  After each module name is an optional set of parameters that go with the module.  For example, you might have a commandline like:


./pox.py foo --bar=3 --baz --spam=disabled
Since the module name is foo, we have either a directory called foo somewhere that POX can find it that contains an __init__.py, or we simply have a foo.py somewhere that POX can find it (e.g., in the ext directory).  At the bare minimum, it might look like this:


def launch (bar, baz = "eggs", spam = True):
  print "foo:", bar, baz, spam
Note that bar has no default value, which makes the bar parameter not optional.  Attempting to run ./pox.py foo with no arguments will complain about the lack of a value for bar.  Notice that in the example given, bar receives the string value "3".  In fact, all arguments come to you as strings – if you want them as some other type, it is your responsibility to convert them.


The one exception to the "all arguments are strings" rule is illustrated with the baz argument.  It's specified on the commandline, but not given a value.  So what does baz actually receive in the launch() function?  Simple: It receives the Python True value.  (If it hadn't been specified on the commandline, of course, it would have received the string "eggs".)


Note that the spam value defaults to True.  What if we wanted to send it a false value – how would we do that?  We could try --spam=False, but that would just get us the string "False" (which if we tested for truthiness is actually Truthy!).  And if we just did --spam, that would get us True, which isn't what we want at all. This is one of those cases where you have to explicitly convert the value from a string to whatever type you actually want.  To convert to an integer or a floating point value, you could simply use Python's built-in int() or float().  For booleans, you could write your own code, but you might consider pox.lib.util's str_to_bool() function which is pretty liberal about accepting things like "on" or "true" or "enabled" as meaning True, and sees everything else as False.


Multiple Invocation
Now what if we were to try the following commandline?


`./pox.py foo --bar=1 foo --bar=2
`

You might expect to see:


foo: 1 eggs True
foo: 2 eggs True
Instead, however, you get an exception.  POX, by default, only allows components to be invoked once.  However, a simple change to your launch() function allows multiple-invocation:


`def launch (bar, baz = "eggs", spam = True, __INSTANCE__ = None):
  print "foo:", bar, baz, spam
`  
  
  
If you try the above commandline again, this time it will work.  Adding the __INSTANCE__ parameter both flags the function as being multiply-invokable, and also gets passed some information that can be useful for some modules that are invoked multiple times.  Specifically, it's a tuple containing:


The number of this instance (0...n-1)
The total number of instances for this module
True if this is the last instance, False otherwise (just a comparison between the previous two, but it's handy)
You might, for example, only want your component to do some of its initialization once, even if your component is specified multiple times.  You can easily do this by only doing that part of your initialization if the last value in the tuple is True.


You might also wish to examine the minimal component given in section "OpenFlow Events: Responding to Switches".  And, of course, check out the code for POX's existing components.


TODO: Someone should write a lot more about developing components.


##API’s do POX


POX contém um número de APIs para ajudá-lo a desenvolver aplicações de controle de rede. Aqui, nós tentamos descrever algumas delas. Isso certamente não é exaustivo, então sinta-se à vontade para contribuir.


Trabalhando com POX: O objeto POX Core


POX tem um objeto chamado “core”  que server como ponto central para muitas apis do POX. Algumas das funções que ele provê são só wrappers convenientes sobre outros funcionalidades e outras são únicas. Contudo, um outro principal propósito do core object é prover interoperabilidade entre componentes.  Frequentemente, ao invés de usar declarações de import para que um componente importe outro componente para que eles possam interagir, , componentes “registram” a si mesmos em core object e outros objetos requisitam o core object.
A maior vantagem dessa abordagem é que as dependendências entre os componentes são codificadas de forma fixa e diferentes componentes que possuem a mesma interface podem ser facilmente intercambiados. Pondo de outro modo, isso provê uma alternativa ao módulo de namespace normal do Python, o que torna mais fácil de manusiar.


Muitos módulos no POX vão querer acessar o core object. Por convensão, isso é feito importando core object, so seguinte modo:


`from pox.core import core
`

TODO: Write more about this!


Registrando componentes


como mencionado acima, pode ser conveniente para um componente “registrar” um objeto de provimento de API no core object. Um exemplo disso pode ser encontrado na implementação do OpenFlow - O componente POX OpenFlow por padrão registra uma instância de OpenFlowNexus como core.openflow, e outras aplicações pode então acessar muitas funcionalidades do OpenFlow apartir daí. Há basicamente dois modos de se registrar um componente - usando core.register() e usando core.registerNew(). O último é de fato só um wrapper conveniente sobre em torno do primeiro. 


core.register() recebe dois argumento. O segundo é o objeto que queremos no core. O primeiro é o nome que queremos usar para isso. Aqui temos um exemplo de um componente simples com uma função launch() que registra esse componente como core.thing:
 
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
  
  

No caso de, por exemplo, funções de execução que podem ser invocadas múltiplas vezes, você pode querer só registrar um objeto uma vez. Você pode simplesmente checar se se ocomponente já foi registrado (usando core.hasComponent()), mas isso pode ser feito também com core.registerNew(). Enquanto você passa um objeto específico para core.register(),  você passa uma classe para core.refisterNew(). Se o componente em questão já foi registrado, registerNew() não faz nada.


registerNew() geralmente recebe um único parâmetro- a classe qeu você deseja instanciar. Se o métido __init)) dessa classe recebe argumentos, voce pode passá-los como parâmetros adicionais para registerNew(). Por exemplo, podemos mudar a função de execução acima para: 


def launch ():
  core.registerNew(MyComponent, "spam")
  core.MyComponent.foo() # prints "MyComponent with arg: spam"


Note que registerNew() registra automaticamente o objeto recebido usando o nome de sua classe (isto é, ele agora é “MComponent” ao invéz de “thing”). Isso pode ser sobreescrito fornecendo um atributo _core_name ao objeto:


class MyComponent (object):
  _core_name = "thing"


  def __init__ (self, an_arg):
        self.arg = an_arg
        print "MyComponent instance registered with arg:", self.arg


  def foo (self):
        print "MyComponent with arg:", self.arg
Trabalhando com endereços: pox.lib.addresses


Endereços IP e Ethernet no POX são representados por classes IPAddr e EthAddr de pox.lib.addresses. Em alguns casos, outros formatos de endereços podem tmabém funcionar (e.g., endereços IP em quádrupla separada por pontos), mas usar classes de endereçamento deve sempre funcionar. 


Por exemplo, quando trabalhamos com endereços IP:


from pox.lib.addresses import IPAddr, EthAddr


ip = IPAddr("192.168.1.1")
print str(ip) # Prints "192.168.1.1"
print ip.toUnsignedN() # Converte em  formato inteiro sem sinal -- 16885952
print ip.toRaw() # Retorna um objeto de tamanho de quatro bytes(uma string de quatro bytes, mais ou menos)


ip = IPAddr(16885952,networkOrder=True)
print str(ip) # Também imprime "192.168.1.1" !


O sistema de eventos: pox.lib.revent


Tratamento de eventos no POX se encaixa dentro do paradigma inscrição e publicação (publish/subscribe). Certos objetos publicam eventos (em linguajar da revent, esse é um evento de “raising”; às vezes também denominado “sourcing”, “firing” ou “dispatching” um evento). Podemos nossubescrever à um evento específico desses objetos (em linguajar da revent, chamado “listening to”; também “handling”, ou “sinking”); o que queremos dizer é que quandp eventos ocorrem, nós gostaríamos que uma parte específica de código seja chamada (um “event handler” ou às vezes um “event listener”). (Se há algo que podemos dizer sobre eventos, é que não há uma terminologia simplista ou encurtada.)


A biblioteca revent pode na verdade fazer algumas coisas estranhas. POX somente usa apenas uma porção de subconjunto de coisas não estranhas dessa funcionalidade, e particularmente usa só um subconjunto desse subconjunto! O que é descrito nessa seção é o subconjunto que o POX mais pesadamente. 
Eventos no POX são todas instancias de subclasses de reventEvent. Uma classe que levanta um evento (uma fonte de evento) herda de revent.EventMixin, e declara quais eventos ela levanta em um uma variével de classe denominada _eventMixin_Events. Aqui está um exemplo de uma classe que levanta dois eventos:     


class Chef (EventMixin):
  """
  Class modeling a world class chef


  This chef only knows how to prepare spam, but we assume does it really well.
  """
  _eventMixin_events = set([
        SpamStarted,
        SpamFinished,
  ])


Tratando eventos


Então talvez seu programa possua um objeto de uma classe Chef chamada chef. Você sabe que ele gera um par de eventos. Talvez você esteja interessado quando seu delicioso spam esteja pronto, então você gostaria de escutar o evento SpamFinished.
  


Tratadores de eventos
Primeiramente, vejamos exatamente como um evento escutador se parece. Resimindo: é uma função (ou um método ou alguma coisa que é invocável). Eles quase sempre recebem somente um argumento- o objeto do evento em si (contuto isso nem sempre é o caso- uma classe de evento pode mudar seu comportamento, em todo caso, essa documentação deve mencionar isso!). Assumindo SpamFinished como um evento típico, ele pode ter um tratador do tipo:


def spam_ready (event):
  print "Spam is ready!  Smells delicious!"
Listening To an Event


Agora precisamos setar de fato nossa função spam_ready para ser uma escutadora do evento SpamFinished:
 
chef.addListener(SpamFinished, spam_ready)


Às vezes você pode não ter a classe do evento (e.g., SpamFinished) no escopo. Você pode a importar caso você queira, mas você pode também usar o método addListenerByName()  alternativamente:


chef.addListenerByName("SpamFinished", spam_ready)


Setando escutadores automaticamente
Frequentemente, seu evento escutador é um método de uma classe. Tmabém, você está frequentemente interessado em escutar múltiplos eventos de um mesmo objeto fonte. revent provê um atalho para essa situação: addListeners().


class HungryPerson (object):
  """ Models a person that loves to eat spam """


  def __init__ (self):
        chef.addListeners(self)


  def _handle_SpamStarted (self, event):
        print "I can't wait to eat!"


  def _handle_SpamFinished (self, event):
        print "Spam is ready!  Smells delicious!"
Quando você chama foo.addListeners(bar), ele procura nos eventos de foo, e se encontra um método em bar um nome como  _handle_EventName, ele seta esse método como escutador.


Em alguns caso, você pode querer apenas que uma única classe escute eventos de múltiplas fontes. Às vezes é importante que você distingua ambos entre si. Para esse propósito, você pode também usar um “prefixo” que é inserido aos nomes dos tradores:


class VeryHungryPerson (object):
  """ Models a person that is hungry enough to need two chefs """


  def __init__ (self):
        master_chef.addListeners(self, prefix="master")
        backup_chef.addListeners(self, prefix="secondary")


  def _handle_master_SpamFinished (self, event):
        print "Spam is ready!  Smells delicious!"


  def _handle_secondary_SpamFinished (self, event):
        print "Backup spam is ready.  Smells slightly less delicious."


Criando seus próprios tipos de eventos
Como visto acima, eventos são subclasses de revent.Event. Então para criar um evento, simplesmente crie uma subclasse de Event. Você pode adicionar qualquer atributo extra ou métodos que você queira. Continuando nosso exemplo:


class SpamStarted (Event):
  def __init__ (self, brand = "Hormel"):
        Event.__init__(self)
        self.brand = brand


  @property
  def genuine (self):
        # If it's not Hormel, it's just canned spiced ham!
        return self.brand == "Hormel"


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


def parseICMP(packet):
        if eth_packet.type == ethernet.IP_TYPE:
            ip_packet = eth_packet.payload
            if ip_packet.protocol = ipv4.ICMP_PROTOCOL
                icmp_packet = ip_packet.payload
...


Essa provavelmente não é a melhor forma de se navegar por um pacote, mas ilustra a estrutura de um cabeçalho de pacote no POX. Em cada nível de encapsulamento os valores do cabeçalho do pacote podem ser obtidos. Por exemplo, o endereço de IP de origem do pacote 
...
src_ip = ip_packet.srcip
icmp_sequence = icmp_packet.seq


E similarmente para outros cabeçalhos de pacotes. Consulte o código específico do pacote para outros cabeçalhos. 


Um método find de algum objeto pode ser usado para encontrar um pacote encapsulado específico atravéz do nome desejado (e.g., “icmp”) ou sua classe (e.g., pkt.ICMP). Se opacote não encapsula o tipo de pacote requisitado, find() retorna None. Por exemplo:


def handle_IP_packet (packet):
  ip = packet.find('ipv4')
  if ip is None:
        # This packet isn't IP!
        return
  print "Source IP:", ip.srcip


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
	



Exemplo - Temporizador único


from pox.lib.recoco import Timer


def handle_timer_elapse (message):
  print "I was told to tell you:", message


Timer(10, handle_timer_elapse, args = ["Hello"])


# Prints out "I was told to tell you: Hello" in 10 seconds


# Alternate way for simple timers:
from pox.core import core # Many components already do this
core.callDelayed(10, handler_timer_elapse, "Hello") # You can just tack on args and kwargs.




Exemplo - Temporizador Recorrente


# Simulate a long road trip


from pox.lib.recoco import Timer


we_are_there = False


def are_we_there_yet ():
  if we_are_there: return False # Cancels timer (see selfStoppable)
  print "Are we there yet?"


Timer(30, are_we_there_yet, recurring = True)


Trabalhando com sockets: ioworker


pox.lib.ioworker contém uma API de alto nível para trabalhar com sockets assincronos em POX. envios são dispare-e-esqueça (fire-and-forget), dados recebidos são buferizados e um callback é disparado quando há algo disponível, etc. 


TODO: Documentation and samples




##OpenFlow no POX


Uma das principais intenções do uso do POX é o desenvolvimento de aplicações de controle OpenFlow. Neste capítulo, descrevemos algumas das características e interfaces do POX que facilitam isso.




O componente do POX que realmente se comunica com os interruptores de OpenFlow  é o openflow.of_01 (o 01 refere-se ao facto de que este componente se comunica pelo protocolo físico OpenFlow 0x01). Meu padrão, of_01 registra um OpenFlow "nexus" sobre o objeto do núcleo como "OpenFlow". Esta é a principal interface para trabalhar com OpenFlow em POX - você pode usar este nexo OpenFlow para enviar comandos para interruptores e receber mensagens de interruptores (na forma de eventos levanta - consulta a subseção abaixo).


Sempre quando um switch se conecta ao POX, também há um objeto de conexão associado. Existe muita sobreposição entre as conexões e o Nexus. Qualquer um deles pode ser usado para enviar uma mensagem a um switch, e a maioria dos eventos são gerados em ambos. Às vezes é mais conveniente usar um ou o outro. Se a sua aplicação está aplicada à eventos de todos os switches, faz sentido ouvir ao Nexus, o qual gera eventos para todos os switches. Se você somente está interessado em um único switch,  pode fazer sentido ouvir a uma conexão específica.




There are three major ways to get a reference to a Connection object:


You can listen to ConnectionUp events on the nexus – these pass the new Connection object along
You can use the nexus's getConnection(<DPID>) method to find a connection by the switch's DPID
You can enumerate all of the nexus's connections via its connections property (e.g., for con in core.openflow.connections)
OpenFlow Events: Responding to Switches
Note: For more background on the event system in POX, see the relevant section in this manual.


Most OpenFlow related events are raised in direct response to a message received from a switch.  As a general guideline, OpenFlow related events have the following three attributes:


attribute    type    description
connection    Connection    Connection to the relevant switch (e.g., which sent the message this event corresponds to)
dpid
long    Datapath ID of relevant switch (use dpid_to_str to format)
ofp    ofp_header subclass    OpenFlow message object that caused this event.  See OpenFlow Messages for info on these objects




In the rest of this section, we describe some of the events provided by the OpenFlow module and topology module.  To get you started, here's a very simple POX component that listens to ConnectionUp events from all switches, and logs a message when one occurs.  You can put this into a file (e.g., ext/connection_watcher.py) and then run it (with ./pox.py connection_watcher) and watch switches connect.


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
ConnectionUp
Event class definition:


class ConnectionUp (Event):
  def __init__ (self, connection, ofp):
        Event.__init__(self)
        self.connection = connection
        self.dpid = connection.dpid
        self.ofp = ofp
Event raised when the connection to an OpenFlow switch has been established.


Taking a look at the attributes:


attribute    type    notes
connection    Connection    This object can be used for communication to the switch.  For example, you can use it to send OpenFlow commands, or you can listen to events sourced by this Connection object in order to handle messages sent by the switch.
dpid    integer/long    This is the DPID associated with the switch.  Use util.dpidToStr() to display it.
ofp    ofp_switch_features    Contains information about the switch, for example supported action types (e.g., whether field rewriting is available), and port information (e.g., MAC addresses and names).  (This is also available on the Connection's features attribute.)
This event can be handled as shown below:


def _handle_ConnectionUp (self, event):
        print "Switch %s has come up." % event.dpid
ConnectionDown
Event class definition:


class ConnectionDown (Event):
  def __init__ (self, connection, ofp):
        Event.__init__(self)
        self.connection = connection
        self.dpid = connection.dpid
Event raised when the connection to an OpenFlow switch has been lost.


PortStatus
Event class definition:


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
Fired in response to port status changes and the code to handle looks like this:


def _handle_PortStatus (self, event):
        if event.added:
            action = "added"
        elif event.deleted:
            action = "removed"
        else:
            action = "modified"
        print "Port %s on Switch %s has been %s." % (event.port, event.dpid, action)
FlowRemoved
Event class definition:


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

Raised when a flow entry has been removed from a flow table. This may either be because of a timeout or because it was removed explicitly.  Such notifications are sent when the flow was installed with the OFPFF_SEND_FLOW_REM flag set.  See the OpenFlow specification for further details.


idleTimeout (bool) - True if expired because of idleness
hardTimeout (bool) - True if expired because of hard timeout
timeout (bool) - True if either of the above is true
deleted (bool) - True if deleted explicitly
Statistics Events
class RawStatsReply (Event):
  def __init__ (self, connection, ofp):
        self.connection = connection
        self.ofp = ofp         # Raw ofp message(s)


class StatsReply (Event):
  def __init__ (self, connection, ofp, stats):
        Event.__init__(self)
        self.connection = connection
        self.ofp = ofp         # Raw ofp message(s)
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
While one can listen for raw stats messages, this is not particularly convenient as you must check the type of statistic it is holding, and you may need to manually assemble complete groups of statistics because a single statistics request may be responded to in multiple parts.  It is usually much easier to listen for specific events that are subclasses of StatsReply, e.g., FlowStatsReceived.  When handling these StatsReply-based events, the stats property will contain a complete set of statistics (e.g., an array of ofp_flow_stats).  See the section on ofp_stats_request for more information.


PacketIn
Event definition:


class PacketIn (Event):
  def __init__ (self, connection, ofp):
        Event.__init__(self)
        self.connection = connection
        self.ofp = ofp
        self.port = ofp.in_port
        self.data = ofp.data
        self._parsed = None
        self.dpid = connection.dpid
Fired in response to PacketIn events


port (int) - number of port the packet came in on
data (bytes) - raw packet data
parsed (packet subclasses) - pox.lib.packet's parsed version
ofp (ofp_packet_in) - OpenFlow message which caused this event
connection - Connection to switch from which this event originated
ErrorIn
Event definition:


class ErrorIn (Event):
  def __init__ (self, connection, ofp):
        Event.__init__(self)
        self.connection = connection
        self.ofp = ofp
        self.xid = ofp.xid
Fired in response to an Error arriving at the controller.  You may also use the asString() method to see a string representation of the error.


BarrierIn
Event Definition:


class BarrierIn (Event):
  def __init__ (self, connection, ofp):
        Event.__init__(self)
        self.connection = connection
        self.ofp = ofp
        self.dpid = connection.dpid
        self.xid = ofp.xid
Fired in response to a barrier reply.


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

<tr><th>buffer_id</th> <th>int/None</th> <th>None</th> <th>ID of the buffer in which the packet is stored at the datapath. If you're not resending a buffer by ID, use None.</th></tr>

<tr><th>in_port</th> <th>int</th> <th>OFPP_NONE</th> <th>Switch port that the packet arrived on if resending a packet.</th></tr>

<tr><th>actions</th> <th>list of ofp_action_XXXX </th> <th>[ ]</th> <th>If you have a single item, you can also specify this using the named parameter "action" of the initializer.</th></tr>

<tr><th>data</th> <th>bytes / ethernet / ofp_packet_in</th> <th>''</th> <th>The data to be sent (or None if sending an existing buffer via its buffer_id).
If you specify an ofp_packet_in for this, in_port, buffer_id, and data will all be set correctly – this is the easiest way to resend a packet.</th></tr>
</table>

If you receive an ofp_packet_in and wish to resend it, you can simply use it as the data attribute.
See section of 5.3.6 of OpenFlow 1.0 spec. This class is defined in pox/openflow/libopenflow_01.py.


ofp_flow_mod - Flow table modification
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
cookie (int) - identifier for this flow rule. (optional)
command (int) - One of the following values:
OFPFC_ADD - add a rule to the datapath (default)
OFPFC_MODIFY - modify any matching rules
OFPFC_MODIFY_STRICT - modify rules which strictly match wildcard values.
OFPFC_DELETE - delete any matching rules
OFPFC_DELETE_STRICT - delete rules which strictly match wildcard values.
idle_timeout (int) - rule will expire if it is not matched in 'idle_timeout' seconds. A value of OFP_FLOW_PERMANENT means there is no idle_timeout (the default).
hard_timeout (int) - rule will expire after 'hard_timeout' seconds. A value of OFP_FLOW_PERMANENT means it will never expire (the default)
priority (int) - the priority at which a rule will match, higher numbers higher priority. Note: Exact matches will have highest priority.
buffer_id (int) - A buffer on the datapath that the new flow will be applied to.  Use None for none.  Not meaningful for flow deletion.
out_port (int) - This field is used to match for DELETE commands.OFPP_NONE may be used to indicate that there is no restriction.
flags (int) - One of the following values:
OFPFF_SEND_FLOW_REM - Send flow removed message to the controller when rule expires
OFPFF_CHECK_OVERLAP - Check for overlapping entries when installing. If one exists, then an error is send to controller
OFPFF_EMERG - Consider this flow as an emergency flow and only use it when the switch controller connection is down.
actions (list) - actions are defined below, each desired action object is then appended to this list and they are executed in order.
match (ofp_match) - the match structure for the rule to match on (see below).
See section of 5.3.3 of OpenFlow 1.0 spec. This class is defined in pox/openflow/libopenflow_01.py line 1831


Exemplo: Installing a table entry
# Traffic to 192.168.101.101:80 should be sent out switch port 4


# One thing at a time...
msg = of.ofp_flow_mod()
msg.priority = 42
msg.match.dl_type = 0x800
msg.match.nw_dst = IPAddr("192.168.101.101")
msg.match.tp_dst = 80
msg.actions.append(of.ofp_action_output(port = 4))
self.connection.send(msg)


# Same exact thing, but in a single line...
self.connection.send( of.ofp_flow_mod( action=of.ofp_action_output( port=4 ),
                                           priority=42,
                                           match=of.ofp_match( dl_type=0x800,
                                                               nw_dst="192.168.101.101",
                                                               tp_dst=80 )))
Exemplo: Clearing tables on all switches
# create ofp_flow_mod message to delete all flows
# (note that flow_mods match all flows by default)
msg = of.ofp_flow_mod(command=of.OFPFC_DELETE)


# iterate over all connected switches and delete all their flows
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
type (int) - The type of stats request (e.g., OFPST_PORT).  Default is to try to guess based on body.
flags (int) - No flags are defined in OpenFlow 1.0.
body (flexible) - The body of the stats request.  This can be a raw bytes object, or a packable class (e.g., ofp_port_stats_request).
See section of 5.3.5 of OpenFlow 1.0 spec for more info on this structure and on the individual statistics types (port stats, flow stats, aggregate flow stats, table stats, etc.). This class is defined in pox/openflow/libopenflow_01.py


TODO: Show some of the individual stats request/reply types?


Exemplo - Web Flow Statistics
Request the flow table from a switch and dump info about web traffic.  This example is meant to be run along with, say, the forwarding.l2_learning component.  It can be pasted into the POX interactive interpreter.


See the Statistics Events section for more info.


1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
import pox.openflow.libopenflow_01 as of
log = core.getLogger("WebStats")


`# When we get flow stats, print stuff out
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


Estrutura de combinação


OpenFlow define uma estrutura de combinação – ofp_match – que permite a você definir um conjunto de cabeçalhos que serão combinados. Você pode tanto criar do zero ou usar um facory method para criar baseado em um pacote pacore já existente.


A estrutura de combinação é definida em pox/openflow/libopenflow_01.py na classe ofp_match.  Seus atributos são derivados dso membros existentes na especificação do OpenFlow, então dirija-se à ela para maiores informações. Ainda assim, ela está resumida na tabela abaixo:


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


my_match = of.ofp_match(in_port = 5, dl_dst = EthAddr("01:02:03:04:05:06"))
`#.. ou ..`
my_match = of.ofp_match()
my_match.in_port = 5
my_match.dl_dst = EthAddr("01:02:03:04:05:06")


Campos não especificados são tratados como wildcards e vão combinar com qualquer pacote. Você pode explicitamente setar um campo para ser wildcard setando o mesmo como None.




Enquanto a estrutura ofp_match do OpenFlow é definida como tendo atributos wildcards, você provavelmente nunca precisará setar isso quando estiver usando o POX- simplesmente não atribua valores ao campo que você quer que seja wildcard (ou atribua NOne).
Os campos de endereço IP podem na verdade ser parcialmente wildcard, o que lhe permite combinar subnets inteiras. Há algumas formas de fazer isso. Aqui estão algumas equivalente:


my_match.nw_src = "192.168.42.0/24"
my_match.nw_src = (IPAddr("192.168.42.0"), 24)
my_match.nw_src = "192.168.42.0/255.255.255.0"


Como exemplo, o código seguinte criará uma combinação para tráfego direcionado a web server:


import pox.openflow.libopenflow_01 as of # POX convention
import pox.lib.packet as pkt # POX convention
my_match = of.ofp_match(dl_type = pkt.ethernet.IP_TYPE, nw_proto = pkt.ipv4.TCP_PROTOCOL, tp_dst = 80)
Define a match from an existing packet
There is a simple way to create an exact match based on an existing packet object (that is, an ethernet object from pox.lib.packet) or from an existing ofp_packet_in.  This is done using the factory method ofp_match.from_packet().


my_match = ofp_match.from_packet(packet, in_port)
The packet parameter is a parsed packet or ofp_packet_in from which to create the match, and the in_port parameter is the switch port you want this match to match agains (or leave it unspecified to match on all switch ports).


Note that you can now set fields of the resultant match object to None if you want a less-than-exact match.


OpenFlow Actions
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


Communicating with a Datapath (Switch)
Connections to datapaths exist in POX in the form of Connection objects.  Connection objects have a variety of events that you can listen to in order to be advised of events related to that particular datapath (in many cases, these are the same as events on the core.openflow object, except those are fired for events on any switch).  Additionally, it can be used to send messages to the datapath.


There are multiple ways that you can get access to a Connection object.  A common way takes advantage of the fact that when a switch connects, a ConnectionUp event is fired on core.openflow – you can simply save the Connection object to your own variable:


class MyClass (object):
        def __init__ (self):
            self.connections = set()
            core.openflow.addListeners(self)


        def _handle_ConnectionUp (self, event):
            self.connections.add(event.connection) # See ConnectionUp event
If you know the DPID of the switch, you can also retrieve it from core.openflow using the getConnection() method.  Lastly, you can iterate core.openflow.connections, which is a list of all active connections.


Note: If you just want to send a message to a datapath and you know its DPID, you can use core.openflow.sendToDPID(dpid, msg).  This is similar to do doing core.openflow.getConnection(dpid).send(msg).


Connection Objects
Once you have a Connection object, you can use it to listen to events specific to that connection/datapath, and you can use its send() method to send OpenFlow messages to the associated datapath.


TODO: Discuss listening to events on Connection objects and other Connection attributes (e.g., .features).


There are many types of messages you might send to a datapath – the OpenFlow specification holds a complete list.  We'll cover two of these – FlowMods and PacketOuts – below.


Exemplo: Sending a FlowMod
To send a flow mod you must define a match structure (discussed above) and set some flow mod specific parameters as shown here:


msg = ofp_flow_mod()
msg.match = match
msg.idle_timeout = idle_timeout
msg.hard_timeout = hard_timeout
msg.actions.append(of.ofp_action_output(port = port))
msg.buffer_id = <some buffer id, if any>
connection.send(msg)
Using the connection variable obtained when the datapath joined, we can send the flowmod to the switch.


Exemplo: Enviando um PacketOut
De modo similar à um flow mod, é preciso primeiro definir um pacote de saída como mostrado:


msg = of.ofp_packet_out(in_port=of.OFPP_NONE)
msg.actions.append(of.ofp_action_output(port = outport))
msg.buffer_id = <some buffer id, if any>
connection.send(msg)

inport é setado como OFPP_NONE por que o pacote foi gerado no controlador e não originado como um pacote no switch.


Aplicações de terceiros, tutoriais, Etc.



Essa sessão tem por objetivo destacar alguns projetos que fazem uso do POX mas que não são partes integralizantes dele. (Isso pode ser movido para uma página própria ou algo assim no furuto.)


POXDesk: Uma interface gráfica web para o POX




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




O guia de estilo de codificação para Python 
Convenções de codificação (The Style Guide for Python Code (AKA PEP 8)) destaca algumas convenções para desenvolvimento em Python. É uma boa base, porém POX não objetiva conformidades estritas. Aqui há algumas diretrizes para escrita de código em POX, especialmente se você gostaria de ter algo inserido no código fonte (merged). Note que emalguns casos elas são adições ou diferenciações à PEP 8. Observe também que a recomendação mais importante é que o código seja legível. Outra coisa é que o o repositório não é interiramente conforme com as diretrizes abaixo (envio de modificações (pull requests) são muito bem vindas!).




Dois espaços para identação 
Comprimento de linha
Máximo de 79 caracteres, no entanto 80 não vai nos matar.
Use juntadores de linha implícitos (e.g. "hanging" parentheses ou colchetes) ao invéz de do caracter de continuação de linha (\) explícitos, ao não ser qeu esse último seja muito estranho.


Continue linhas abaixo do parantêse/colchete apropriado (Estilo Lisp)  quando isso funcionar bem. Quando não se pode fazer isso, “minha” preferência é identar um “espaço único”, contudo eu sei que isso deixa muitos programadores de Pythn loucos, então sou exitante ao definir uma regra específica aqui, contanto que isso esteja claro. A regra básica é que deve haver identenção mais ou menos como o usual - i.e., não use dois espaços para linha contínua. Estou cada vez mais usando quatro espaços. 


Duas linhas em branco separam partes de alto nível de código com significância estrutural (classes, funções de mais alto nível, etc.). Você pode usar três para separar porções grandes de código (contudo, quando alguém é tentado a fazer isso, é sempre uma boa idéia se perguntar se duas partes grandes de código deveriam estar em arquivos separados). Métodos dentro de classes devem ter uma ou duas, mas devem ser consistentes dentro da classe em particular.


Colocar um espaço entre o nome da função e o abre parênteses da lisat de parâmetros. Similarmente para classe e super clase. Isso é: 
“def foo (bar):” e não “def foo(bar):”.


Use somente novo estilo de classes. (isso quer dizer herdar de object.)


Docstrings:
Ou atenha-se à """ one line """, ou tenha os abre e fecha """ nas linhas.


Nomeando 
Classes devem geralmente ter Caixa-alta no início
Métodos e outros atributos devem ser caixa_baixa_com_underscores. Note que isso é atualemente violado por todo lado (no entanto está ficando melhor na versão betta).
Membros “privados” (que você não quer que outros mexam explicitamente ) devem começar com um underscore. 
“constantes” devem ser CAIXA_ALTA_COM_UNDERSCORES
A palavras chave para variável “catch-all” é chamada kw (contrariando a convensão do Python de kwargs) 


##FAQs


* Como posso mudar a porta Openflow 6633?


Se você ligar o modo verboso, você verá que é omódulo openflow.of_01 que escuta conexões. Essa é a dica: é esse componente que precisa ser configurado. Para fazer isso basta passar uma “porta” como argumento para esse componente na linha de comando:


./pox.py openflow.of_01 --port=1234 <outros argumentos de linha de comando>


   * Como posso ter algum componente iniciando automaticamente toda vez que rodar o POX?

A resposta curta é que não há um modo suportado para fazer isso. Contudo, é bem simples somente criar um pequeno componente que lança qualquer componente que você queira.


Por exemplo, digamos que que esteja cansado de toda vez ter qeu se lembrar da seguinte linha de comando:


./pox.py log.level --DEBUG samples.pretty_log openflow.keepalive --interval=15 forwarding.l2_pairs


Escrevendo o seguinte componente em ext/startup.py, você pode substituir o que foi feito acima por simplesmente:


./pox.py startup
# Put me in ext/startup.py


def launch ():
  from log.level import launch
  launch(DEBUG=True)


  from samples.pretty_log import launch
  launch()


  from openflow.keepalive import launch
  launch(interval=15) # 15 seconds


  from forwarding.l2_pairs import launch
  launch()




   * Como posso fazer os switches enviarem um pacote com payload completo para o controlador?

POr padrão, quando um pacotes não se encaixa em nenhuma entrada na tabela de fluxo, ele é encaminhado para o controlador. No POX, esse padrão é OFP_DEFAULT_MISS_SEND_LEN,  que é 128. Isso é provavelmente suficiente para, e.g., ethernet, IP e cabeçalhos TCP... mas provavelmente não suficiente para pacotes completos. Se você quizer inspecionar payloads de pacotes completos, você tem duas opções:


Instalar uma entrada na tabela. Se você instala uma regra na tabela com packet_out to OFPP_CONTROLLER, POX fará com que o switch enviei o pacote completo por padrão (você pode manualmente setar números menores de bytes se você quizer).


Mudar o tamanho do envio. Se você setar ore.openflow.miss_send_len durante a inicialização (antes de todos os switches se conectarem), switches devem enviar essa quantidade de bytes quando um pacote não combinar com nenhuma entrada na tabela.
Dê uma olhada no componente misc.packet_dump para um exemplo.




   * Como posso estabelecer comunicação entre os componentes?


Componentes são simplesmente pacotes e módulos em Python, então um modo de fazer isso é do mesmo que você comunica entre qualquer módulo em Python - importe um deles e acesse suas variáveis de alto nível e funções.


POX também tem um mecanismo alternativo, que é melhor descrito na sessão Trabalhando com POX: O objeto POX Core. 


   * Como posso usar o POX com o Mininet?


Use a função de controlador remoto. Por exemplo, se você está rodando POX na mesma máquina que Mininet:


`mn --topo=linear --mac --controller=remote
`

Umna configuração comum é rodar o Mininet em uma máquina virtual e o POX no seu ambiente de hospedeiro. Nesse caso, aponte o Mininet para o endereço IP da máquina hospedeira. Se você estiver usando o VirtualBox e um “Host-only Adpter”, esse é o endereço atribuído ao adaptador virtual no VirtualBox (i.e., vboxnet0). Por exemplo:


`mn --topo=linear --mac --controller=remote --ip=192.168.56.1
`

Estou vendo múltiplas mensagens de packet_in e encaminhamento não está funcionando; o que que ouve?
Esse problema é frequentemente visto ao usar um dos componentes de encaminhamento “learning” (l2_learning, l2_multi, etc.) em uma topologia em malha ou outro com um loop. Esses componentes de encaminhamento não tentam trabalhar em uma topologia com loops. 


O component spanning_tree component tem como objetivo ser uma solução henérica para esse problema, logo, voc}e pode tentar executá-lo tmabém. Isso pode ser útil também para evitar broadcasts até que discovery tenha tempo para descobrir toda a topologia. Alguns componentes (assim como l2_learning) tem a opção de forçar isso.

Estou usando o switch de referência (ou o Pantou) e os switches ficam se diconectando. Ajuda?
A resposta curta é que você rodou o componente openflow.keepalive tmabém. Veja a descrição desse componente acima para maiores informações.


###Meu código não está funcionando! Podem me ajudar?


Possivelmente. Há algumas coisas que podem te fornecer uma auto ajuda. Se nenhum desses modos funcionar ou se aplicar, há alguns coisas que você pode fazer para nos ajudar a te ajudar. Aqui há algumas coisas que você pode tentar: 


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

Fazendo o que foi dito acima, torna bemmais fácil para as pessoas te ajudarem e você potencialmene economiza tempo - se você não fizer o que foi dito acima, é bem provável que as primeiras sugestões que você receberá da lista será fazer algo que foi dito anteriomente aqui! 

