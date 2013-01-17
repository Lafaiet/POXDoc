# POX


[* Installing POX](github.com/Lafaiet/POXDoc/blob/master/PT_BR/installing.md)

  * Requirements
  * Getting the Code
  * Selecting a Branch / Version
  * PyPy Support

* Invoking POX

* Components in POX
  * Stock Components
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
      * Disabling the Console Log
      * Log Formatting
      * Log Output
    * log.color
    * log.level
    * samples.pretty_log
    * tk
    
* Developing your own Components
  * The "ext" directory
  * The launch function
    * A Simple Example
    * Multiple Invocation
* POX APIs
  * Working with POX: The POX Core object
    * Registering Components
  * Working with Addresses: pox.lib.addresses
  * The Event System: pox.lib.revent
    * Handling Events
       * Event Handlers
       * Listening To an Event
       * Automatically Setting Listeners
    * Creating Your Own Event Types
    * Raising Events
  * Working with packets: pox.lib.packet
    * Ethernet (ethernet)
    * IP version 4 (ipv4)
    * TCP (tcp)
       * tcp_opt class
    * Example: ARP messages
  * Threads, Tasks, and Timers: pox.lib.recoco
    * Executing Code in the Future using a Timer
  * Working with sockets: ioworker
* OpenFlow in POX
  * OpenFlow Events: Responding to Switches
    * ConnectionUp
    * ConnectionDown
    * PortStatus
    * FlowRemoved
    * Statistics Events
    * PacketIn
    * ErrorIn
    * BarrierIn
  * OpenFlow Messages
    * ofp_packet_out - Sending packets from the switch
    * ofp_flow_mod - Flow table modification
       * Example: Installing a table entry
       * Example: Clearing tables on all switches
    * ofp_stats_request - Requesting statistics from switches
       * Example - Web Flow Statistics
  * Match Structure
       * Define a match from an existing packet
  * OpenFlow Actions
       * Output
       * Enqueue
       * Set VLAN ID
       * Set VLAN priority
       * Set Ethernet source or destination address
       * Set IP source or destination address
       * Set IP Type of Service
       * Set TCP/UDP source or destination port
  * Communicating with a Datapath (Switch)
       * Connection Objects
       * Example: Sending a FlowMod
       * Example: Sending a PacketOut
* Third-Party Tools, Tutorials, Etc.
   * POXDesk: A POX Web GUI
   * OpenFlow Tutorial
   * OpenFlow Switch Tutorial
   * Flow Statistics Collector Example
* Coding Conventions
* FAQs
   * How can I change the OpenFlow port from 6633?
   * How can I have some components start automatically every time I run POX?
   * How do I get switches to send complete packet payloads to the controller?
   * How can I communicate between components?
   * How can I use POX with Mininet?
   * My code isn't working!  Can you help me?