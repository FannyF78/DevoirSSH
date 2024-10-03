<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Devoir SSH</title>
  <link rel="stylesheet" href="https://stackedit.io/style.css" />
</head>

<body class="stackedit">
  <div class="stackedit__html"><h1 id="introduction">Introduction</h1>
<p>This file will guide you through your first steps in <strong>SSH</strong> (secure shell), you will learn how to create a keygen and secure the transfer your files. You will also learn how to use <em>fail2ban</em> in order to secure your server and avoid intrusions. At last we will configure Wireshark, your logbook.</p>
<h1 id="ssh--secure-shell">SSH : Secure Shell</h1>
<ul>
<li>What is SSH?</li>
</ul>
<p>SSH is a secure protocol to transfer files from your local device to a distant one. Every time you want to use SSH your command must start from your bash <strong>#ssh</strong></p>
<ul>
<li>How to connect to your distant server?<br>
In order to connect to your distant server you will need the IP adress of your server and connect yourself with the <em>root</em>, your adress should look like this :</li>
</ul>
<blockquote>
<p>root@your distant server IP adress</p>
</blockquote>
<p>You can now create your own user on the distant server :</p>
<pre><code>sudo adduser yourname
</code></pre>
<p>Add a password. You can skip the other values.<br>
Now that you are added in the server as a new member you can assign yourself the “super user” rights. This is the command to let you do that.</p>
<pre><code>sudo visudo
</code></pre>
<p>Underneath the <em>root  <strong>ALL = (ALL:ALL) ALL</strong></em> add your name and do get the same rights as the root. Exit your distant server and connect with your own user name.</p>
<p>Quick check that your connexion is etablished and that you can use your terminal with the following commands :</p>
<p><strong><code>pwd</code></strong><br>
<strong><code>ls</code></strong><br>
<strong><code>whoami</code></strong></p>
<h2 id="authentification">Authentification</h2>
<p>We will now configurate a key to secure the connexion to the SSH.<br>
Firstly we will have to generate a key to your local device.</p>
<p>Your command will be :</p>
<pre><code>ssh-keygen -t ed25519` 
</code></pre>
<p>On your screen you will get a public and a private key. Copy your public key to your distant server from your local machine, with this command :</p>
<pre><code>ssh-copy-id yourusername@ip.adress.of.your.server
</code></pre>
<p>You can now desactivate your password  in order to secure your connexion</p>
<pre><code> sudo nano /etc/ssh/sshd_config
</code></pre>
<p>then,<br>
&gt; <code>PasswordAuthentication no</code></p>
<p>Restart your distant machine</p>
<pre><code>sudo systemctl restart ssh
</code></pre>
<p>and check that your changes are in the server;</p>
<pre><code>ssh yourusername@ip.adress.of.your.server
</code></pre>
<h2 id="use-sftp-and-scp">Use SFTP and SCP</h2>
<ul>
<li>What is the SFTP and the SCP</li>
</ul>
<p>The SFTP is a protocol that allows you to transfer filles between two machines through an SSH connexion.<br>
The SCP is a protocol that copies files from a devise to another through SSH also.</p>
<p>These two protocols will allow you to transfer or to copy files safely by encrypting your data.</p>
<h2 id="the-scp--secure-copy-protocol">The SCP : Secure Copy Protocol</h2>
<p>In order to transfer our data with the SCP Protocol your will use this command on your local server.</p>
<pre><code>echo "your file"&gt;file.txt
</code></pre>
<ul>
<li>
<p>How to tranfer your file :</p>
<p><code>scp fichier.txt username@ipadress:/home/username/</code></p>
</li>
<li>
<p>Check that your file has been transfered</p>
<p><code>scp fichier.txt username@ipadress"ls -l/home/username/"</code></p>
</li>
</ul>
<p>Your file has been succesfully transfered to your distant server. No data has leaked because your file is encrypted.</p>
<h2 id="the-sftp--secure-file-transfer-protocol">The SFTP : Secure File Transfer Protocol</h2>
<p>In order to transfer our data with the SFTP Protocol your will use this command on your distant server.</p>
<pre><code>sftp username@ip.adress
</code></pre>
<p>You can now check the repositories on the server</p>
<pre><code>ls
cd/home/username
</code></pre>
<p>In order to copy your file to the distant server use the command</p>
<pre><code>put file.txt
</code></pre>
<p>Well done, your file has been safely transfered. No data has leaked because your file is encrypted.</p>
<h2 id="ssh-tunnel">SSH Tunnel</h2>
<p>An SSH Tunnel will allow you to verify the network traffic of your through an encrypted connexion. You can use an SSH tunnel to acces to a data base that  you don’t have acces because of a firewall , if you use a non secure connexion, to get a secure acces from distance or to bypass the restrictions of a network.</p>
<ol>
<li>How to use *SSH</li>
</ol>
<p>In order to use the tunnel you have to redirect your traffic to the the port 8080 from you local divice to the distant one, to the port 80.</p>
<pre><code>ssh -L 8080:localhost:80 username@ip.adress
</code></pre>
<p>You can now acces to the web page from your local device.</p>
<blockquote>
<p><a href="http://localhost:8080">http://localhost:8080</a>.</p>
</blockquote>
<ol start="2">
<li>How to secure your server SSH</li>
</ol>
<p>Your server needs to be secure now, to avoid attacks by brute force.</p>
<p>In order to secure your server you will have to change the port set by SSH.</p>
<p>Firstly you will have to modify the port set by SSH</p>
<pre><code>sudo nano /etc/ssh/sshd_config
</code></pre>
<p>Change your port : Port 2222 and then restart your machine</p>
<pre><code>sudo systemctl restart ssh
</code></pre>
<p>And connect yourself back to the server.</p>
<p>Your server is less vulnerable now.</p>
<h1 id="fail2ban">Fail2ban</h1>
<ul>
<li>What is Fail2ban</li>
</ul>
<p>Fail2ban is a security tool who will help you to protect your server from brutal attacks by blocking non authorised access.</p>
<ul>
<li>How to use Fail2ban</li>
</ul>
<p>Firtsly you wil have to install Fail2ban</p>
<pre><code>sudo apt install fail2ban 
</code></pre>
<p>Then configurate your Fail2ban for SSH</p>
<pre><code>sudo nano /etc/fail2ban/jail.local
</code></pre>
<p>Put your password.</p>
<p>Add a section <code>[sshd]</code> ,then <code>enabled = true</code>and lastly you will have to secure  the port that we have used in the SSH Tunnel before, <code>port = 2222</code>.</p>
<p>Restart your system</p>
<pre><code>sudo systemctl restart fail2ban
</code></pre>
<p>You can now test from another machine that the server bans you when you try to connect with an incorrect password.</p>
<p>To un-ban yourself :</p>
<p>Check who is in the “jail”</p>
<pre><code>sudo fail2ban-client status &lt;name.of.the.jail&gt;
</code></pre>
<p>With this command you will get the list of the IP adress that have tried to log to your server.</p>
<p>Use the IP adress to unban yourself</p>
<pre><code>sudo fail2ban-client set &lt;nname.of.the.jail
&gt; unbanip &lt;IP-adress&gt;
</code></pre>
<p>Your IP adress has been un-banned.</p>
<h1 id="wireshark">Wireshark</h1>
<ul>
<li>What is wireshark</li>
</ul>
<p>It’s a tool that will allow you to analyse network data. You will be able to analyse the traffic to your server.</p>
<ul>
<li>How to install Wireshark</li>
</ul>
<p>Let’s go back to your local machine (Linux).<br>
Execute these two commands.</p>
<pre><code>sudo apt update 
sudo apt install wireshark
</code></pre>
<p>You can launch your Wireshark</p>
<pre><code>sudo wireshark
</code></pre>
<p>You may encounter a problem with this command and get a message telling you that the command cannot install or open wireshark. In that case you can simply install wireshark from the website directly</p>
<blockquote>
<p><a href="https://www.wireshark.org/download.html">https://www.wireshark.org/download.html</a></p>
</blockquote>
<p>Once Wireshark installed and opened the tool offers you a view of all the connexions that are done on your machine (wifi, ethernet,…)</p>
<p>On the top of the tool you will see a search bar that allows you to filter the isolate the port 2222.</p>
<p>On the research bar search for the <em>tcp.port == 2222</em></p>
<p>Click on the shark fin and wireshark will now start to analyse the traffic to the port 2222.</p>
<p>You can check it by logging yourself to the distant server and execute command such as</p>
<pre><code>ls,
pwd 
whoami
</code></pre>
<p>Every move you will make to the server will appear to the wireshark with your IP adress.</p>
<p>`</p>
</div>
</body>

</html>
