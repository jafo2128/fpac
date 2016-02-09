# Introduction #

This wiki page will contain some FAQ's and answers on things that may not be covered in the documentaiton.


## FPAC and Linux RMS ##

There are two ways to define an RMS command within fpac.conf (in the Command section):

The first way calls the RMS Gate direct.
> `RMS = /usr/local/bin/rmsgw -P 0 %U`

The second way connects to the the RMS Gate like a RF connect request.
> `RMS = connect k4gbb-10 3100352726`

The second way is preferred way. It allows the FPAC node to supply the
correct line ends and prevent the overwriting of lines supplied by the CMS.

You will likely also want to define an application line in the Applications section:

> `K4GBB-10 = /usr/bin/rmsgw -P 0 %U`

Note that the -P option defines the axport (from the /etc/axports) file to connect with. This MUST be a valid port that has been defined in /etc/rmsgw/channels.xml for this configuration. If you don't use a valid axport/channel, then rmsgw won't start (you'll get a banner and that is it if you were to try connecting from the command line).


There is a newer rmschanstat script available from the LinuxRMS Yahoo! group than is included with the linuxrms source. You will want to get this newer script as it supports FPAC.

In this new script, you **need to check the path to fpad** and correct it based on your installation.


There have been reports of some problems with the above RMS command. If you get an error like this:

```
K4GBB-8 (Commands = ?) : rms
axutils: SSID must follow '-' and be numeric in the range 0-15 - '3100352726'
```

Then one option is to change the command to:

> `RMS = connect k4gbb-10, 3100352726`

Note the "," in there.

It seems that this also may be related to the callsign you have defined for the port in axports. If port 0 in axports has the same callsign and SSID as the call used in the Command and Application in fpac.conf (ie in this example, port 0 also has the call k4gbb-10), you may run in to this problem. Changing the SSID of the call in axports to anything else seems to resolve the issue too.


## White Pages ##

Some explanation of the White Pages system.

Manually entered records are added to the local WP database.

Twice an hour the local node connects to its neighbours WP database and
exchanges database changes.

The node to node WP connection is made at L3.

I don't believe calls heard on the local ports are added to the WP
database.

The Heard (from mheard) list is used locally to complete connect requests.

The local node is automatically placed into the WP database when FPAC is placed on the air the 1st time.

There is an issue with secondary ROSE addresses. The addresses
assigned to added User Ports are not automatically entered into the database. They should be manually added via WPEDIT.

WPEDIT is the utility used to manually edit the WP database.

Entries deleted do not get removed immediately. They are changed to Removed status, and in two weeks they may be erased by the WPMAINT
utility. WPMAINT has to be run in order for those removals to be processed. Callsigns that are not updated for a year also get changed to Removed status and maybe erased 14 days later (by WPMAINT). Usually WPMAINT is run as part of the startup script when the node is started. **This can be a problem** if the node is not restarted periodically.


## SYSop and SHELL Security Explained ##

For security reasons you have to access the SYSop commands to get to
the FPACSHELL (SHELL).

To access the SYSop commands you have to know your FPAC Node's
password/passphrase.

In case you have forgotten the password/passphrase - check fpac.conf.

Connect to the FPAC node and issue the SYSop command.

You will see something like...

```
W4ABC-8 (Commands = ?) : sys
W4ABC-8> 10 19 15 20 6 [1301597890]
```

You may answer the challenge in one of two ways.

If your password/phrase is `Mary_has_a_little_boat`:

1. Enter the full password/passphrase: `Mary_has_a_little_boat`

2. Enter the characters that correspond to the numbers not in the
braces (from the example above those would be 10 19 15 20 6). The valid response in this case would be `abtoh`.

Your password response is validated by:

```
Pass OK

W4ABC-8 (Commands = ?) :
```

You are now in the Sysop Mode and may use all the "normal" commands
plus the defined Sysop commands.

`SHEll` should be one of these commands and it is invoked like the SYSop
command.

This time your valid response will be:

```
Pass OK
#
```

You are now in a **root** shell on the node and you may issue Linux commands. Note that there is no indication of the directory you are in, the root shell environment is homed in /tmp. Also note that you have limited permissions and are limited to basic commands.

Keep is simple and don't over use it... the frequency has eyes and
curious fingers.