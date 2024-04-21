# 21 - FTP

<pre class="language-bash"><code class="lang-bash"><strong>//ftp command
</strong><strong>ftp &#x3C;FQDN/IP> 	Interact with the FTP service on the target.
</strong>nc -nv &#x3C;FQDN/IP> 21 	Interact with the FTP service on the target.
telnet &#x3C;FQDN/IP> 21 	Interact with the FTP service on the target.
openssl s_client -connect &#x3C;FQDN/IP>:21 -starttls ftp 	Interact with the FTP service on the target using encrypted connection.
wget -m --no-passive ftp://anonymous:anonymous@&#x3C;target>/&#x3C;port> 	Download all available files on the target FTP server.
get &#x3C;파일명> #파일 가져오는명령어 
put &#x3C;file name> # file upload command
</code></pre>
