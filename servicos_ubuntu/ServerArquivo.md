# Introdução

Nesta etapa do projeto iremos instalar e configurar nosso servidor de arquivos usando **FTP** e **SMB** que são os protocolos de rede mais comuns no compartilhamento de arquivos.<br>

O FTP (File Transfer Protocol) é um protocolo antigo que é usado para transferir arquivos pela internet. Ele funciona como um cliente-servidor, onde o cliente se conecta a um servidor FTP usando um nome de usuário e senha, e então pode enviar e receber arquivos para o servidor. O FTP é amplamente utilizado para transferência de arquivos grandes, como imagens, áudio e vídeo.

Já o SMB (Server Message Block) é um protocolo de compartilhamento de arquivos usado em LAN. Ele permite que vários usuários acessem e compartilhem arquivos e pastas em um servidor de arquivos. O SMB é usado principalmente em redes Windows, e pode ser usado para compartilhar impressoras e outros recursos em rede.

Ambos os protocolos são importantes para compartilhar arquivos em redes, mas o FTP é mais utilizado para transferências pela internet e o SMB é mais utilizado em redes locais.

Se quiser saber mais sobre o FTP veja esse artigo da [Hostinger](https://www.hostinger.com.br/tutoriais/ftp-o-que-e-como-funciona) e sobre o SMB acesse a [página oficial](https://docs.microsoft.com/en-us/windows-server/storage/file-server/file-server-smb-overview) aqui.

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
Essa diretiva por padrão vem comentada, vamos descomentá-la e configurar com o usuário e o diretório raiz que ele terá acesso (mais a frente criarei um usuário com esse mesmo nome e o mesmo diretório):

![image](https://user-images.githubusercontent.com/104470835/230734940-ea6c56ff-9407-4203-a1c4-649a93574043.png)

5. `Port` - essa diretiva define a porta TCP na qual o servidor FTP escuta as conexões do cliente. O valor padrão é a porta 21, que é o padrão para servidores FTP. Não alteraremos ela, pois queremos o servidor FTP no [modo ativo](https://www.pop-rs.rnp.br/~berthold/etcom/redes2-2000/trabalhos/FTP_EltonMarques.htm).

![image](https://user-images.githubusercontent.com/104470835/230732012-89f6f674-54a8-4ff2-bd9a-236bb6c1dd4b.png)

6. `MaxInstances` - define o número máximo de instâncias do servidor que podem ser executadas simultaneamente. Por padrão, o valor é 30. Limitaremos para apenas 5.

![image](https://user-images.githubusercontent.com/104470835/230732039-5e1404f0-bd7e-4bc2-bdc5-dbe54ef04f1b.png)

7. `AllowOverwrite` - esta diretiva habilita ou desabilita a capacidade dos clientes de sobregravarem arquivos existentes no servidor FTP. Por padrão ela vem ativa e deixarei assim.

![image](https://user-images.githubusercontent.com/104470835/230732201-23b9c93c-ccd6-438a-b6a1-4886385bf67f.png)

8. Adicionar a linha `RootLogin off` - essa diretiva especifica se o usuário root pode fazer login no servidor. No caso, quero desabilitada:

![image](https://user-images.githubusercontent.com/104470835/230733590-2c9ffa23-4372-4827-98ea-ebc983dd1ee8.png)

9. Adicionar a linha `TransferRate`- essa diretiva controla a velocidade de transferência de arquivos entre o cliente e o servidor, ela tem dois tipos de comandos: `RETR` que é para baixar um arquivo ou `STOR` para enviar um arquivo. Vamos adicinar a seguinte linha `TransferRate RETR 1024:1024` e `TransferRate STOR 1024:1024`. Estamos limitando as taxas de download e upload para apenas 1kilobyte por segundo:

![image](https://user-images.githubusercontent.com/104470835/230734163-b4041c18-110a-4802-af54-2b908d3ef32b.png)

Pois bem, depois de concluírmos todas essas etapas, vamos salvar o arquivo com `ctrl+o`. 

### Adicionar Usuário

Antes de testarmos nosso servidor, precisamos criar/adicionar o usuário que definimos no arquivo `proftpd.conf`. Vejamos os passos abaixo:

1. Criar usuário `userftp` com o comando `sudo adduser userftp`:

![image](https://user-images.githubusercontent.com/104470835/230734888-d7609dbe-f9fb-4b55-b1ca-e6d5e487b0d7.png)

2. Vamos criar, por motivos de segurança, um diretório separado para esse usuario com o comando `mkdir /home/userftp/ftp`.

3. Agora vamos editar as informações desse usuário no arquivo `/etc/passwd`, para abri-lo digite o comando `sudo nano /etc/passwd` e vá para a última linha. **Atenção! Muito cuidado em mexer neste arquivo, pois ele contém todas as informações de login no sistema**.

- o usuário é pra estar assim:

![image](https://user-images.githubusercontent.com/104470835/230735610-6971028e-07fb-454b-89e2-72a199fb3044.png)

- Agora vou adicionar o diretório que criei na etapa anterior. Após isso salve o arquivo com `ctrl+o`. Resultado fica assim:

![image](https://user-images.githubusercontent.com/104470835/230737111-2eea89db-9626-4df2-891d-20b243df2419.png)

4. Vamos alterar as permissões do diretório para esse usuário com o comando `chown userftp:userftp ftp`. Mas veja que antes as permissões eram `root:root`:

![image](https://user-images.githubusercontent.com/104470835/230736130-04a57b1b-0469-4b06-a7d1-db64802a7b7d.png)

E também dar permissão total para ler, escrever e executar para o usuário no diretório `/ftp` com o comando `chmod 777 ftp`:

![image](https://user-images.githubusercontent.com/104470835/230736240-eef00e9a-be43-45de-9070-ab9d04dfa612.png)

## Teste do Servidor

Pronto, terminamos todas as configurações para que tenhamos minimamente nosso servidor de arquivos FTP funcionando. Agora vamos reiniciar o serviço com o comando `sudo /etc/init.d/proftpd restart`. Feito isso, é hora de testarmos se está funcionando tudo corretamente. 

Há várias maneiras para realizar isso, no meu caso, o gerenciador de arquivos nativo do minha máquina real permite que eu acesse o servidor usando o endereço no seguinte padrão `ftp://IP-DO-SERVIDOR` (veja como você pode fazer aí na sua máquina). 

![image](https://user-images.githubusercontent.com/104470835/230736491-bc86cb8d-dce6-4a2d-a560-936bbf4b720f.png)

O servidor vai pedir as credenciais de acesso, digite seu usuário e senha. E pronto, estamos dentro do servidor e temos o nosso "pendrive remoto" (kkk):

![image](https://user-images.githubusercontent.com/104470835/230737165-f574be9f-da14-4458-a982-192f0b772b0c.png)

Vamos verificar se o arquivo está realmente na pasta do usuário:

![image](https://user-images.githubusercontent.com/104470835/230737184-41d9feb4-ce21-4bdb-8f06-2aca5582b848.png)

Realizando mais um teste, vou criar um `.txt` no servidor e abrir na minha máquina real:

![image](https://user-images.githubusercontent.com/104470835/230737299-dec45309-079d-4d21-844c-bb88b3b30313.png)

Resultado: 

![image](https://user-images.githubusercontent.com/104470835/230737320-51a3718a-7394-4d6d-8112-9fc55b9af00a.png)

## Verificando os Logs

Todos os logs do nosso servidor FTP são encotrados no diretório `/var/log/proftpd`.

![image](https://user-images.githubusercontent.com/104470835/230737452-93fea887-09a0-4bd7-b00b-5f00fd6588d5.png)

* `xferlog`: este arquivo contém informações detalhadas sobre todas as transferências de arquivos realizadas através do servidor FTP, incluindo o nome do arquivo transferido, a hora e a data da transferência, o endereço IP do cliente e o código de status da transferência. Veja o resultado no meu caso:

![image](https://user-images.githubusercontent.com/104470835/230737659-f4160cac-9d07-4b78-a615-1400252c6d11.png)


* `proftpd.log`: este arquivo contém informações gerais sobre o funcionamento do servidor FTP, como mensagens de depuração, avisos e erros. Veja novamente o meu:

![image](https://user-images.githubusercontent.com/104470835/230737680-7f58171b-d989-433f-a5fd-c9e94e198c6a.png)

É exatamente o que você está pensando, fiz várias tentativas para fazer o login por causa de um pequeno erro que, inclusive, só achei vendo esse arquivo. **Então sempre olhe os arquivos de logs quando surgir um problema**(kkk).

* `controls.log`: é um arquivo de log específico que contém informações sobre a comunicação do servidor com outros sistemas de segurança, como firewalls, IDS/IPS e sistemas de autenticação externos. No meu servidor não foi registrado nada ainda nesse arquivo.

# SAMBA
<div align="center">

![Logo_Samba_Software](https://user-images.githubusercontent.com/104470835/230779987-b39c773f-0809-43be-8b88-7cbf5f11a9e2.png)
</div>

Para nosso servidor de arquivos usando o protocolo SMB, instalaremos o servidor SAMBA que é construído em cima dele e se você quiser saber mais sobre o projeto acesse a página oficial nesse [link](https://www.samba.org/). Mas antes, vejamos um resumo do que é o Samba.

O Samba é um software livre e de código aberto que permite a comunicação multiplataforma entre sistemas Windows, Linux e Unix em uma rede. Ele implementa o protocolo SMB/CIFS (daí a origem do nome "SAMBA"), usado por sistemas operacionais Windows para compartilhar arquivos e recursos em uma rede. O Samba pode ser usado como servidor de arquivos, servidor de impressão ou controlador de domínio, e é amplamente utilizado em empresas e organizações em todo o mundo para permitir a comunicação e compartilhamento de arquivos em uma rede heterogênea de sistemas operacionais.

## Instalação

Para instalar você deve utilizar o seguinte comando `sudo apt-get install samba samba-common`. O `samba` é o pacote principal do SAMBA e o `samba-common` é um pacote que contém arquivos de configuração e outros recursos comuns compartilhados entre os diferentes componentes do Samba, ele é necessário para a instalação do software e deve ser instalado junto com o pacote principal.

Concluída a instalação vejamos o diretório do serviço no caminho `/etc/samba`:

![image](https://user-images.githubusercontent.com/104470835/230787192-c370d70e-903f-4a57-8759-1db0b3d3698a.png)

Antes de continuarmos com a configuração é importante termos um resumo dos arquivos que estão nesse diretório, são eles:

* `smb.conf`:  é o arquivo de configuração principal do Samba e define as opções gerais de configuração, bem como as seções de compartilhamento para os diretórios compartilhados. Ele é usado para definir as configurações de segurança, as permissões de compartilhamento, os nomes de usuário e senha permitidos etc.
* `gdbcommands`: é um arquivo de script usado pelo depurador GDB para executar comandos específicos durante a depuração de programas que utilizam o Samba como biblioteca ou para depurar o próprio Samba.
* `tls`: contém certificados e chaves TLS usados pelo servidor do Samba para criptografar a comunicação de rede entre o cliente e o servidor.  

## Configuração

Vamos nessa seção criar um usuário que possa utilizar o nosso servidor de arquivos Samba, mas antes precisamos entender como funcionar de maneira geral toda a estrutura do arquivo de configuração `smb.conf`. 

O arquivo `smb.conf` é composto de várias partes ou secções como queira chamar. Cada uma dessas partes é identificada por uma chave colocada entre colchetes, vejamos no gif abaixo o arquivo na prática:

![Screencast from 09-04-2023 14_48_14](https://user-images.githubusercontent.com/104470835/230788505-712a2f53-e7a0-4ab0-bfea-0e1c720bc408.gif)

As principais secções desse arquivo são:

* `[global]`: é onde são definidas as configurações globais do servidor Samba, como o nome do grupo de trabalho, o nome do servidor, o nível de log, a autenticação, o suporte a impressoras, a segurança, entre outros.

* `[homes]`: esta secção define as configurações de compartilhamento de diretórios home dos usuários. Cada usuário tem acesso ao seu próprio diretório home.

* `[printers]`: esta secção define as configurações de compartilhamento de impressoras.

* `[nome_personalizado]`: e aqui são definidas as configurações de um compartilhamento específico, onde "nome_personalizado" é o nome do compartilhamento definido pelo administrador.

Agora, dentro de cada secção há várias opções de configurações, vou listar as principais de maneira resumida:

* `path`: o caminho para o diretório que está sendo compartilhado.

* `valid users`: uma lista de usuários que têm permissão para acessar o compartilhamento.

* `read only`: define se o compartilhamento é apenas para leitura ou para leitura e gravação.

* `writeable`: define se o compartilhamento permite gravação.

* `create mask`: as permissões padrão para novos arquivos criados no compartilhamento.

* `directory mask`: as permissões padrão para novos diretórios criados no compartilhamento.

* `guest ok`: define se o compartilhamento é aberto para acesso de convidados, ou seja, sem autenticação.

* `security`:especifica o modo de autenticação usado pelo Samba.

*  `encrypt passwords`: especifica se as senhas dos usuários serão criptografadas durante o processo de autenticação. 

*   `smb passwd file`: é usada para especificar o caminho do arquivo que armazena as senhas dos usuários.

São muitos os parâmetros de configuração para uma secção, não listarei todos por que foge do objetivo principal que queremos aqui nesse projeto. Talvez, eu faça uma parte 2 tratando detalhadamente as configurações do básico ao avançado. Mas com o que já temos, podemos fazer o servidor para o nosso usuário.

1. Adicionar a secção para o usuário `usersamba` no final do arquivo como no exemplo abaixo:

![image](https://user-images.githubusercontent.com/104470835/230792179-0f7dcb45-f711-4312-a49e-176f140bed53.png)

Veja como defini `create mask = 0770` e `directory mask = 0770`. Isso significa que quando um arquivo ou diretório for criado no compartilhamento, ele receberá permissões de leitura, gravação e execução para o **proprietário** e o **grupo proprietário**, mas nenhum acesso para outros usuários. Depois de feito isso, salve o arquivo com `ctrl+o`.

2. Após editar o arquivo de configuração `smb.conf`, podemos fazer uma checagem para ver se está tudo correto e para isso utilizamos o comando `testparm smb.conf`. Se caso houver problemas, o utilitário irá mostrar onde está o erro. Veja o teste no meu caso:

![ezgif-2-4cedfe085c](https://user-images.githubusercontent.com/104470835/231552683-d443eb21-e9b4-452c-9374-1855b45cf193.gif)


3. Criar usuário `usersamba` e diretório `/usersamba/samba` - use os passos dados na secção ***Adicionar Usuário*** mais acima quando configuramos o servidor FTP, lembrando apenas de nomear o usuário como `usersamba` e o diretório correspondente.

4. Depois de ter adicionado o usuário no sistema é preciso adicionar o usuário no samba e para isso utilizamos o seguinte padrão de comando `smbpasswd -a [NOME-DE-USUARIO]` e se caso deseje excluir o usuário é o padrão `smbpasswd -x [NOME-DE-USUARIO]`. É interessante adicionar o mesmo nome de usuário e senha que você adicionou no sistema operacional. No meu caso, o comando utilizado foi `smbpasswd -a usersamba`, veja:

![ezgif-1-cc9a958255](https://user-images.githubusercontent.com/104470835/231550699-f260333c-b359-4b97-ab4a-c9311e740418.gif)

5. Agora vamos reiniciar o Samba com o seguinte comando `sudo /etc/init.d/smbd restart`. Podemos verificar que estar funcionando com o comando `sudo /etc/init.d/smbd status`.

![image](https://user-images.githubusercontent.com/104470835/230790659-58796d7f-80a2-4501-9cb7-edab3b4cb8f4.png)

## Teste do Servidor

Enfim, terminado a configuração podemos testar o funcionamento do servidor e para isso segue o mesmo que acontece quando testamos o servidor FTP mais acima. No meu caso, usarei o meu gerenciador de arquivos que oferece suporte ao protocolo SMB e o padrão de acesso é o mesmo `smb://IP-DO-SERVIDOR`.

![image](https://user-images.githubusercontent.com/104470835/230790980-32d66288-460f-4c6d-bb3e-0bd652b2d417.png)

Veja o que aparece para mim:

![image](https://user-images.githubusercontent.com/104470835/230791001-a157f511-1262-41f5-89a5-fa94c34b9175.png)

Perceba que ali também há uma pasta compartilhada para impressoras e se você voltar no gif que mostro o arquivo `smb.conf` verá que por padrão está pasta já está disponível para ser usada

