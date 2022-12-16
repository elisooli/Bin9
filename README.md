<h1 align="center">Bind9</h1>
<p>Este documento apresenta um tutorial de instalação e configuração do Bind9, um sistema DNS, que trata da resolução de nomes para endereços IP e vice-versa.<p>


<h3>Instalação</h3>
Instalação do pacote e documentação do Bind9:

```apt-get install -y bind9 bind9utils bind9-doc dnsutils```

<h3>Configuração</h3>

<h4>Edição do arquivo named.conf.local</h4>

Após a instalação do Bind9, edite o arquivo named.conf.local:<br>
```
cd /etc/bind
nano named.conf.local
```


No arquivo, adicione uma zona direta e uma reversa. No trecho abaixo, foi usado "exemplo.com" como domínio:

```
//zona direta
zone exemplo.com { 
type master; 
file "/etc/bind.db.exemplo.com"
 };
 
//zona reversa
zone "1.9.10.in-addr.arpa { 
type master; 
file "/etc/bind.db.1.9.10" 
};
 ```
Verifique se há erros:

```named-checkconf```

Após verificar, digite esses comandos:<br>
```
cp db.local db.exemplo.com
cp db.local db.1.9.10
```
 
<h4>Edição do arquivo db.exempo.com</h4>

`nano db.exemplo.com`


Deixe o arquivo assim, no lugar de localhost. substitua por exemplo.com e adicione o IP da máquina:

```
;
;BIND data file for local loopback interface
;
$TTL	604800
@  	IN  	SOA  	exemplo.com.  		root.exemplo.com. (
                              2		         ; Serial
                         604800		         ; Refresh
                          86400		         ; Retry
                        2419200		         ; Expire
                         604800 )		 ; Negative Cache TTL
;
@	IN  NS 	    exemplo.com
@	IN  A       10.9.1.193
@	IN  AAA     ::1
```

Verifique se há erros:

```named-checkzone exemplo.com /etc/bind/db.exemplo.com```

<h4>Edição do arquivo db.1.9.10</h4>

`nano db.1.9.10`

Deixe o arquivo assim, no lugar de localhost. substitua por exemplo.com:

```
;
;BIND data file for local loopback interface
;
$TTL	604800
@  	IN  	SOA  	exemplo.com.  		root.exemplo.com. (
                              2		         ; Serial
                         604800		         ; Refresh
                          86400		         ; Retry
                        2419200		         ; Expire
                         604800 )		 ; Negative Cache TTL
;
@	IN NS	  exemplo.com
@	IN AAA	 ::1
```
Verifique se há erros:

```named-checkzone 1.9.10.in-addr.arpa /etc/bind/db.1.9.10```

Reinicie o serviço:

```
Sudo systemctl restart bind9.service
Sudo systemctl status bind9.service
```
<h4>Edição do arquivo resolv.conf</h4>

Edite o arquivo resolv.conf em /etc:

```nano resolv.conf```

Adicione:

```
Domain exemplo.com
Search exemplo.com
nameserver 10.9.1.193
```
 
<h4>Teste</h4>

```
nslookup exemplo.com
nslookup 10.9.1.193
```

