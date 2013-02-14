# POX


* Instalando o POX
  * Requisitos
  * Adquirindo o Código
  * Selecionando um Branch / Versão
  * PyPy Supporte

* Invocando o POX

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
    * Um exemplo simple
    * Envocação múltipla
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
* Ferramentas de terceiros, Tutoriais, Etc.
   * POXDesk: Uma interface gráfica web para o POX
   * Tutorial OpenFlow
   * Tutorial Switch OpenFlow
   * Exemplo de Coletor de estatísticas de fluxo
* Convenções de codificação
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

POX oficialmente suporta Windows, Mac OS, e Linux (contudo ele tem sido usando em outros sistemas também).  A lot of the development happens on Mac OS, so it almost always works on Mac OS.  Occasionally things will break for the other OSes; the time it takes to fix such problems is largely a function of how quickly problems are reported.  In general, problems are noticed on Linux fairly quickly (especially for big problems) and noticed on Windows rather slowly.  If you notice something not working or that seems strange, please submit an issue on the github tracker or send a message to the pox-dev mailing list so that it can be fixed!

POX can be used with the "standard" Python interpreter (CPython), but also supports PyPy (see below).

Getting the Code
The best way to work with POX is as a git repository.  You can also grab it as a tarball or zipball, but source control is generally a good thing.

POX is hosted on github.  If you intend to make modifications to POX itself, you might consider making your own github fork of it from the POX repository page.  If you just want to grab it quickly to run or play around with, you can simply create a local clone:

$ git clone http://github.com/noxrepo/pox
$ cd pox
Selecting a Branch / Version
The POX repository has multiple branches.  Specifically, it has at least some release branches and at least one active branch.  The default branch (what you get if you just do the above commands) will be the most recent release branch.  Release branches may get minor updates, but are no longer being actively developed.  On the other hand, active branches are being actively developed.  In theory, release branches should be somewhat more stable (by which we mean that if you have something working, we aren't going to break it).  On the other hand, active branches will contain improvements (bug fixes, new features, etc.).  Whether you should base your work on one or the other depends on your needs.  One thing that may factor into your decision is that you'll probably get better support on the mailing list if you're using an active branch (lots of answers start with "upgrade to the active branch").

As of this writing, the active branch is named betta.  To use the betta branch, you simply check it out after cloning the repository:

~$ git clone http://github.com/noxrepo/pox
~$ cd pox
~/pox$ git checkout betta
PyPy Support
While it's not as heavily tested as the normal Python interpreter, it's a goal of POX to run well on the PyPy Python runtime.  There are two advantages of this.  First, PyPy is generally quite a bit faster than CPython.  Secondly, it's very easily portable – you can easily package up POX and PyPy in a single tarball and have them ready to run.

You can, of course, download, install, and invoke PyPy in the usual way.  On Mac OS and Linux, however, POX also supports a really simple method: Download the latest PyPy tarball for your OS, and decompress it into a folder named "pypy" alongside pox.py.  Then just run pox.py as usual (./pox.py), and it should use PyPy instead of CPython.

Invoking POX
POX is invoked by running pox.py.  POX itself has a couple of optional commandline arguments than can be used at the start of the commandline:

option	meaning
--verbose	Display extra information (especially useful for debugging startup problems)
--no-cli	Do not start an interactive shell (No longer applies as of betta)
--no-openflow	Do not automatically start listening for OpenFlow connections
But running POX by itself doesn't do much – POX functionality is provided by components (POX comes with a handful of components, but POX's target audience is really people who want to be developing their own).  Components are specified on the commandline following any of the POX options above. An Exemplo of a POX component is forwarding.l2_learning.  This component makes OpenFlow switches operate kind of like L2 learning switches.  To run this component, you simply name it on the command line following any POX options:

./pox.py --no-cli forwarding.l2_learning
You can specify multiple components on the command line.  Not all components work well together, but some do.  Indeed, some components depend on other components, so you may need to specify multiple components.  For Exemplo, you can run POX's web server component along with l2_learning:

./pox.py --no-cli forwarding.l2_learning web.webcore
Some components take arguments themselves.  These follow the component name and (like POX arguments) begin with two dashes.  For Exemplo, l2_learning has a "transparent" mode where switches will even forward packets that are usually dropped (such as LLDP messages), and the web server's port number can be changed from the default (8000) to an arbitrary port.  For Exemplo:

./pox.py --no-cli forwarding.l2_learning --transparent web.webcore --port=8888
 (If you're starting to think that command lines can get a bit long and complex, there's a solution: write a simple component that just launches other components.)

Components in POX
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

./pox.py log --no-default
Log Formatting
Please see the documentation on Python's LogRecord attributes for details on log formatting.  As a quick example, you can add timestamps to your log as follows:

./pox.py log --format="%(asctime)s: %(message)s"
Or with simpler timestamps:

./pox.py log --format="[%(asctime)s] %(message)s" --datefmt="%H:%M:%S"
See the samples.pretty_log component for another example (and, particularly, for an example that uses POX's color logging extension).

Log Output
Log messages are processed by various handlers which then print the log to the screen, save it to a file, send it over the network, etc.  You can write your own, but Python also comes with quite a few, which are documented in the Python reference for logging.handlers. POX lets you configure a lot of Python's built-in handlers from the commandline; you should refer to the Python reference for the arguments, but specifically, POX lets you configure:

Name	Type
stderr	StreamHandler for stderr stream
stdout	StreamHandler for stdout stream
File	FileHandler for named file
WatchedFile	WatchedFileHandler
RotatingFile	RotatingFileHandler
TimedRotatingFile	TimedRotatingFileHandler
Socket	SocketHandler - Sends to TCP socket
Datagram	DatagramHandler - Sends over UDP
SysLog	SysLogHandler - Outputs to syslog service
HTTP	HTTPHandler - Outputs to a web server via GET or POST
To use these, simply specify the Name, followed by a comma-separated list of the positional arguments for the handler Type.  For example, FileHandler takes a file name, and optionally an open mode (which defaults to append), so you could use:

./pox.py log --file=pox.log
Or if you wanted to overwrite the file every time:

./pox.py log --file=pox.log,w
You can also use named arguments by prefacing the entry with a * and then using a comma-separated list of key=value pairs.  For example:

./pox.py log --*TimedRotatingFile=filename=foo.log,when=D,backupCount=5
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

./pox.py log.level --WARNING
If you are trying to debug a problem with OpenFlow connections, however, you may want to turn up the verbosity of OpenFlow-related logs.  You can adjust all OpenFlow-related log messages like so:

./pox.py log.level --WARNING --openflow=DEBUG
If this leaves you with too many DEBUG level messages from openflow.discovery which you are not interested in, you can then turn it down specifically:

./pox.py log.level --WARNING --openflow=DEBUG --openflow.discovery=INFO
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

./pox.py foo --bar=1 foo --bar=2
You might expect to see:

foo: 1 eggs True
foo: 2 eggs True
Instead, however, you get an exception.  POX, by default, only allows components to be invoked once.  However, a simple change to your launch() function allows multiple-invocation:

def launch (bar, baz = "eggs", spam = True, __INSTANCE__ = None):
  print "foo:", bar, baz, spam
If you try the above commandline again, this time it will work.  Adding the __INSTANCE__ parameter both flags the function as being multiply-invokable, and also gets passed some information that can be useful for some modules that are invoked multiple times.  Specifically, it's a tuple containing:

The number of this instance (0...n-1)
The total number of instances for this module
True if this is the last instance, False otherwise (just a comparison between the previous two, but it's handy)
You might, for example, only want your component to do some of its initialization once, even if your component is specified multiple times.  You can easily do this by only doing that part of your initialization if the last value in the tuple is True.

You might also wish to examine the minimal component given in section "OpenFlow Events: Responding to Switches".  And, of course, check out the code for POX's existing components.

TODO: Someone should write a lot more about developing components.

POX APIs
POX contains a number of APIs to help you develop network control applications.  Here, we attempt to describe some of them. It is certainly not exhaustive so feel free to contribute.

Working with POX: The POX Core object
POX has an object called "core", which serves as a central point for much of POX's API.  Some of the functions it provides are just convenient wrappers around other functionality, and some are unique.  However, one of the other major purposes of the core object is to provide a rendezvous between components.  Often, rather than using import statements to have one component import another component so that they can interact, components will instead "register" themselves on the core object, and other components will query the core object.  A major advantage to this approach is that the dependencies between components are not hard-coded, and different components which expose the same interface can be easily interchanged.  Thought of another way, this provides an alternative to Python's normal module namespace, which is somewhat easier to rearrange.

Many modules in POX will want to access the core object.  By convention, this is doing by importing the core object as so:

from pox.core import core
TODO: Write more about this!

Registering Components
As mentioned above, it can be convenient for a component to "register" an API-providing object on the core object.  An example of this can be found in the OpenFlow implementation – the POX OpenFlow component by default registers an instance of OpenFlowNexus as core.openflow, and other applications can then access a lot of the OpenFlow functionality from there.  There are basically two ways to register a component – using core.register() and using core.registerNew().  The latter is really just a convenience wrapper around the former.

core.register() takes two arguments.  The second is the object we want to register on core.  The first is what name we want to use for it.  Here's a really simple component with a launch() function which registers the component as core.thing:

class MyComponent (object):
  def __init__ (self, an_arg):
    self.arg = an_arg
    print "MyComponent instance registered with arg:", self.arg

  def foo (self):
    print "MyComponent with arg:", self.arg


def launch ():
  component = MyComponent("spam")
  core.register("thing", component)
  core.thing.foo() # prints "MyComponent with arg: spam"
In the case of, for example, launch functions which can be invoked multiple times, you may still only want to register an object once.  You could simply check if the component has already been registered (using core.hasComponent()), but this can also be done with core.registerNew().  While you pass a specific object to core.register(), you pass a class to core.registerNew().  If the named component has already been registered, registerNew() just does nothing.

registerNew() generally takes a single parameter – the class you want it to instantiate.  If that class's __init__ method takes arguments, you can pass them as additional parameters to registerNew().  For example, we might change the launch function above to:

def launch ():
  core.registerNew(MyComponent, "spam")
  core.MyComponent.foo() # prints "MyComponent with arg: spam"
Note that registerNew() automatically registers the given object using the object's class name (that is, it's now "MyComponent" instead of "thing").  This can be overridden by giving the object an attribute called _core_name:

class MyComponent (object):
  _core_name = "thing"

  def __init__ (self, an_arg):
    self.arg = an_arg
    print "MyComponent instance registered with arg:", self.arg

  def foo (self):
    print "MyComponent with arg:", self.arg
Working with Addresses: pox.lib.addresses
IP and Ethernet addresses in POX are represented by the IPAddr and EthAddr classes of pox.lib.addresses.  In some cases, other address formats may work (e.g., dotted-quad IP addresses), but using the address classes should always work.

For example, when working with IP addresses:

from pox.lib.addresses import IPAddr, EthAddr

ip = IPAddr("192.168.1.1")
print str(ip) # Prints "192.168.1.1"
print ip.toUnsignedN() # Convert to network-order unsigned integer -- 16885952
print ip.toRaw() # Returns a length-four bytes object (a four byte string, more or less)

ip = IPAddr(16885952,networkOrder=True)
print str(ip) # Also prints "192.168.1.1" !
The Event System: pox.lib.revent
Event Handling in POX fits into the publish/subscribe paradigm.  Certain objects publish events (in revent lingo, this is "raising" an event; also sometimes called "sourcing", "firing" or "dispatching" an event).  One can then subscribe to specific events on these objects (in revent lingo, this is "listening to"; sometimes also "handling" or "sinking"); what we mean by this is that when the event occurs, we'd like a particular piece of code to be called (an "event handler" or sometimes an "event listener").  (If there's one thing we can say about events, it's that there's no shortage of terminology.)

	The revent library can actually do some weird stuff. POX only uses a fairly non-weird subset of its functionality, and mostly uses a pretty small subset of that subset!  What is described in this section is the subset that POX makes use of most heavily.
Events in POX are all instances of subclasses of revent.Event.  A class that raises events (an event source) inherits from revent.EventMixin, and declares which events it raises in a class-level variable called _eventMixin_Events.  Here's an example of a class that raises two events:

class Chef (EventMixin):
  """
  Class modeling a world class chef

  This chef only knows how to prepare spam, but we assume does it really well.
  """
  _eventMixin_events = set([
    SpamStarted,
    SpamFinished,
  ])
Handling Events
So perhaps your program has an object of class Chef called chef.  You know it raises a couple events.  Maybe you're interested in when your delicious spam is ready, so you'd like to listen to the SpamFinished event.

Event Handlers
First off, let's see exactly what an event listener looks like.  For one thing: it's a function (or a method or some other thing that's callable).  They almost always just take a single argument – the event object itself (though this isn't always the case – an event class can change this behavior, in which case, its documentation should mention it!).  Assuming SpamFinished is a typical event, it might have a handler like:

def spam_ready (event):
  print "Spam is ready!  Smells delicious!"
Listening To an Event
Now we need to actually set our spam_ready function to be a listener for the SpamFinished event:

chef.addListener(SpamFinished, spam_ready)
Sometimes you may not have the event class (e.g., SpamFinished) in scope.  You can import it if you want, but you can also use the addListenerByName() method instead:

chef.addListenerByName("SpamFinished", spam_ready)
Automatically Setting Listeners
Often, your event listener is a method on a class.   Also, you often are interested in listening to multiple events from the same source object.  revent provides a shortcut for this situation: addListeners().

class HungryPerson (object):
  """ Models a person that loves to eat spam """

  def __init__ (self):
    chef.addListeners(self)

  def _handle_SpamStarted (self, event):
    print "I can't wait to eat!"

  def _handle_SpamFinished (self, event):
    print "Spam is ready!  Smells delicious!"
When you call foo.addListeners(bar), it looks through the events of foo, and if it sees a method on bar with a name like _handle_EventName, it sets that method as a listener.

In some cases, you may want to have a single class listening to events from multiple event sources.  Sometimes it's important that you can tell the two apart.  For this purpose, you can also use a "prefix" which gets inserted into the handler names:

class VeryHungryPerson (object):
  """ Models a person that is hungry enough to need two chefs """

  def __init__ (self):
    master_chef.addListeners(self, prefix="master")
    backup_chef.addListeners(self, prefix="secondary")

  def _handle_master_SpamFinished (self, event):
    print "Spam is ready!  Smells delicious!"

  def _handle_secondary_SpamFinished (self, event):
    print "Backup spam is ready.  Smells slightly less delicious."
Creating Your Own Event Types
As noted above, events are subclasses of revent.Event.  So to create an event, simply create a subclass of Event. You can add any extra attributes or methods you want.  Continuing our example:

class SpamStarted (Event):
  def __init__ (self, brand = "Hormel"):
    Event.__init__(self)
    self.brand = brand

  @property
  def genuine (self):
    # If it's not Hormel, it's just canned spiced ham!
    return self.brand == "Hormel"
Note that you should explicitly call the superclass's __init__() method!  (You can do this as above, or using the new-school super(MyEvent, self).__init__().)

Voila! You can now raise new instances of your event!

Note that in our handlers for SpamStarted events, we could have accessed the brand or genuine attributes on the event object that gets passed to the handler.

Raising Events
To raise an event so that listeners are notified, you just call raiseEvent on the object that will publish the event:

# One way to do it
chef.raiseEvent(SpamStarted("Generic"))

# Another way (slightly preferable)
chef.raiseEvent(SpamStarted, "Generic")
(The second way is slightly preferable because if there are no listeners, it avoids ever even creating the event object.)

Often, a class will raise events on itself (self.raiseEvent(...)), but as you see in the example above, this isn't necessarily the case.

Working with packets: pox.lib.packet
Lots of applications in POX interact with packets (e.g., you might want to construct packets and send them out of a switch, or you may receive them from a switch via an ofp_packet_in OpenFlow message).  To facilitate this, POX has a library for parsing and constructing packets.  The library has support for a number of different packet types.

Most packets have some sort of a header and some sort of a payload.  A payload is often another type of packet.  For example, in POX one generally works with ethernet packets which often contain ipv4 packets (which often contain tcp packets...).  Some of the packet types supported by POX are:

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
All packet classes in POX are found in pox/lib/packet.  By convention, you import the POX packet library as:

import pox.lib.packet as pkt
One can navigate the encapsulated packets in two ways: by using the payload attribute of the packet object, or by using its find() method. For example, here is how you could parse a ICMP message using the payload attribute:

def parseICMP(packet):
    if eth_packet.type == ethernet.IP_TYPE:
        ip_packet = eth_packet.payload
        if ip_packet.protocol = ipv4.ICMP_PROTOCOL
            icmp_packet = ip_packet.payload
...
This is probably not the best way to navigate a packet, but it illustrates the structure of packet headers in POX. At each level of encapsulation the packet header values can be obtained. For example, the source ip address of the ip packet above and ICMP sequence number can be obtained as shown:

...
src_ip = ip_packet.srcip
icmp_sequence = icmp_packet.seq
And similarly for other packet headers. Refer to the specific packet code for other headers.

A packet object's find() method can be used to find a specific encapsulated packet by the desired type name (e.g., "icmp") or its class (e.g., pkt.ICMP).  If the packet object does not encapsulate a packet of the requested type, find() returns None.  For example:

def handle_IP_packet (packet):
  ip = packet.find('ipv4')
  if ip is None:
    # This packet isn't IP!
    return
  print "Source IP:", ip.srcip
The following sections detail some of the useful attributes/methods/constants for some of the supported packet types.

Ethernet (ethernet)
Attributes:

dst (EthAddr)
src (EthAddr)
type (int) - The ethertype or ethernet length field.  This will be 0x8100 for frames with VLAN tags
effective_ethertype (int) - The ethertype or ethernet length field.  For frames with VLAN tags, this will be the type referenced in the VLAN header.
Constants:

IP_TYPE, ARP_TYPE, RARP_TYPE, VLAN_TYPE, LLDP_TYPE, JUMBO_TYPE, QINQ_TYPE - A variety of ethertypes
IP version 4 (ipv4)
Attributes:

srcip (IPAddr)
dstip (IPAddr)
tos (int) - 8 bits of Type Of Service / DSCP+ECN
id (int) - identification field
flags (int)
frag (int) - fragment offset
ttl (int)
protocol (int) - IP protocol number of payload
csum (int) - checksum
Constants:

ICMP_PROTOCOL, TCP_PROTOCOL, UDP_PROTOCOL - Various IP protocol numbers
DF_FLAG - Don't Fragment flag bit
MF_FLAG - More Fragments flag bit
TCP (tcp)
Attributes:

srcport (int) - Source TCP port number
dstport (int) - Destination TCP port number
seq (int) - Sequence number
ack (int) - ACK number
off (int) - offset
flags (int) - Flags as bitfield (easier to use all-uppercase flag attributes)
csum (int) - Checksum
options (list of tcp_opt objects)
win (int) - window size
urg (int) - urgent pointer
FIN (bool) - True when FIN flag set
SYN (bool) - True when SYN flag set
RST (bool) - True when RST flag set
PSH (bool) - True when PSH flag set
ACK (bool) - True when ACK flag set
URG (bool) - True when URG flag set
ECN (bool) - True when ECN flag set
CWR (bool) - True when CWR flag set
Constants:

FIN_flag, SYN_flag, etc. - Bits corresponding to flags
tcp_opt class
Attributes:

type (int) - TCP Option ID (probably corresponding constant below)
val (varies) - Option value
Constants:

EOL, NOP, MSS, WSOPT, SACKPERM, SACK, TSOPT - Option type IDs
Exemplo: ARP messages
You might want the controller to proxy the ARP replies rather than flood them all over the network depending on whether you know the MAC address of the machine the ARP request is looking for. To handle ARP packets in you should have an event listener set up to receive packet ins as shown:

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
            #send this packet to the switch
            #see section below on this topic
        elif packet.payload.opcode == arp.REPLY:
            print "It's a reply; do something cool"
        else:
            print "Some other ARP opcode, probably do something smart here"
See the l3_learning component for a more complete example of using the controller to parse ARP requests and generate replies.

Threads, Tasks, and Timers: pox.lib.recoco
This is a big subject, and a lot could be said.  Feel free to add something!

There's a small amount of example material in pox/lib/recoco/examples.py

Executing Code in the Future using a Timer
It's often useful to have a piece of code execute from time to time.  For example, you may want to examine the bytes transferred over a specific flow every 10 seconds.  It's also a fairly common case where you know you want something to happen at some specific time in the future; for example, if you send a barrier, you might want to disconnect the switch if 5 seconds go by without the barrier reply showing up.  This is the type of task that the pox.lib.recoco.Timer class is designed to handle – executing a piece of code at a single or recurring time in the future.

Note: The POX core object's callDelayed() is often an easier way to set a simple timer.  (See example below.)

Timer Constructor Arguments

arg	type - default	meaning
timeToWake	number (seconds)	
Amount of time to wait before calling callback (absoluteTime = False), or specific time to call callback (absoluteTime = True)
callback	callable (e.g., function)	A function to call when the timer elapses
absoluteTime	boolean - False	When False, timeToWake is a number of seconds in the future.  When True, timeToWake is a specific time in the future (e.g., a number of seconds since the epoch, as reported with time.time()).  Note that absoluteTime=True can not be used with recurring timers.
recurring	boolean - False	When False, the timer online fires once – timeToWake seconds from when it's started.  When True, the timer fires every timeToWake seconds.
args, kw	sequence, dict - empty	These are arguments and keyword arguments passed to callback.
scheduler	Scheduler - None	The scheduler this timer is executed with.  None means to use the default (you want this).
started	boolean - True	If True, the timer is started automatically.
selfStoppable	boolean - True	If True, the callback of a recurring timer can return False to cancel the timer.
Timer Methods

name	arguments	meaning
cancel	None	Stop the timer (do not call the callback again)
Exemplo - One-Shot timer

from pox.lib.recoco import Timer

def handle_timer_elapse (message):
  print "I was told to tell you:", message

Timer(10, handle_timer_elapse, args = ["Hello"])

# Prints out "I was told to tell you: Hello" in 10 seconds

# Alternate way for simple timers:
from pox.core import core # Many components already do this
core.callDelayed(10, handler_timer_elapse, "Hello") # You can just tack on args and kwargs.
Exemplo - Recurring timer

# Simulate a long road trip

from pox.lib.recoco import Timer

we_are_there = False

def are_we_there_yet ():
  if we_are_there: return False # Cancels timer (see selfStoppable)
  print "Are we there yet?"

Timer(30, are_we_there_yet, recurring = True)
Working with sockets: ioworker
pox.lib.ioworker contains a high level API for working with asynchronous sockets in POX.  Sends are fire-and-forget, received data is buffered and a callback fired when there's some available, etc.

TODO: Documentation and samples

OpenFlow in POX
One of the primary purposes for using POX is for developing OpenFlow control applications.  In this chapter, we describe some of the POX features and interfaces that facilitate this.

The POX component that actually communicates with OpenFlow switches is openflow.of_01 (the 01 refers to the fact that this component speaks OpenFlow wire protocol 0x01).  My default, of_01 registers an OpenFlow "nexus" on the core object as "openflow".  This is the main interface for working with OpenFlow in POX – you can use this OpenFlow nexus to send commands to switches and to receive messages from switches (in the form of events it raises – see the subsection below).

Every time a switch connects to POX, there is also an associated Connection object.  There is a lot of overlap between Connections and the Nexus.  Either one can be used to send a message to a switch, and most events are raised on both.  Sometimes it's more convenient to use one or the other.  If your application is interested in events from all switches, it may make sense to listen to the Nexus, which raises events for all switches.  If you're interested only in a single switch, it may make sense to listen to the specific Connection.

There are three major ways to get a reference to a Connection object:

You can listen to ConnectionUp events on the nexus – these pass the new Connection object along
You can use the nexus's getConnection(<DPID>) method to find a connection by the switch's DPID
You can enumerate all of the nexus's connections via its connections property (e.g., for con in core.openflow.connections)
OpenFlow Events: Responding to Switches
Note: For more background on the event system in POX, see the relevant section in this manual.

Most OpenFlow related events are raised in direct response to a message received from a switch.  As a general guideline, OpenFlow related events have the following three attributes:

attribute	type	description
connection	Connection	Connection to the relevant switch (e.g., which sent the message this event corresponds to)
dpid
long	Datapath ID of relevant switch (use dpid_to_str to format)
ofp	ofp_header subclass	OpenFlow message object that caused this event.  See OpenFlow Messages for info on these objects


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

attribute	type	notes
connection	Connection	This object can be used for communication to the switch.  For example, you can use it to send OpenFlow commands, or you can listen to events sourced by this Connection object in order to handle messages sent by the switch.
dpid	integer/long	This is the DPID associated with the switch.  Use util.dpidToStr() to display it.
ofp	ofp_switch_features	Contains information about the switch, for example supported action types (e.g., whether field rewriting is available), and port information (e.g., MAC addresses and names).  (This is also available on the Connection's features attribute.)
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
Raised when a flow entry has been removed from a flow table. This may either be because of a timeout or because it was removed explicitly.  Such notifications are sent when the flow was installed with the OFPFF_SEND_FLOW_REM flag set.  See the OpenFlow specification for further details.

idleTimeout (bool) - True if expired because of idleness 
hardTimeout (bool) - True if expired because of hard timeout 
timeout (bool) - True if either of the above is true 
deleted (bool) - True if deleted explicitly 
Statistics Events
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

OpenFlow Messages
OpenFlow messages are how OpenFlow switches communicate with controllers.  The messages are defined in the OpenFlow Specification.  There are multiple versions of the specification; POX currently supports OpenFlow version 1.0.0 (wire protocol version 0x01).

POX contains classes and constants corresponding to elements of the OpenFlow protocol, and these are defined in the file pox/openflow/libopenflow_01.py (the 01 referring to the wire protocol version). For the most part, the names are the same as they are in the specification.  In a few instances, POX has names which we think are better.  Additionally, POX defines some classes do not correspond to specific structures in the specification (the specification does not describe structs which are just a plain OpenFlow header only differentiated by the message type attribute – POX does).  Thus, you may well wish to refer to the OpenFlow Specification itself in addition to this document (and, of course, the POX code and pydoc/Sphinx reference).

A nice aspect of POX's OpenFlow library is that many fields have useful default values or can infer values.

In the following subsections, we will discuss a useful subset of POX's OpenFlow interface.

TODO: Redo following sections to have tables of values/types/descriptions rather than snippets from init functions.

ofp_packet_out - Sending packets from the switch
The main purpose of this message is to instruct a switch to send a packet (or enqueue it).  However it can also be useful as a way to instruct a switch to discard a buffered packet (by simply not specifying any actions).

attribute	type	default	notes
buffer_id	int/None	None	ID of the buffer in which the packet is stored at the datapath. If you're not resending a buffer by ID, use None.
in_port	int	OFPP_NONE	Switch port that the packet arrived on if resending a packet.
actions	list of ofp_action_XXXX	[ ]	If you have a single item, you can also specify this using the named parameter "action" of the initializer.
data	bytes / ethernet / ofp_packet_in	''	
The data to be sent (or None if sending an existing buffer via its buffer_id).

If you specify an ofp_packet_in for this, in_port, buffer_id, and data will all be set correctly – this is the easiest way to resend a packet.

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

# When we get flow stats, print stuff out
def handle_flow_stats (event):
  web_bytes = 0
  web_flows = 0
  for f in event.stats:
    if f.match.tp_dst == 80 or f.match.tp_src == 80:
      web_bytes += f.byte_count
      web_flows += 1
  log.info("Web traffic: %s bytes over %s flows", web_bytes, web_flows)

# Escutar status de fluxo
core.openflow.addListenerByName("FlowStatsReceived", handle_flow_stats)

# Requisitar de fato status de fluxo de todos os switches
for con in core.openflow.connections: # make this _connections.keys() for pre-betta
  con.send(of.ofp_stats_request(body=of.ofp_flow_stats_request()))

Estrutura de combinação

OpenFlow define uma estrutura de combinação – ofp_match – que permite a você definir um conjunto de cabeçalhos que serão combinados. Você pode tanto criar do zero ou usar um facory method para criar baseado em um pacote pacore já existente.

A estrutura de combinação é definida em pox/openflow/libopenflow_01.py na classe ofp_match.  Seus atributos são derivados dso membros existentes na especificação do OpenFlow, então dirija-se à ela para maiores informações. Ainda assim, ela está resumida na tabela abaixo:

Atributos de ofp_match :

Attributo	Significado
in_port	Porta do switch que o pacotes chegou
dl_src	Endereço de origem Ethernet
dl_dst	Endereço de destino Ethernet
dl_vlan	VLAN ID
dl_vlan_pcp	VLAN prioridade
dl_type	Ethertype / tamanho (e.g. 0x0800 = IPv4)
nw_tos	IP TOS/DS bits
nw_proto	IP protocolo (e.g., 6 = TCP) or lower 8 bits of ARP opcode
nw_src	Endereço de origem IP 
nw_dst	Endereço de destino IP 
tp_src	Porta de origem TCP/UDP 
tp_dst	Porta de destino TCP/UDP 

Atributos podem ser especificados tanto sobre um objeto como durante sua inicialização.  Isto é, os seguintes são equivalentes:

my_match = of.ofp_match(in_port = 5, dl_dst = EthAddr("01:02:03:04:05:06"))
#.. ou ..
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

Exemplo: Sending a PacketOut
In a similar manner to a flow mod, one must first define a packet out as shown here:

msg = of.ofp_packet_out(in_port=of.OFPP_NONE)
msg.actions.append(of.ofp_action_output(port = outport))
msg.buffer_id = <some buffer id, if any>
connection.send(msg)
The inport is set to OFPP_NONE because the packet was generated at the controller and did not originate as a packet in at the datapath.

Third-Party Tools, Tutorials, Etc.
This section attempts to note some projects which use POX but are not part of POX itself.  (This may get moved to its own page or something in the future.)

POXDesk: A POX Web GUI


This is a side-project of Murphy's in a very early state.  It runs on the betta branch of POX and provides a number of features: a flow table inspector, a log viewer, a simple topology viewer, a terminal, an L2 learning switch implemented in JavaScript, etc.  It's implemented using the Qooxdoo JavaScript framework on the front end, and POX's web server and messenger service on the backend.

Site: https://github.com/MurphyMc/poxdesk/wiki

Blog post with more info and screenshots: http://www.noxrepo.org/2012/09/pox-web-interfaces/

OpenFlow Tutorial
The OpenFlow Tutorial has a POX version which guides the reader through setting up a test environment using Mininet and implementing a hub and learning switch, among other things.

Site: http://www.openflow.org/wk/index.php/OpenFlow_Tutorial

OpenFlow Switch Tutorial
These are examinations of some different ways to write "switch" type applications in OpenFlow with POX.  Prepared by William Emmanuel Yu.

OpenFlow Switch Tutorial (of_sw_tutorial.py) - this is a simple OpenFlow module with various switch implementations. The following implementations are present:
Dumb Hub - in this implementation, all packets are sent to the controller and then broadcast to all ports. No flows are installed.
Pair Hub - Flows are installed for source and destination MAC address pairs that instruct packets to be broadcast.
Lazy Hub - A single flow is installed to broadcast packet to all ports for any packet.
Bad Switch - Here flows are installed only based on destination MAC addresses. Find out what the problem is!
Pair Switch - Simple to a pair hub in that it installs flows based on source and destination MAC addresses but it forwards packets to the appropriate port instead of doing a broadcast.
Ideal Pair Switch - Improvement of the above switch where both to source and to destination MAC address flows are installed. Why is this an improvement from the above switch?
OpenFlow Switch Interactive Tutorial (of_sw_tutorial_oo.py) - this is the same module above that is Interactive. If run with a pox py command line. Users can dynamically load and unload various switch implementations.
Sample Interactive Switch Session
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


OpenFlow Switch Tutorial for Betta (of_sw_tutorial_resend.py) - this is the same module as above but takes advantage of resend functionality in the betta branch.
Flow Statistics Collector Exemplo
Prepared by William Emmanuel Yu.

Flow Statistics Collector Exemplo (flow_stats.py) - this module collects statistics every 5 seconds using a timer. There are three (3) kinds of statistics collected: port statistics, flow statistics and a sample displaying on web statistics (similar to an example below). This complete port and flow statistics requires the pox.openflow.of_json module in the betta branch of Pox.
Coding Conventions
The Style Guide for Python Code (AKA PEP 8) outlines some conventions for developing in Python.  It's a good baseline, though POX does not aim for strict conformance.  Here are some guidelines for writing code in POX, especially if you'd like to have it merged.  Note that in some cases they are in addition to or differ from PEP 8.  Also note that the most important guideline is that code is readable.  Also also note that the code in the repository does not entirely conform with the below guidelines (pull requests that improve consistency are very welcome!).

Two spaces for indentation
Line wrapping / line length:
Maximum of 79 characters, though 80 won't kill us.
Use implicit line joining (e.g. "hanging" parentheses or brackets) rather than the explicit backslash line-continuation character unless the former is very awkward
Continue lines beneath the appropriate brace/parenthesis (Lisp style) when that works well. When it doesn't, *my* preference is to indent *a single space*, though I know that drives a lot of Python coders crazy, so I'm hesitant to set a specific rule here as long as it's clear.  The basic rule is that it should be either more or less indentation than usual -- i.e., don't use two spaces for a continued line.  I am more and more using four spaces.
Two blank lines separate top-level pieces of code with structural significance (classes, top level functions, etc.). You can use three for separating larger pieces of code (though when one is tempted to do this, it's always a good idea to ask oneself if the two larger pieces of code should be in separate files).   Methods within a class should be one or two, but should be consistent within the particular class.
Put a space between function name and opening parenthesis of parameter list.  Similar for class and superclass.  That is: "def foo (bar):", not "def foo(bar):".
Use only new-style classes.  (This means inherit from object.)
Docstrings:
Either stick to """ one line """, or have the opening and closing """ on lines by themselves.
Naming
Classes should generally by InitialCapped.
Methods and other attributes should be lower_with_underscores.  Note that this is currently violated all over the place (though it's getting better in the betta branch).
"Private" members (which you explicitly don't want others relying on) should start with an underscore.
"constants" should be UPPER_WITH_UNDERSCORES
The keyword arguments catch-all variable is called kw (against Python convention of kwargs)
FAQs
How can I change the OpenFlow port from 6633?
If you turn on verbose logging, you'll see that it's the openflow.of_01 module which listens for connections.  That's the hint: it's this component that you need to reconfigure.  Do so by passing a "port" argument to this component on the commandline:

./pox.py openflow.of_01 --port=1234 <other commandline arguments>
How can I have some components start automatically every time I run POX?
The short answer is that there's no supported way for doing this.  However, it's pretty simple to just create a small component that launches whatever other components you want.

For example, let's say you were tired of always having to remember the following commandline:

./pox.py log.level --DEBUG samples.pretty_log openflow.keepalive --interval=15 forwarding.l2_pairs
But writing the following component in ext/startup.py, you can replace with above with simply:

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
How do I get switches to send complete packet payloads to the controller?
By default, when a packet misses all the entries in the flow table, only the first X bytes of the packet are sent to the controller.  In POX, this defaults to OFP_DEFAULT_MISS_SEND_LEN, which is 128.  This is probably enough for, e.g., ethernet, IP, and TCP headers... but probably not enough for complete packets.  If you want to inspect complete packet payloads, you have two options:

Install a table entry.  If you install a table entry with a packet_out to OFPP_CONTROLLER, POX will have the switch send the complete packet by default (you can manually set some smaller number of bytes if you want).
Change the miss send length.  If you set core.openflow.miss_send_len during startup (before any switches connect), switches should send that many bytes when a packet misses the whole table.
Check the misc.packet_dump component for an example.

How can I communicate between components?
Components are just Python packages and modules, so one way you can do this is the same way you communicate between any Python modules – import one of them and access its top-level variables and functions.

POX also has an alternate mechanism, which is described more in the section Working with POX: The POX Core object.

How can I use POX with Mininet?
Use the remote controller type.  For example, if you are running POX on the same machine as Mininet:

mn --topo=linear --mac --controller=remote
A common configuration is to run Mininet in a virtual machine and run POX in your host environment.  In this case, point Mininet at an IP address of the host environment.  If you're using VirtualBox and a "Host-only Adapter", this is the address assigned to the VirtualBox virtual adapter (e.g., vboxnet0).  For example:

mn --topo=linear --mac --controller=remote --ip=192.168.56.1
I'm seeing many packet_in messages and forwarding isn't working; what gives?
This problem is often seen when attempting to use one of the "learning" forwarding components (l2_learning, l2_multi, etc.) on a mesh or other topology with a loop.  These forwarding components do not attempt to work on loopy topologies.

The spanning_tree component is meant to be a fairly generic solution to this problem, so you might try running it as well.  It can also be helpful to prevent broadcasts until discovery has had time to discover the entire topology.  Some components (such as l2_learning) have an option to enforce this.

My code isn't working!  Can you help me?
Possibly.  There are a number of things you can do to help yourself, though.  If none of those work or apply, there are a number of things you can do to help us help you.  Here are some things you might try:

Read the logs.  If the logs don't seem to say anything useful, try reading them at a lower log level, such as the DEBUG level.  That way, you'll get all the log messages.  Do this by adding log.level --DEBUG to your commandline.  See the log.level component section for more info on adjusting log levels.
Look at the OpenFlow traffic.  If you don't seem to be getting an event that you think you should be, or you think you're sending messages but the switch doesn't seem to be responding to, or anything else where you think there's a breakdown in communication between a switch and POX, it can be helpful to look at what actually got put on the wire.  There is an OpenFlow dissector for Wireshark (you can Google for it).  You can either run it as usual, or you can use POX's openflow.debug component to generate synthetic traces which show exactly what POX thinks it saw – one message per synthetic "packet" (which makes the Wireshark list easier to read).
Run a newer version.  Particularly, if you are running a release branch, you might think about running the active branch instead.  While active branches may contain new problems, they also fix old ones!  See the "Selecting a Branch / Version" section for more information.
Search the mailing list archive.  This would be more helpful if it weren't so flaky.  Sorry about that, hopefully we'll get around to fixing it before too long!
If none of those work, you might try posting to the pox-dev mailing list (sign up at http://www.noxrepo.org/community/mailing-lists/).  When you do, you'll probably get better results the more you can do the following:

Post the commandline with which you invoked POX.
Post the POX log.  It's probably a good idea to post them at DEBUG level.  Even if you didn't see anything in the log, it may be helpful to someone else.  The first part of the log (before the Up message) is especially useful, as it tells which operating system and Python interpreter you are running, and many components announce themselves.  If you don't post this, you might at least try to include some of this information yourself.
Post which version of POX you are using.  Did you just do a git clone http://github.org/noxrepo/pox, or did you switch branches?  Did you do this recently or are you potentially using an older version?
Post what kind of switches you are using.  Are you running POX with Mininet?  Which version?  What commandline did you use to invoke Mininet?  If you're running with hardware switches, what kind?  If you're running with a software switch, which one and which version?
Post code which illustrates the problem.  A minimal example is great, but anything is better than nothing.
Post what you've tried already.  Hopefully you've tried to address the issue yourself.  What have you tried and what were the results?
Doing the above makes it easier for people to help you, and also potentially saves time – if you don't do the things mentioned above, it's quite possible that the first suggestions you get from the mailing list will be to try the things mentioned above!
