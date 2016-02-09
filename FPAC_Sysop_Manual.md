# 1.0 Introduction #

The ROSE protocol is based on the X.121 addressing scheme. It allows fast and efficient packet routing with a very small protocol overhead (only three bytes). FPAC is based on the ROSE protocol and has been written for the Linux OS. Linux FPAC is an extension of the TNC based ROSE switch and the DOS FPAC switch.

FPAC includes a white pages system that records the address of each node and user on the system. This information can also be exchanged with every other connected FPAC node in the system. This allows users to connect to any KNOWN callsign without having to give any node or routing information.

## 1.1 Version of FPAC ##

This document refers to FPAC version >=3.27.14.

## 1.2 Updates ##

New versions of this document are available from:

http://fpac.googlecode.com

## 1.3 Related Documentation ##

There are many documents that relate to Linux and Packet Networking. I suggest you do a [Web Search](http://www.google.com) to see what is available.

The two references below are excellent examples of what has been written.

  * Terry Dawson's (VK2KTJ) AX25 How-To http://www.linuxdocs.org/HOWTOs/AX25-HOWTO.html
  * Bernard Pidoux's (F6BVP) Mini How-To http://rose.fpac.free.fr/MINI-HOWTO/

## 1.4 FPAC Source ##

The latest FPAC source package may be downloaded from:

http://fpac.googlecode.com


# 2.0 FPAC Tools #

The FPAC distribution includes a set of tools. Here is a brief description of each of them.

## 2.1 fpad ##

Fpad is the main piece of code. It makes the interface between AX.25 and ROSE.

It allows User connections from ax.25 to the network and vice-versa in pseudo-digipeating mode, without first connecting to the FPAC node application.

## 2.2 fpacnode ##

Fpacnode is the User interface. It acts like the Linux Node application. In fact, many parts of the code have been taken from Linux Node. Additions have been made for the ROSE, NetRom, TCPIP and FlexNet protocol support. Fpacnode also gathers and supplies statistics on the system.

Fpacnode has a built-in set of commands. They can be disabled or replaced and new commands may be added. A second set of commands is available when entering the Sysop Mode.

The Sysop Mode is accessed via the SYS (Sysop) command.

The Sysop Mode is password protected. The password to enter the Sysop Mode may be plain text, NetRom, or MD2/MD5 keys.

Built in commands
```
A     Displays the list of aliases. An alias allows you to connect any call sign (even remote) using a local call sign. For instance call sign F6FBB-3 could route to the cluster F6KRQ-3 through the 	 	network. Aliases may be also directly connected to by the user without using fpacnode.

L     Displays the list of L3 links (trunks)

M <port>    Displays a list of the last 20 heard stations. Giving a <port> as first argument will list the stations only for that port. Note the underlying process that supplies this is the mheard application, which must be running.

N     Displays a list of known Linux FPAC switches. A list of call signs may be given as an argument, or a star to get a long format display.

Ne    Displays a list of known NetRom nodes.

P     Displays a list and description of available ports on this node.

R     Displays information on addresses and the routing to other FPAC nodes.

S     Displays a status of the Linux and FPAC resources.

U     Displays a list of the Users of the ROSE/FPAC system.

SYS   Switches to SYSOP mode

W     Displays the FPAC White Pages database. Arguments to the command allow database queries.
```

## 2.3 fpacshell ##

Fpacshell opens a Linux shell session to the User in the Sysop Mode. This is allows maintenance with **root** privileges. For security reasons a password, accepting plain text, NetRom key, or MD2/MD5 key is provided.

Fpacshell is enabled from the Sysop Command block of the FPAC configuration file.

See Sysop section 5.4.

## 2.4 fpacstat ##

Fpacstat is the statistics applications. It computes the traffic statistics of the trunks (L3 links between switches) and updates the last-hour stats in /var/ax25/fpac/fpacstat.dat every minute.

The last-day file, /var/ax25/fpac/fpacstat.day, is updated at the end of every hour.

These files can be read at any time and sent to the user via a command defined in the command section.

The "-c" option writes `<CR>` characters instead of `<LF>` to be compatible with the packet radio protocol in which `<CR>` characters are used as end of line.

## 2.5 fpacwpd ##

Fpacwpd is the white pages database server daemon. It will respond to any local or remote query. The database is stored in /var/ax25/fpac/fpacwp3.dat.

To get a report on this server, connect to WP via ROSE (or to the L3 callsign) and type ":" (colon).

```
     Example: c wp 3100352726

     After the connection, type a :. The server will dump its current status. Type return to end the connection. 
```

For normal WP queries use the WP command.

## 2.6 fpaccfg ##

Fpaccfg will dump the contents of the fpac.conf file to stdout. It can be used in a node command to view the config file on the node.

## 2.7 fpacroute ##

Fpacroute is the daemon used to execute the Trace command.

## 2.8 fpacwdog ##

Fpacwdog sends a `<cr>` to a specified serial port.

## 2.9 mailbbs ##

Mailbbs is a simple mailer. It is used to send service messages from the FPAC node to a BBS.

## 2.10 wpserv ##

Wpserv dumps the contents of the white pages database file to stdout.

## 2.11 wpedit ##

Wpedit allows you to change or delete WP entries.

The syntax is: `wpedit <call>`
```
     # wpedit w1aw
     Callsign	Status	Last Update		DNIC	Address	Type	Digis
     W1AW				12/12/10 02:00 =>	3100	808323	Node	

     WP-Editing W1AW : (A)ddress (D)igi (N) Toggles node attribute (R)emove (U)ndelete (B)ye
```

## 2.12 wpcmp ##

Wpcmp compares two database files (fpacwp3.dat) and dumps the results to stdout.

The syntax is `wpcmp <file1.dat> <file2.dat>`

## 2.13 wpmaint ##

Wpmaint is a database repair utility. The existing white pages database is read, and a new database is created.

## 2.14 monitor ##

Monitor is the equivalent of the ax25-tool listen, the exception being that monitor is terminated with a `<cr>` rather than a `<ctrl-c>`. This allows it to be called from within fpacnode to monitor traffic and then be exited gracefully with a `<cr>`.

## 2.15 calibrate ##

Calibrate will send UI frames out a specified axport until `<cr>` is pressed. The port name to use has to match a port listed in axports.

This allows calibration of KISS devices that are attached to the system with kissattach.

## 2.16 remote ##

This is a simple "remote control" program that lets you manually toggle the pins of a parallel port.


# 3.0 Installation of FPAC #

ROSE is configured by the rsports file in /etc/ax25/ (or /var/ax25 depending on your distro).

This file will be created and replaced each time fpad is run.

Nothing else is needed other than loading the rose module, fpad takes care of writing the file based on all the FPAC config files.

## 3.1 Binaries ##

Depending on your distribution and/or compile options, the FPAC binaries may be located in /usr/local/sbin and /usr/local/bin directory or /usr/sbin and /usr/bin.

If you decide to change their location, check the fpac.sh and/or ax25-up script and make sure they are looking in the right place.

## 3.2 List of FPAC Files ##

```
calld           Receives calls from a remote BBS via TCP
flexd           Download and handle Flexnet destinations database
fpac.sh         Shell script to start/stop fpac
fpaccfg         Dumps the configuration of fpac
fpacnode        The FPAC node
fpacroute       Daemon for traceroute
fpacshell       Runs a shell with MD5 protection
fpacstat        Computes the traffic statistics of the trunks
fpacwdog        Simple watchdog that sends a character to a serial port
fpacwpd         The white pages database server daemon
fpad            The FPAC pad. Access from/to AX25/NetRom/ROSE
mailbbs         Simple mailer to send FPAC's services messages to a BBS
monitor         Monitor the traffic on a port(s)
remote          Parallel port control program
wpcmp           WP database compare utility
wpedit          WP database callsign editor
wpmaint         WP maintenance utility
wpserv          WP database dump utility
wpstress        ** Obsolete - WP utility
```

## 3.3 Directories ##

The directories used depend on your OS and/or compile time options. The configuration files are typically in /etc/ax25 and the help files in /var/ax25/fpac/help/.

The help files are called for help on internal commands within fpacnode. See Section 6.4 on the Help files.

The data directory is /var/ax25/fpac/. Data files will be created at FPAC runtime (stats files, white pages database, etc).


# 4.0 Starting FPAC #

## 4.1 fpac.sh ##

This shell script file is used to start, stop, restart and display the status of the various parts of the FPAC node. Fpac.sh is typically located in /usr/local/sbin.

In most systems there is a sym-link, fpac, placed in /usr/sbin/ which points to /usr/local/sbin/fpac.sh.

## 4.2 fpac.sh During Linux Start-up ##

To start fpac, run /usr/sbin/fpac.sh -start from your rc.local or start-up file. There is no need to add an & (ampersand) at the end of the line. Fpad appications are daemons. The Ax25 layer must be fully initialized before FPAC can be started.

## 4.3 fpac.sh Options ##

The options for fpac.sh are: `-start`, `-stop`, `-restart` and `-status`.

If a co-located BBS is available, a message function is available to send a packet message to the Sysop informing them of a restart.

## 4.5 ax25/ax25-up/ax25-down ##

These files are typically used on Debian-type systems.

ax25 can be placed in /etc/init.d and used to call ax25-up and ax25-down as required.

The ax25-up script can be modified to start the whole ax.25 subsystem as well as FPAC.

ax25-down is used to shut down the ax.25 subsystem.


# 5.0 Configuration of FPAC #

FPAC is configured primarily by fpac.conf which must be located in the /etc/ax25 directory (or similar depending on your OS).

## 5.1 The fpac.conf Configuration File ##

This file is made of keywords to which are given values or blocks of values. A block is always ended by a "End" keyword. Keywords are not case dependent, but values are often are. Blank lines are allowed and comments must be proceeded by a # (hash/octothorp).

Here is an example of fpac.conf:
```
    # FPAC configuration file
    # Information about the node
    L2call    = F6FBB-10
    L3call    = F6FBB-11
    DNIC      = 2080
    Address   = 931502
    Coverage  = 931602 931202 931102
    InetPort  = 10093
    Password  = mypassword
    City      = Roquettes
    State     =
    Country   = France
    Locator   = JN03QL
    UserPort  = *
    DefPort   = 0
    #
           
    # Other commands
    Command
      BBs = /usr/sbin/call_k4gbb 3100352726
      CLuster = connect f6krq-3 2080931501
      STat = /bin/cat /var/ax25/fpac/fpacstat.dat 				              		   /var/ax25/fpac/fpacstat.day
      CONVers = /usr/sbin/convers -l %u
      Telnet =
    End

    # Other SysOp commands

    Sysop
      RM   = /bin/rm /tmp/%1
      LS   = /bin/ls -l /tmp/%1
      CAT  = /bin/cat /tmp/%1
      YGET = /usr/sbin/yapp -u /tmp/%1
      YPUT = /usr/sbin/yapp -d /tmp/%1
      DIsk = /bin/df -k/usr/sbin/fpacshell
    End

    # Other applications
    Application
            F6FBB-1 = /usr/sbin/call_tcp server 1022
    End


    # Additional user ports
    AddPort = UHF
            Address = 931402
            Port = 5
    End

    AddPort = BEX
            Address = 931302
            Port = 0
    End

    # List of users on specific port
    User = F6BEX
            Path = F6BEX
            Port = 2
    End

    # List of aliases
    Alias = F6FBB-2
            Path = F6FBB-2,931501
    End

    Alias = F6FBB-3
            Path = F6KRQ-3,931501
    End

    # List of adjacent nodes
    # Nodes may instead be listed in their own fpac.nodes file
    Node = Prat
            Path     = F5JTU-9
            DNIC     = 2080
            Address  = 909501
            Port     = 3
    End

    Node = Rebigue
            Path     = F6BEX-9
            DNIC     = 2080
            Address  = 931501
            Port     = 0
    End

    Node = Test
            Path     = F6FBB-9
            DNIC     = 2080
            Address  = 931503
            Port     = 5
    End

    # Routes to adjacent nodes
    # Routes may instead be listed in their own fpac.routes file
    Routes
            DNIC = 2080

            1 = Rebigue
            2 = Rebigue
            3 = Rebigue
            4 = Rebigue
            5 = Rebigue
            6 = Rebigue
            7 = Prat
            8 = Rebigue
            909 = Prat
            909202 = Rebigue
            909502 = Rebigue
            931201 = Rebigue
            931501 = Rebigue
            931203 = Test
            931503 = Test
            911 = Prat
            930 = Prat
            934 = Prat
            946 = Rebigue
            965 = Rebigue
            966 = Prat
            981 = Rebigue
            981501 = Prat
            982 = Rebigue
    End
```

## 5.2 Header Information ##

All the keywords of the header are mandatory and must be present.

```
    # Information about the node

    L2call    = F6FBB-10
    L3call    = F6FBB-11
    DNIC      = 2080
    Address   = 931502
    Coverage  = 931602 931202 931102
    InetPort  = 10093
    Password  = mypassword
    City      = Roquettes
    State     =
    Country   = France
    Locator   = JN03QL
    UserPort  = 1 2
    DefPort   = 0
```

  * L2call is the main call sign seen by users. Connecting this call sign will give access to fpacnode. Using this call sign as digipeater will give access to local or remote station via the network.

  * L3call is the call sign used to transport the X25 frames. This call sign should not be connected by users (no text or other thing is sent). It will be used by remote switches to transport the X25 circuits with the PID 01 (X25). Using this call sign as digipeater will give a silent connection and disconnection, no message or report will be sent. Mostly interesting for machines such as a BBS.

  * DNIC is the country designator. It follows the CCITT standard. It is 2080 for France, 3020 for Canada, 3100 for USA. Some designators are given in the appendix.

  * Address is the numerical (ROSE) reference of the switch. This address is generaly copied from the telephone number hierarchy.

  * Coverage is the list of addresses that a switch will answer to in addition to the base address. It may be blank, if desired.

  * InetPort is the port number (decimal) listened by FPAC for UDP connections from other nodes (through ax25ipd).

  * Password is the word or sentence to compute the password for the SYS command.

  * City, State, and Country are the location of the switch. Up to 22 characters for City, State, and Country names.

  * Locator is the Maidenhead Grid Square location of the switch.

  * UserPort is a list of ports available for the user's connections. A `*` (asterisk) enables all ports. Link ports should NOT be enabled as a User Port.

  * DefPort is the default port for connections when no port is defined. For an internet connected node, this typically would be the port associated with ax25ipd.

## 5.3 Command Section ##

There are several commands defined within the FPAC code. The Command section adds or removes (by redefining) commands.

These commands will be executed with the **root** privileges, so avoid potentially destructive commands!

The Command section includes a list of keywords which will appear in the main menu of fpacnode.

If no value is given to the keyword the command will be deleted from the default commands.

A keyword identical to a default command will redefine it. The default command will be replaced by the new one.

Upper case characters are the mandatory characters to be received for the command to be interpreted.

The value is a command which can be executed by Linux. The user will be linked to this command, sending what he types and receiving the answers, until the commands ends. The user will be returned to the fpacnode menu.

In the case of the Connect command, a L2 or L3 connection is established. If a port is specified a L2 connection will be established. If no port is specified, a L3 NetRom connection will be established. NetRom must be initialized and the call sign is a known.

A ROSE address must be specified to establish a L3 ROSE connection.  If no address is specified then the WP Database will be searched for a route. If the FPAC white pages database is unable to supply a route the connection will fail.

```
    # Other commands
    Command
            BBs = /usr/sbin/call_tcp serveur 1022
            CLuster = connect f6krq-3 2080931501
            STat = /bin/cat /var/ax25/fpac/fpacstat.dat /var/ax25/fpac/fpacstat.day
            CONVers = /usr/sbin/convers -l %u
            Telnet =
    End
```

## 5.4 Sysop Section ##

The Sysop section adds commands to the menu when the User enters the Sysop Mode. These are generally, maintenance commands.

These commands will be executed with the **root** privileges!

The SHell command enables a Linux shell via fpacshell.

```
    # Other SysOp commands
    Sysop
            DIsk = /bin/df -k
            RM   = /bin/rm /tmp/%1
            LS   = /bin/ls -l /tmp/%1
            CAT  = /bin/cat /tmp/%1
            YGET = /usr/sbin/yapp -u /tmp/%1
            YPUT = /usr/sbin/yapp -d /tmp/%1
            SHell= /usr/sbin/fpacshell
    End
```

## 5.5 Application Section ##

The Application section includes a list of lines which associates a Linux application to a call sign. When a the user connects the specified call sign, they will be linked to the associated Linux program.

```
    # Other applications
    Application
            Telpac-10 = /usr/local/sbin/telpac_node %S
    End
```

In the above example, if a user connects to TELPAC-10, they will be connected to the telpac application.

## 5.6 AddPort Section ##

The AddPort section may contain one or more AddPort blocks. Each block will specify an address and a port. The block must be closed with an End statement.

This assigns a ROSE address to that port. Only User ports should be assigned a address.

```
   # List of routes to port
    AddPort = UHF
            Address = 931402
            Port = 1
    End

    AddPort = BEX
            Address = 931302
            Port = 0
    End
```

For example, if your node has a VHF port and a UHF port, you would define those each in an AddPort block, give them an address, and associate them with the axport they are connected to.

## 5.7 User Section ##

The User section may contain one or more User blocks. Each block will specify a user associated to a port. This allows connects to a User call sign on a link port. Otherwise, only stations specified in the Adjacent Nodes Blocks are allowed to connect to a link port.

Link ports are ports NOT specified in the UserPort statement in the header. Users specified in this block are usually a BBS or other Service that are listening on a link port.

```
    # List of users on specific port
    User = F6BEX
            Path = F6BEX
            Port = 2
    End
```

## 5.8 Alias Section ##

The Alias section is similar to Application section, but redirects the connection request.

For instance, the user connection F6FBB-3 will be re-routed to F6KRQ-3 address 931501 (the default DNIC of the switch will be used).

The Alias section may contain one or more Alias blocks. One block for each alias required.

```
    # List of aliases
    Alias = F6FBB-2
           Path = F6FBB-2,931501
    End

    Alias = F6FBB-3
           Path = F6KRQ-3,931501
    End
```

## 5.9 Node Section ##

This section may be moved to a separate file /etc/ax25/fpac.nodes, if desired. If a separate file is used, omit this section from fpac.conf.


The Node section may contain a maximum of 32 Node blocks. Each block will specify one adjacent (directly connected) switch. The node may be a ROSE TNC stack or a DOS FPAC node.

Use as many blocks as needed in this section.

```
    # List of adjacent nodes
     Node = North
            Path     = F6BEX-9
            DNIC     = 2080
            Address  = 931501
            Port     = 9
		 NoWp     = 0
    End
    Node = South
            Path     = K4GBB-9
            DNIC     = 3100
            Address  = 352726
            Port     = 8
 		 NoWp     = 1
    End
```

Each block begins with the name of the switch. This name will be used later in the routing section. The name is case sensitive. The block must contain all five keywords and terminate with a End statement.

  * Path specifies the call sign/SSID of the adjacent switch, optionally followed by one or more digipeaters.

  * DNIC specifies the DNIC (4 digits) of the adjacent switch.

  * Address specifies the main address of the adjacent switch (the address defined at the top of the fpac.conf in the adjacent node).

  * Port specifies the axport on which the adjacent switch is reached on.

  * NoWP = 1 specifies that the adjacent does not support a White Pages Server (default 0 for FPAC nodes since they DO support White Pages).

## 5.10 Routes Section ##

This section may be moved to a separate file /etc/ax25/fpac.routes. If a separate file is used omit this section from fpac.conf.

The file or section **must** begin with the word ROUTES and end with the Word End.

Routing is made using X.121 addresses. This section defines the routing table rules.

#1. Addresses are evaluated on a hierarchical basis. DNIC first and then the address from left to right.
> a)DNIC, 4 digits for the country code
> b)Address, 6 digits for the region/city/district/address. Address assignment is different for each country.

#2. You may specify a Primary and up to two Alternates Routes for an address. The address file is evaluated from the bottom up.

#3. The switch name must be the same as a Node from the adjacent nodes section/file. It is case sensitive.

#4. All Adjacent Nodes must have their **full** address defined **only once** in the Route table.

#5. A full route address, including the DNIC, maybe 4 to 6 digits long.

```
 Routes
  DNIC = 0
  7100 = LAB	# 1st Alternate
  7100 = DJT	# Primary route
  7120 = LAB	# 1st Alternate
  7120 = DJT	# Primary route
  5050 = DJT	# 1st Alternate
  5050 = AUS	# Primary route
  2080 = SLC	# 2nd Alternate
  2080 = DJT	# 1st Alternate
  2080 = FR0	# Primary route
  DNIC = 3100
321328 = MLB
386524 = LAB 	# 2nd Alternate
386524 = ORL 	# 1st Alternate
386524 = DEL	# Primary route
407277 = ORL
407322 = LAB
727626 = DJT 	# Secondary
727626 = CLW 	# Primary
863494 = Arcade
End
```

  * The DNIC statement will define the DNIC for the addresses that follow.

  * Addresses shorter than 6 digits will be used as a mask. Addresses beginning with this value will be routed to the specified switch.

  * The most complete address value will be used first.

```
		Example :
        DNIC = 2080
        981 = Rebigue
        981501 = Prat
```

This means that all addresses starting with 981 for the DNIC 2080 will be routed to Rebigue except 981501 which will be routed to Prat. The order is not important, the addresses are sorted in the routing algorithm. The FPAC Routes command will display the route table upside down from the table in fpac.conf/fpac.routes.


# 6.0 Special Files #

## 6.1 fpac.hello ##

This optional text file is located in /etc/ax25/ and sent when a connection is made to fpacnode.

Typically it is a short greeting to the users with a little info about the node.

## 6.2 fpac.info ##

This text file is located in /etc/ax25/ and is sent to the user in response to the Info command.

It is typically a longer form of information about the node and the associated system, and may contain information/examples on how to connect to various services offered.

## 6.3 fpac.dnic ##

This file is located in /usr/local/etc/ax25 and associates a DNIC to a 3 characters country designation. The country designator may be used in some commands instead of the DNIC.

Only the two first words on each line are used. The rest of the line is ignored. Empty lines or lines starting starting with a # character are also ignored.

```
    # Example of fpac.dnic file
    FRA 2080
    ESP 2120
    USA 3100
    AUS 5050
    CAN 3020
```

## 6.4 Help Files ##

Help files are in the directory /var/ax25/fpac/help. The filename **must** be the same as the command, and is case sensitive.

```
    For example:
     The help file for the Connect command will be Connect.hlp.
 
     The help file for the SYSop command will be SYSop.hlp
```

When the user requests help on a command within fpacnode, these are the files that get called to explain the command.

You can create your own help files for your custom commands.


# 7.0 FPAC Utilities #

## 7.1 wpedit ##

Edits the FPAC White Page Database.

Syntax `wpedit <call sign>`

If the call sign is present in the database is may be edited.

## 7.2 wpserv ##

Queries the FPAC White Pages database and dumps it to stdout.

## 7.3 wpmaint ##

Removes outdated records. Old records in the WP Database are Not automatically deleted. When marked for deletion by wpedit, wpmaint will remove them only when they are 7 days or older.

## 7.4 aprslist ##

Lists APRS heard on a port and sends beacon. The aprslist application can be started with options:

```
-l logging = TRUE
-s    show = TRUE
-d  daemon = 0
-v prints aprslist VERSION
-? prints Usage: aprslist [-l] [-s] [-d] [-v]
```

When started, aprslist will start sending APRS beacons and collect APRS information that can be displayed on a local console. Aprslist sends the coordinates on each port that is configured and then the information for the specific port.

All APRS info is also written into /var/ax25/aprs.dat. It is in ASCII readable form.

```
# Configuration file for FPAC module aprslist
# /etc/ax25/aprslist.conf
#
# Geographical coordinates
#         deg min sec
longitude  2  18  80  e
latitude  48  53  08  n
#
# 	*** Beacon information by port ***
# [port] - is device name in /etc/ax25/axports
# period - in minutes for sending coordinates and beacon to APRS
# from – callsign
# to – APRS area
# beacon - Additional Beacon text
[0]
from F6BVP
to APRS
period 1
#
[4]
from F6BVP
beacon JN18DV 145,275 1200b 433,775 9600b http://f6bvp.org
```

## 7.5 mailbbs ##

Sends messages to a BBS. This command may be used to send packet messages to a local or network BBS.

The text of the message may be taken from a text file. This text could be statistics, alerts, or a change in status of the FPAC node. The mailbbs command is incorporated into fpac.sh.

Usage: `mailbbs [-d callsign@bbs] [-f filename] [-i sender] [-t title] [-v] port bbs-callsign [[via] digis...]`

## 7.6 calibrate ##

This command sends full frames to test an ax.25 port.

Syntax: `calibrate -t <seconds> <port>`

The -t option stops transmitting after the specified time in seconds, otherwise press Enter to stop transmitting.

**Note:** If axports parms PacLen is set to less than 256 you will get an error message.

## 7.7 monitor ##

The monitor command starts a full screen session similar to the ax25 tool listen.

For options see `listen -h`

Use 

&lt;CR&gt;

 to end the session.