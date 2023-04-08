# Introdução

Nesta etapa do projeto iremos instalar e configurar nosso servidor de arquivos usando **FTP** e **Samba** que são os mais comuns no compartilhamento de arquivos.<br>

O FTP (File Transfer Protocol) é um protocolo antigo que é usado para transferir arquivos pela internet. Ele funciona como um cliente-servidor, onde o cliente se conecta a um servidor FTP usando um nome de usuário e senha, e então pode enviar e receber arquivos para o servidor. O FTP é amplamente utilizado para transferência de arquivos grandes, como imagens, áudio e vídeo.

O Samba, por outro lado, é um protocolo de compartilhamento de arquivos usado em LAN que permite que computadores com diferentes sistemas operacionais se comuniquem e compartilhem arquivos e impressoras em uma rede. O Samba usa o protocolo SMB (Server Message Block) para compartilhamento de arquivos.

Ambos os protocolos são importantes para compartilhar arquivos em redes, mas o FTP é mais utilizado para transferências pela internet e o Samba é mais utilizado em redes locais.

Se quiser saber mais sobre o FTP veja esse artigo da [Hostinger](https://www.hostinger.com.br/tutoriais/ftp-o-que-e-como-funciona) e sobre o Samba acesse a [página oficial](https://www.samba.org/samba/) do projeto.

***Todos os comandos serão executados em modo root!***

# File Transfer Protocol - FTP
<div align="center">
  
![ftp-how-to-700x353 (convert io)](https://user-images.githubusercontent.com/104470835/230723173-efbc5c21-3114-4e8d-a79b-96f36096bff4.png)

</div>

Usaremos no nosso servidor o **ProFTPd** que é um servidor FTP livre e de código aberto. Ele suporta muitos recursos como limites de taxa de transferência, limites de conexão, configuração de diretórios raiz, suporte a IPv6 e muito mais. Além de recursos avançados, como por exemplo, a autenticação de usuários com vários métodos, incluindo senhas em texto claro, criptografia SSL/TLS e autenticação LDAP. Veja mais no [site oficial.](http://www.proftpd.org/)


## Instalação e Configuração

Para instalar você deve utilizar o seguinte comando `sudo apt-get install proftpd`. Concluída a instalação vejamos o diretório do serviço no caminho `/etc/proftpd/`.

![image](https://user-images.githubusercontent.com/104470835/230725096-44977df7-5234-42af-af62-6cadb892dc11.png)

Antes de prosserguimos é importante termos um resumo dos arquivos que estão nesse diretório que são arquivos de configuração do servidor, são eles:

* `proftpd.conf`: este é o arquivo de configuração principal do servidor. Ele contém as configurações globais para o servidor FTP, bem como as configurações para cada um dos módulos que o servidor pode usar.

* `modules.conf`: é o arquivo usado para habilitar e desabilitar módulos no servidor. Ele contém uma lista de módulos disponíveis e seus status.

* `conf.d/`: este diretório contém arquivos de configuração adicionais para o servidor. Esses arquivos podem ser usados para configurar aspectos específicos do servidor FTP, como autenticação de usuários e configurações de diretórios raiz.

* `dhparams.pem`: é um arquivo de configuração usado para o protocolo SSL/TLS (Secure Sockets Layer/Transport Layer Security) para criptografia de dados em tráfego na web do servidor ftp.

* `ldap.conf`: é o arquivo usado para configurar a autenticação LDAP (Lightweight Directory Access Protocol). 

* `snmp.conf`: é o arquivo usado para configurar o protocolo SNMP (Simple Network Management Protocol).

* `tls.conf`: é o arquivo usado para configurar o TLS (Transport Layer Security). Ele contém informações sobre a criptografia, autenticação, certificados e outras configurações de segurança.

* `blacklist.dat`: é o arquivo usado para definir endereços IP ou nomes de host que são proibidos de acessar o sistema ou aplicativo.

* `geoip.conf`: é o arquivo usado para configurar a detecção de localização geográfica dos usuários com base em seus endereços IP. 

* `sftp.conf`: é o arquivo usado para configurar o serviço SFTP (Secure File Transfer Protocol).

* `sql.conf`: é o arquivo usado para configurar a autenticação com bancos de dados SQL.

* `virtuals.conf`: é o arquivo usado para configurar a criação de hosts virtuais em servidores web.

