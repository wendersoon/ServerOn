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


## Instalação

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

Neste projeto iremos trabalhar apenas com as configurações básicas do arquivo `proftpd.conf`. Talvez, assim que o tempo permitir, irei trabalhar em uma parte 2 dos serviços com as configurações avançadas e bem detalhadas.

## Configuração

Abra o arquivo `proftpd.conf` com o comando `sudo nano /etc/proftpd/proftpd.conf`.

1. `ServerName` - defina o nome do seu servidor, no meu caso vou chamar-lo de `flash`:

![image](https://user-images.githubusercontent.com/104470835/230727019-b25a6f49-13be-42a5-a11e-433965e34f3d.png)

2. `ShowSymlinks` - é a diretiva que controla se os links simbólicos são mostrados para o usuário ou não, portanto é uma configuração de risco para a segurança. No nosso caso iremos desativa-lo, pois **por padrão ele veio ativo** e para isso basta colocar `off` a frente da diretiva:

![image](https://user-images.githubusercontent.com/104470835/230727542-879db06d-a8e9-42b4-b2f9-bdc26a1daac1.png)

3. `TimeoutNoTransfer`, `TimeoutStalled` e `TimeoutIdle` - não iremos alterar os valores padrões dessas diretivas mas é importante mencioná-las aqui por se tratarem do controle do tempo de usuários no servidor. Resumidamente são:
    *  `TimeoutNoTransfer`: controla o tempo máximo em que uma conexão inativa pode permanecer aberta sem transferência de dados antes que ela seja encerrada.
    *  `TimeoutStalled`: controla o tempo máximo em que uma transferência de dados pode ficar inativa antes que seja cancelada pelo servidor.
    *  `TimeoutIdle`: controla o tempo máximo que um usuário pode permanecer conectado sem realizar nenhuma ação antes que a conexão seja encerrada.

![image](https://user-images.githubusercontent.com/104470835/230728081-1b549663-0fa9-4167-afef-4b2ca109e2e3.png)

*Obs: esses tempos são em segundos.*

4. `DefaultRoot` - essa diretiva define o diretório raiz padrão para cada usuário conectado no servidor. Há 3 formas de configurá-lo: 

  * definindo um diretório para todos os usuários - `DefaultRoot /home/ftp`;
  * definindo para todos os usuários de um grupo - `DefaultRoot ~ group1 /home/group1`;
  * definindo para usuários individuais - 
  ```
  DefaultRoot /home/user1 user1
  DefaultRoot /home/user2 user2
  ```
Essa diretiva por padrão vem comentada, vamos descomentá-la e configurar com o usuário e o diretório que ele terá acesso (mais a frente criarei um usuário com esse mesmo nome e o mesmo diretório).

![image](https://user-images.githubusercontent.com/104470835/230729657-ab7bdeaa-f4d7-4abc-a698-fda34c22b14c.png)

5. 
