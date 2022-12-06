#<h1 align="center">**Bin9**</h1>
Este Readme apresenta um tutorial de instalação e configuração do Bind9, um sistema DNS, que trata da resolução de nomes para endereços IP e vice-versa.

<h3>Instalação</h3>

Instalação do pacote e documentação do Bind9:

`apt-get install -y bind9 bind9utils bind9-doc dnsutils`

<h3>Configuração</h3>

<h4>Edição do arquivo named.conf.local</h4>

Após a instalação do Bind9, edite o arquivo named.conf.local:

`cd /etc/bind`<br>
`nano named.conf.local` <br>

Adicione:<br>
`zone exemplo.com {
  type master; 
  file "/etc/bind.db.exemplo.com"
  };`<br>
  
`zone "1.9.10.in-addr.arpa {
  type master; 
  file "/etc/bind.db.1.9.10"
  };`
  
  Verifique se há erros:<br>
  `named-checkconf`

<h4>Edição do arquivo db.exempo.com</h4>
