# Mac OS X Forensics tools #

_These parsers are from my MSc Project in Royal Holloway, University of London (2013-2014)._


The goal of these tools is only for developing purpose. The full documented and well implemented version is going to be in **PLASO**:<br>
<br>
<b>Web Project:</b> <a href='http://plaso.kiddaland.net/'>http://plaso.kiddaland.net/</a> <br>
<b>Plaso Source Code: <a href='https://code.google.com/p/plaso/'>https://code.google.com/p/plaso/</a></b><br>

<h2>Installation</h2>
To parse the binaries struct I have used the Python library Construct and for the binary Plist the librari binplist. By default it is not installed (Mac OS X or Linux):<br>
<pre><code>$ sudo easy_install construct<br>
$ sudo easy_install binplist<br>
</code></pre>

<h2>Binary files</h2>
<ul><li><b>asl.py</b>: Apple System Log parsers (/private/var/log/asl).<br>
</li><li><b>bsm.py</b>: Basic Security Module (/private/var/audit/).<br>
</li><li><b>kcpass.py</b>: Decrypt the password store in "/etc/kcpassword" when autologin session is enabled.<br>
</li><li><b>utmpx.py</b>: UTMPX session file (/private/var/run/utmpx).<br>
</li><li><b>cups_ipp.py</b>: CUPS IPP Control files parser.<br>
</li><li><b>plist_artifacts.py</b>: Parsing a group of Plist files that contain timestamp values.<br>
</li><li><b>plist_user.py:</b> Mac OS X 10.8 and 10.9 users Plist file.</li></ul>


<h2>Checking the tools</h2>

Using the tools.<br>
<br>
<br>
<h3>UTMPX</h3>

In a live systems:<br>
<pre><code>$ python utmpx.py<br>
</code></pre>

In a no live systems:<br>
<pre><code>$ python utmpx.py utmpx utmpx_file<br>
</code></pre>

Output example:<br>
<pre><code>   UTMPX File: [utmpx]<br>
<br>
   Header:<br>
	ID: 10<br>
	UptimeTime: Wed, 13 Nov 2013 17:52:34 +0000 (1384365154)<br>
<br>
	Entry: 1<br>
	* User: moxilo<br>
	* Terminal: console<br>
	* Hostname: localhost<br>
	* Status: USER_PROCESS (0x07)<br>
	* Timestamp: Wed, 13 Nov 2013 17:52:41 +0000 (1384365161)<br>
	------------------------------<br>
	Entry: 2<br>
	* User: moxilo<br>
	* Terminal: ttys000<br>
	* Hostname: localhost<br>
	* Status: USER_PROCESS (0x07)<br>
	* Timestamp: Thu, 14 Nov 2013 03:47:22 +0000 (1384400842)<br>
	------------------------------<br>
	Entry: 3<br>
	* User: moxilo<br>
	* Terminal: ttys003<br>
	* Hostname: localhost<br>
	* Status: DEAD_PROCESS (0x08)<br>
	* Timestamp: Thu, 14 Nov 2013 03:37:14 +0000 (1384400234)<br>
	------------------------------<br>
</code></pre>


<h3>Basic security module</h3>

Parsing all the files:<br>
<pre><code>$ for i in `ls logs`; do python bsm.py logs/$i; done<br>
</code></pre>

Output example:<br>
<pre><code>Parsing BSM file [logs/20131102040419.20131103031511].<br>
<br>
        Event: 1<br>
        * ID: 59<br>
        * Event Type: audit startup (45000)<br>
        * Modifier: 0<br>
        * Time: 2013-11-02 04:04:19 (1383365059.132)<br>
        * Text: launchctl::Audit startup<br>
        * Exit SUCCESS(0), Return value 0<br>
        * Trailer: 59<br>
        -------------------------------<br>
        Event: 2<br>
        * ID: 88<br>
        * Event Type: SecSrvr AuthEngine (45025)<br>
        * Modifier: 0<br>
        * Time: 2013-11-02 04:04:23 (1383365063.743)<br>
        * Subject: aid(4294967295), euid(0), egid(0), uid(0), gid(0), pid(11), session_id(100000)<br>
        * Text: begin evaluation<br>
        * Exit SUCCESS(0), Return value 0<br>
        * Trailer: 88<br>
        -------------------------------<br>
        Event: 3<br>
        * ID: 160<br>
        * Event Type: SecSrvr AuthEngine (45025)<br>
        * Modifier: 0<br>
        * Time: 2013-11-02 04:04:23 (1383365063.745)<br>
        * Subject: aid(4294967295), euid(0), egid(0), uid(0), gid(0), pid(11), session_id(100000)<br>
        * Text: com.apple.ServiceManagement.daemons.modify<br>
        * Text: com.apple.ServiceManagement.daemons.modify<br>
...<br>
</code></pre>


Search for parsing error, I really appreciate if you can report to me this errors:<br>
<pre><code>$ for i in `ls logs`; do python bsm.py logs/$i | grep "ERROR\|WARNING"; done | more<br>
</code></pre>


<h3>Binary Apple System Log</h3>

You need to copy the ASL files:<br>
<pre><code>$ mkdir logs<br>
$ sudo cp /private/var/log/asl/* logs/<br>
$ chown your_user logs/*<br>
</code></pre>

Parsing all ASL files:<br>
<pre><code>$ for i in `ls logs`; do python asl.py logs/$i; done<br>
</code></pre>

Output example:<br>
<pre><code>ASL Header:<br>
 Version: 2<br>
 Timestamp: 1385252161<br>
 FirstRecord: 0xd3<br>
 LastRecord: 0x611be<br>
<br>
         Record in: 0xd3<br>
         * Next record in: 0x1c4<br>
         * ASLMessageID: 95820<br>
         * Timestamp: 2013-11-24 00:16:00 (1385252160)<br>
         * Level: NOTICE (5), PID: 72<br>
         * UID: 0, GID: 0, Read_GID: 80<br>
         * Host: DarkTemplar-2.local<br>
         * Sender: hidd<br>
         * Facility: user<br>
         * Message: MultitouchHID: device bootloaded<br>
         * Sender_Mach_UUID: E1D2C14B-1147-3219-9D2D-E4699F159A2F<br>
        ------------------------------------------------------<br>
         Record in: 0x1c4<br>
         * Next record in: 0x2bb<br>
         * ASLMessageID: 95828<br>
         * Timestamp: 2013-11-24 00:16:00 (1385252160)<br>
         * Level: NOTICE (5), PID: 18<br>
         * UID: 0, GID: 0, Read_GID: 80<br>
...<br>
</code></pre>

<h3>CUPS IPP Control Files</h3>

Parsing all the files:<br>
<pre><code>for i in `ls /private/var/spool/cups/c0*`; do python cups_ipp.py $i; done<br>
</code></pre>

Output example:<br>
<pre><code>Creation time: 2013-11-03 18:07:21 (1383502041).<br>
Process time: 2013-11-03 18:07:21 (1383502041).<br>
Completed time: 2013-11-03 18:07:32 (1383502052).<br>
URI: ipp://localhost:631/printers/RHULBWB<br>
User: moxilo<br>
Job name: Assignament 1<br>
Application: LibreOffice<br>
Owner: Joaquin Moreno Garijo<br>
Copies: 1<br>
Printer ID: RHULBWD<br>
Job ID: urn:uuid:d51116d9-143c-3863-62aa-6ef0202de49aB<br>
Computer name: localhost<br>
Document format: application/pdf<br>
------------------------<br>
</code></pre>

<h2>KCPassword</h2>
Output example:<br>
<pre><code># xxd /etc/kcpassword<br>
0000000: 1ceb 3147 d217 2f11 40ff 63bf       ..1G../.@.c.<br>
# python kcpass.py 1ceb3147d2172f1140ff63bf<br>
<br>
         Kcpasswd: 0x1ceb3147d2172f1140ff63bf.<br>
         Magic Xor: 0x7d895223d2bcddeaa3b91f.<br>
         The password is: "abcd".<br>
#<br>
</code></pre>

<h2>User Accounts</h2>

Output example:<br>
<pre><code># python plist_user.py /private/var/db//dslocal/nodes/Default/users/sexyuser.plist <br>
User: sexyuser<br>
UID: 501<br>
GID: 20<br>
Shell: /bin/bash<br>
Policy:<br>
  failedLoginTimestamp at 2001-01-01T00:00:00Z.<br>
  lastLoginTimestamp at 2001-01-01T00:00:00Z.<br>
  passwordLastSetTime at 2013-06-24T20:47:32Z.<br>
Available Passwords:<br>
 Mac OS X user password:<br>
  Iterations: 12345<br>
  Salt: AAAAAA......AAAAAAA<br>
  Entropy: BBBBBBB......BBBBBBBBB<br>
 Kerberos:<br>
  Version: Kerberosv5<br>
  Hash: LKDC:SHA1.ABCDE...ABCDE<br>
</code></pre>

<h2>Plist Artifacts</h2>

<ul><li>Airport<br>
</li><li>Apple Account<br>
</li><li>Bluetooth<br>
</li><li>Mac OS X Update<br>
</li><li>Spotlight<br>
</li><li>Timemachine</li></ul>

Output example:<br>
<pre><code># python plist_artifacts.py /Library/Preferences/com.apple.TimeMachine.plist<br>
File: /Library/Preferences/com.apple.TimeMachine.plist<br>
TimeMachine Device: ['8E30XXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX']<br>
Backup at 2013-09-14 13:24:11+00:00<br>
Backup at 2013-09-25 08:40:55+00:00<br>
Backup at 2013-10-03 14:24:36+00:00<br>
Backup at 2013-10-16 00:32:18+00:00<br>
Backup at 2013-10-24 20:51:30+00:00<br>
Backup at 2013-11-02 00:22:19+00:00<br>
Backup at 2013-11-10 13:27:00+00:00<br>
Backup at 2013-11-22 14:35:14+00:00<br>
Backup at 2013-12-05 17:51:51+00:00<br>
Backup at 2013-12-10 15:37:32+00:00<br>
Backup at 2013-12-22 14:38:11+00:00<br>
Backup at 2014-01-04 13:09:10+00:00<br>
Backup at 2014-01-04 13:38:38+00:00<br>
</code></pre>

<h2>Recent Files</h2>

Output example:<br>
<pre><code>$ python mac_recent.py com.apple.recentitems.plist <br>
Recent applications in com.apple.recentitems: Terminal.app<br>
	Path: /Applications/Utilities/Terminal.app<br>
	Inode Path: /6380731/6380732/6391024<br>
	HD Partition Root Name: Macintosh HD<br>
	HD Root UUID: 43B7DEF7-8F02-3A55-820A-2F4DE404F33A<br>
	HD Root mount in: /<br>
	Sandbox ID: 1a7ea0277c27df5d5ddba1c0a1d927a92614e84b<br>
	Sandbox Path: /applications/utilities/terminal.app<br>
Recent applications in com.apple.recentitems: Console.app<br>
	Path: /Applications/Utilities/Console.app<br>
	Inode Path: /6380731/6380732/6762361<br>
	HD Partition Root Name: Macintosh HD<br>
	HD Root UUID: 43B7DEF7-8F02-3A55-820A-2F4DE404F33A<br>
	HD Root mount in: /<br>
	Sandbox ID: d55e75bc98e594f2affd6f2a91cd51feeea0e297<br>
	Sandbox Path: /applications/utilities/console.app<br>
...<br>
</code></pre>

<h2>Anti forensics</h2>

With the code show here you can be allowed to delete entries in all these binaries files without any problem.<br>
Indeed, I have a script called "manzanita" (small apple) that already do it, but I don't know if I am going to publish the source code. I used it only to try to detect the delete entries.