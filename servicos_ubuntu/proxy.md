
# SERVIDOR PROXY

![0_c_9_5AFpbROPZkgM](https://user-images.githubusercontent.com/104470835/232338513-9449c3d1-6f37-4f19-9586-2803d76be3a0.gif)

Um servidor **proxy** é um intermediário entre um cliente e um servidor, que permite que o cliente acesse recursos na Internet por meio do servidor proxy. O servidor proxy atua como um "proxy" para o cliente, encaminhando suas solicitações para o servidor de destino e, em seguida, encaminhando as respostas de volta para o cliente. Isso pode ajudar a melhorar a velocidade de acesso a recursos na Internet, reduzir o uso de banda, fornecer segurança adicional e permitir o acesso a recursos restritos por IP. Se quiser saber mais como funciona o proxy, veja esse [artigo da Hostiger](https://www.hostinger.com.br/tutoriais/servidor-proxy).

<div align="center">
  
![squid-logo-png](https://user-images.githubusercontent.com/104470835/232339288-39561db9-2b0b-414b-b5fb-75da234ad849.png)

 </div>

E nesta etapa do projeto, iremos instalar e configurar nosso servidor proxy. Para isso instalaremos o **Squid** que é um servidor proxy de código aberto que pode ser executado em sistemas Unix, Linux, macOs e Windows. É amplamente utilizado por empresas, organizações governamentais e provedores de serviços de internet para melhorar o acesso à internet de seus usuários, tendo por isso grande relevância quando se pensa em montar um servidor proxy. Se você quiser saber mais sobre o Squid acessa a [página oficial](http://www.squid-cache.org/).

***Todos os comandos serão em modo root***

## Instalação

Para instalar você deve utilizar o seguinte comando `sudo apt-get install squid`. Concluída a instalação vejamos o diretório do serviço no caminho /etc/squid/.

![image](https://user-images.githubusercontent.com/104470835/232340079-405e78f3-d592-42eb-b439-83b88602039b.png)

Antes de prosserguimos é importante termos um resumo dos arquivos que estão nesse diretório que são arquivos de configuração do servidor, são eles:

* `squid.conf`: este arquivo contém todas as configurações principais do Squid, como portas de escuta, diretórios de log, políticas de cache, autenticação de usuários, filtros de conteúdo, entre outras.

* `conf.d`: é um diretório usado por algumas distribuições do Linux, como o Ubuntu, para armazenar arquivos de configuração adicionais do Squid. 

* `errorpage.css` é um arquivo css usado para personalizar a página de erro do Squid. A página de erro é exibida quando um usuário tenta acessar um site bloqueado ou quando ocorre um erro no servidor proxy. Nesse arquivo, podemos ter uma página personalizada com o logo da empresa etc.

## Um Pouco Antes da Configuração

***A configuração é logo após essa secção***

Antes de iniciarmos as configurações, é recomendável fazermos uma cópia do arquivo de configuração squid.conf. Isso se deve ao fato de que, como veremos mais adiante, é um arquivo extenso e muito bem documentado. Para fazer uma cópia de backup, utilize o comando `sudo cp /etc/squid/squid.conf /etc/squid/squid.conf.backup`. 

Depois de ter feito isso, abra o arquivo de configuração principal com o comando `sudo nano squid.conf` (lembre-se de estar no diretório correspondente), pare um momento e leia-o. Verá que dos serviços que trabalhamos até agora é o que apresenta uma documentação muito extensa e detalhada. Não abordaremos todos os aspectos desse documento, mas acho interessante darmos uma olhada nos principais para termos um entendimento geral do arquivo.

![image](https://user-images.githubusercontent.com/104470835/232581249-92d56fc0-477b-486f-a55c-28b808d971b8.png)

Conforme a imagem acima, são as recomendações de configurações mínimas que são necessárias para iniciar e usar o Squid como um servidor proxy. Preste atenção na diretiva `acl` que acompanha todas as linhas desmarcadas desse trecho. Essa diretiva (`acl` - Access Control List) é usada para definir uma **lista de controle de acesso** que pode ser usada para **permitir ou negar** o acesso a determinados recursos. As `acl` podem ser definidas de várias maneiras no arquivo `squid.conf`, isto é, definidas por endereço IP, nome de domínio, expressões regulares, por lista de palavras-chave, por hora do dia, por tipo de arquivo etc., realmente a lista é longa. Por exemplo, a diretiva `acl Safe_ports port 21` está criando uma ACL chamada `Safe_ports` que permite ou nega o acesso à porta 21. 

![image](https://user-images.githubusercontent.com/104470835/232587024-b5fc43c1-932a-4902-8d6a-e70c9a4bd752.png)

Ademais, a diretiva `acl` é usada em conjunto com outra chamada `http_access` que é a que define as regras de acesso aos recursos da web com base em uma ou mais ACLs definidas **anteriormente** no arquivo `squid.conf` - veja que destaquei que precisa ser anterior porquê o arquivo de configuração é lido de cima para baixo - essa diretiva é usada para permitir ou negar o acesso aos recursos, também conta com dois parâmetros principais que são `allow` e `deny` (fazem justamente o que nome sugere). Por exemplo, temos na imagem acima o seguinte trecho `http_access deny !Safe_ports`, isso está me dizendo que a diretiva nega(`deny`) o acesso a todas as portas que **não**(`!`) estão listadas em `Safe_ports`. Exemplificando menlhor, se a ACL `Safe_ports` incluir as portas 80, 443, 8080 e 8443, a diretiva `http_access deny !Safe_ports` negará o acesso a todas as outras portas, exceto essas quatro portas seguras.

![image](https://user-images.githubusercontent.com/104470835/232590264-5156e9f0-d553-4179-9102-284f9d0f9778.png)

Nesse print temos duas diretivas que trabalham em conjunto e são muito importantes no uso de um servidor proxy. São elas as `cache_swap_low` e `cache_swap_high`, essas duas diretivas especificam, respectivamente, o limite inferior e superior para o uso do disco rígido em relação ao cache de objetos do Squid. Os valores definidos em cada uma são porcentagens do espaço total do disco dedicado ao cache, o primeiro valor significa que o Squid começará a remover objetos do cache quando o uso do disco atingir 90% e o segundo valor diz que o Squid começará a recusar novas solicitações de cache quando o uso do disco atingir ou ultrapassar 95%.

São muitas as personalizações que o Squid permite-nos, vimos apenas um punhado das principais configurações e não é meu objetivo, neste projeto, abordar cada uma delas e sim para que tenhamos um servidor proxy minimamente funcional. Então por isso, vamos as configurações!

## Configuração

Utilizarei as configurações que foram apresentadas em sala de aula, pois elas possuem o essencial para o que nos propomos aqui. 

Abra o arquivo `squid.conf` e adicione as sequintes diretivas ao final do arquivo ou, se preferir, pode apagar toda a documentação (lembre-se que temos nosso backup se as coisas derem errado) e colar as configurações abaixo, para apagar tudo dentro do arquivo user o comando `> squid.conf`:


```
http_port 3128
error_directory /usr/share/squid/errors/Portuguese
cache_mem 1024 MB
cache_dir ufs /var/spool/squid 10000 16 256
maximum_object_size_in_memory 64 KB
maximum_object_size 512 MB
cache_swap_low 70
cache_swap_high 95
access_log daemon:/var/log/squid/access.log squid
cache_log /var/log/squid/cache.log
acl localnet src 192.168.0.0
acl Safe_ports port 80 # http
acl Safe_ports port 21 # ftp
acl Safe_ports port 443 # https
http_access deny !Safe_ports
acl sitesproibidos url_regex -i "/etc/squid/sitesproibidos"
http_access deny localnet sitesproibidos
http_access allow localnet
http_access allow all
```

Vamos ao significado de cada uma das diretivas pra você saber o que estamos configurando, são eles:

* `http_port 3128`: define a porta em que o Squid escutará as conexões HTTP;
* `error_directory /usr/share/squid/errors/Portuguese`: : define o diretório onde estão as páginas de erro do Squid em português. Veja a imagem dele baixo:

![image](https://user-images.githubusercontent.com/104470835/232602617-303a6a7f-39b6-4a48-badb-1c445ac3db58.png)

* `cache_mem 1024 MB`: define a quantidade de memória RAM que será utilizada para armazenar objetos em cache, **essa configuração depende das especifícações de seu servidor**.
* `cache_dir ufs /var/spool/squid 10000 16 256`: define o diretório onde serão armazenados os objetos em cache.A sintaxe padrão dela é `cache_dir aufs Directory-Name Mbytes L1 L2 [options]` você pode está verificando na [documentação](http://www.squid-cache.org/Doc/config/cache_dir/). Os parâmetros definidos foram: `ufs`(Unix File System) - é o tipo de sistema de arquivos que o Squid usa no armazenamento da cache; `10000` informa o tamanho em MB do diretório de cache (**veja o que seu sistema suporta**); `16 256` - diz que criará 16 diretórios de nível 1 e que dentro de cada diretório teremos 256 outros diretórios;
* `maximum_object_size_in_memory 64 KB`: define o tamanho máximo de um objeto que pode ser armazenado em cache na memória RAM;
* `maximum_object_size 512 MB`: Define o tamanho máximo de um objeto que pode ser armazenado em cache no disco rígido;
* `cache_swap_low 70` e `cache_swap_high 95`: já foram explicados mais acima;
* `access_log daemon:/var/log/squid/access.log squid`: define o arquivo de log onde serão registradas as requisições e respostas HTTP;
* `cache_log /var/log/squid/cache.log`: define o arquivo de log onde serão registradas as ações relacionadas ao cache de objetos;
* Em seguida temos as ACL's e o `http_acess` que já foram explicadas anteriormente. Mas vamos à alguns detalhes, na linha `acl localnet src 192.168.0.0` estamos criando uma acl para nossa rede local que têm origem (`src`) no IP `192.168.0.0`, por isso configure de acordo com sua rede. Ademais, temos a linha `acl sitesproibidos url_regex -i "/etc/squid/sitesproibidos.txt"`, que é exatamento o que você está pensando, criamos uma acl que ler o regex contindo no arquivo `sitesproibidos` e logo em seguida negamos com `http_access deny localnet sitesproibidos` qualquer recurso da web que contenha um termo presente nesse arquivo.

Agora que você sabe os significados das diretivas, sinta-se a vontade pra acessar a [documentação oficial](http://www.squid-cache.org/Doc/config/). Veja como está meu arquivo `squid.conf`:

![image](https://user-images.githubusercontent.com/104470835/232612749-cbf4a01f-d259-4e50-abb9-cd442f743c64.png)

Depois de feito isso e antes de reiniciar o serviço, devemos criar o diretório de cache definido em `cache_dir`. Para isso utilize o comando `sudo mkdir -p /var/spool/squid` e altere as permissões (você já pode está verificando com ls -l dentro do diretório se já está `proxy:proxy`) desse diretório para o proxy com o comando `chown proxy:proxy /var/spool/squid`. Em seguida crie dentro `/etc/squid/` o arquivo *sitesproibidos* com o comando `touch sitesproibidos` e se quiser já adicionar alguma palavra para testar depois fique a vontade, no meu caso adicionei a palavra "quadrado".

Agora use o comando da verdade (kkk) para verificar se as configurações estão corretas, se não aprecer nenhum aviso é porquê está tudo ok! O comando é `squid -k check`. Depois utilize o seguinte comando `echo 1 > /proc/sys/net/ipv4/ip_forward`, ele é uma configuração do Linux que permite que pacotes sejam encaminhados de uma interface de rede para outra. Habilitando assim o encaminhamento de pacotes IPv4 na máquina, permitindo que o Squid atue como um proxy para os clientes, é necessário reiniciar a máquina para que a configuração entre em vigor.

Logo após, utilize o comando `iptables -t nat -A POSTROUTING -o <interface de internet> -s <ip/mascara> -j MASQUERADE` e substitua por sua interface de rede, o IP e a máscara, por exemplo, no meu caso fica assim `iptables -t nat -A POSTROUTING -o enp0s3 -s <192.168.0.0/24> -j MASQUERADE`. Esse comando configura uma regra de NAT no iptables do Linux, isto é, permite que dispositivos em uma rede local (IP 192.168.0.0/24) possam acessar a Internet por meio de uma interface de rede específica (enp0s3), através do redirecionamento do tráfego de saída da rede local para essa interface de rede.

Se tudo deu certo até aqui, então reinicie com `sudo /etc/init.d/squid restart` e aguarde um pouco. Vamos verificar o status do nosso servidor com `sudo /etc/init.d/squid status`.

![image](https://user-images.githubusercontent.com/104470835/232618807-421ebdf8-b7e3-49c5-ba36-de0a1a6af433.png)

## Teste do Servidor 

Enfim, terminado a configuração vamos testar se nosso servidor está funcionando. Para isso temos de adicionar o IP do servidor nas configurações de rede da máquina da onde partirão os acessos e isso vai depender de qual sistema operacional você está usando, então se você não saber com fazer pode está lendo esse artigo que irá ajudar([artigo](https://www.avast.com/pt-br/c-how-to-set-up-a-proxy)) 

![image](https://user-images.githubusercontent.com/104470835/232621073-588bc992-163d-4ddb-acc7-9064175acacc.png)

Depois de configurando, podemos ver os logs de acesso em `sudo nano /var/log/squid/access.log`:

![image](https://user-images.githubusercontent.com/104470835/232621590-89c989a8-7d96-4cf9-8340-66660e54f03a.png)

E também o log da cache com `sudo nano /var/log/squid/cache.log`

![image](https://user-images.githubusercontent.com/104470835/232621805-f1130aae-8b6e-4384-8294-3e63d5665d17.png)

E por fim, podemos ver os objetos da cache que estão no caminho `/var/spool/squid`. Perceba na imagem abaixo os vários diretórios criados, você pode está explorando eles em busca dos dados.

![image](https://user-images.githubusercontent.com/104470835/232622045-2e3fb4ea-e6e2-47b7-9adf-8ff0845192fa.png)

**Está funcionando!**

Terminamos aqui essa parte do projeto. Até o próximo, obrigado!

<div align = "center">

![meme-32513-muito-obrigado!-](https://user-images.githubusercontent.com/104470835/232622936-5b1e935d-de54-4749-a620-c0638c7afbcd.jpg)

<div>








