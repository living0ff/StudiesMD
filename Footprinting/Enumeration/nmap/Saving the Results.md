## Diferentes formatos

Enquanto executamos várias varreduras, devemos sempre salvar os resultados. Podemos usá-los mais tarde para examinar as diferenças entre os diferentes métodos de varredura que usamos. `Nmap` pode salvar os resultados em 3 formatos diferentes.

- Saída normal ( `-oN`) com a extensão do arquivo
-  Saída Grepable ( `-oG`) `.nmap`  com a extensão do arquivo `.gnmap` 
- Saída XML ( `-oX`) com a extensão do arquivo `.xml`

Também podemos especificar a opção ( `-oA`) para salvar os resultados em todos os formatos. O comando pode ficar assim:

```shell-session
casluxd@htb[/htb]$ sudo nmap 10.129.2.28 -p- -oA target

Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-16 12:14 CEST
Nmap scan report for 10.129.2.28
Host is up (0.0091s latency).
Not shown: 65525 closed ports
PORT      STATE SERVICE
22/tcp    open  ssh
25/tcp    open  smtp
80/tcp    open  http
MAC Address: DE:AD:00:00:BE:EF (Intel Corporate)

Nmap done: 1 IP address (1 host up) scanned in 10.22 seconds
```

|**Opções de digitalização**|**Descrição**|
|---|---|
|`10.129.2.28`|Examina o destino especificado.|
|`-p-`|Verifica todas as portas.|
|`-oA target`|Salva os resultados em todos os formatos, iniciando o nome de cada arquivo com 'target'.|

Se nenhum caminho completo for fornecido, os resultados serão armazenados no diretório em que estamos atualmente. Em seguida, examinamos os diferentes formatos `Nmap` criados para nós.

```shell-session
casluxd@htb[/htb]$ ls

target.gnmap target.xml  target.nmap
```

```shell-session
casluxd@htb[/htb]$ cat target.nmap

# Nmap 7.80 scan initiated Tue Jun 16 12:14:53 2020 as: nmap -p- -oA target 10.129.2.28
Nmap scan report for 10.129.2.28
Host is up (0.053s latency).
Not shown: 4 closed ports
PORT   STATE SERVICE
22/tcp open  ssh
25/tcp open  smtp
80/tcp open  http
MAC Address: DE:AD:00:00:BE:EF (Intel Corporate)

# Nmap done at Tue Jun 16 12:15:03 2020 -- 1 IP address (1 host up) scanned in 10.22 seconds
```

```shell-session
casluxd@htb[/htb]$ cat target.gnmap

# Nmap 7.80 scan initiated Tue Jun 16 12:14:53 2020 as: nmap -p- -oA target 10.129.2.28
Host: 10.129.2.28 ()	Status: Up
Host: 10.129.2.28 ()	Ports: 22/open/tcp//ssh///, 25/open/tcp//smtp///, 80/open/tcp//http///	Ignored State: closed (4)
# Nmap done at Tue Jun 16 12:14:53 2020 -- 1 IP address (1 host up) scanned in 10.22 seconds
```

```shell-session
casluxd@htb[/htb]$ cat target.xml

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE nmaprun>
<?xml-stylesheet href="file:///usr/local/bin/../share/nmap/nmap.xsl" type="text/xsl"?>
<!-- Nmap 7.80 scan initiated Tue Jun 16 12:14:53 2020 as: nmap -p- -oA target 10.129.2.28 -->
<nmaprun scanner="nmap" args="nmap -p- -oA target 10.129.2.28" start="12145301719" startstr="Tue Jun 16 12:15:03 2020" version="7.80" xmloutputversion="1.04">
<scaninfo type="syn" protocol="tcp" numservices="65535" services="1-65535"/>
<verbose level="0"/>
<debugging level="0"/>
<host starttime="12145301719" endtime="12150323493"><status state="up" reason="arp-response" reason_ttl="0"/>
<address addr="10.129.2.28" addrtype="ipv4"/>
<address addr="DE:AD:00:00:BE:EF" addrtype="mac" vendor="Intel Corporate"/>
<hostnames>
</hostnames>
<ports><extraports state="closed" count="4">
<extrareasons reason="resets" count="4"/>
</extraports>
<port protocol="tcp" portid="22"><state state="open" reason="syn-ack" reason_ttl="64"/><service name="ssh" method="table" conf="3"/></port>
<port protocol="tcp" portid="25"><state state="open" reason="syn-ack" reason_ttl="64"/><service name="smtp" method="table" conf="3"/></port>
<port protocol="tcp" portid="80"><state state="open" reason="syn-ack" reason_ttl="64"/><service name="http" method="table" conf="3"/></port>
</ports>
<times srtt="52614" rttvar="75640" to="355174"/>
</host>
<runstats><finished time="12150323493" timestr="Tue Jun 16 12:14:53 2020" elapsed="10.22" summary="Nmap done at Tue Jun 16 12:15:03 2020; 1 IP address (1 host up) scanned in 10.22 seconds" exit="success"/><hosts up="1" down="0" total="1"/>
</runstats>
</nmaprun>
```

Com a saída XML, podemos criar facilmente relatórios HTML fáceis de ler, mesmo para pessoas não técnicas. Isso é muito útil posteriormente para documentação, pois apresenta nossos resultados de maneira detalhada e clara. Para converter os resultados armazenados do formato XML para HTML, podemos usar a ferramenta `xsltproc`.

```shell-session
casluxd@htb[/htb]$ xsltproc target.xml -o target.html
```

Se agora abrirmos o arquivo HTML em nosso navegador, veremos uma apresentação clara e estruturada de nossos resultados.

![[Pasted image 20230616124459.png]]

Mais informações sobre os formatos de saída podem ser encontradas em: [https://nmap.org/book/output.html](https://nmap.org/book/output.html)