![captura2](https://user-images.githubusercontent.com/104470835/229310772-b7ef102c-b681-4706-9dc6-75572a2eafdb.jpg)


Para começarmos precisamos enteder o **DNS** (Domain Name System) que é um sistema de nomenclatura de domínios que atribui nomes **amigáveis e fáceis** de lembrar a endereços IP numéricos, que são usados para identificar servidores e dispositivos na internet. Em vez de digitar uma sequência de números para acessar um site ou serviço, o DNS permite que os usuários simplesmente digitem um nome de domínio, como "google.com", e o sistema se encarrega de traduzir esse nome em um endereço IP correspondente. Imagina se toda que você precisasse acessar um site tivesse de lembrar o IP dele, atualmente isso é impossível e ainda bem que há o serviço DNS. Para saber mais veja esse [artigo](https://www.cloudflare.com/pt-br/learning/dns/what-is-dns/) da Cloudflare que oferece um dos maiores serviços de DNS do mundo.<br>

Já BIND9 é um servidor de DNS de código aberto amplamente utilizado em sistemas Linux. Ele é capaz de executar funções de servidor DNS primário, secundário ou cache e suporta uma variedade de recursos avançados, como DNS dinâmico, DNSSEC e DNS reverso. O BIND9 também pode ser configurado para gerenciar várias zonas de DNS, permitindo que administradores de rede gerenciem e controlem o acesso a diferentes recursos de rede. Se você quiser saber mais, acesse o site oficial pelo [link](https://www.isc.org/bind/).<br>

E nesta etapa do projeto, iremos instalar e fazer algumas configurações básicas para que tenhamos nosso próprio servidor de DNS com o BIND9. <br>

## Instalação

*Todos os comandos serão executados em modo root.*<br>

Para instalar usamos o seguinte comando `sudo apt install bind9 bind9-dnsutils bind9-doc`. O `bind9` é o pacote principal do nosso servidor DNS, o `bind9-dnsutils` é um pacote que fornece utilitérios adicionais que ajudam a gerenciar o DNS, como por exemplo, o `nslookup` que usaremos mais a frente e por fim `bind9-doc` é o pacote que contém a documentação oficial do BIND9.<br>

Podemos ver seu diretório no caminho `/etc/bind/`<br>
![image](https://user-images.githubusercontent.com/104470835/229319975-fbf96f9d-8d92-4447-b9b0-907f0be0533f.png)

Antes de continuarmos é importante termos um resumo dos arquivos que estão nesse diretório que são arquivos de configuração do servidor, são eles:

* `named.conf`: é o arquivo principal de configuração do BIND9. Este arquivo define as opções globais para o servidor DNS, bem como as zonas de DNS que o servidor está autorizado a responder.

* `named.conf.local`: é um arquivo de configuração adicional que inclui zonas de DNS adicionais que não estão incluídas no arquivo named.conf principal.

* `named.conf.options`: é outro arquivo de configuração adicional que define opções adicionais para o servidor DNS, como servidores DNS forwarders, regras de segurança, entre outras.

* `named.conf.default-zones`: é um arquivo de exemplo incluído com o BIND9 que contém as zonas de DNS padrão que são comuns em muitas configurações de servidor DNS.

* `bind.keys`: é um arquivo incluído com o BIND9 que contém uma lista dos chaves DNSSEC confiáveis.

* `db.127`: é um arquivo de zona especial que mapeia o endereço IP 127.0.0.1 para o nome de domínio "localhost". É usado para resolver consultas DNS para o servidor local.

* `db.0`: é um arquivo de zona especial que mapeia o endereço IP 0.0.0.0 para o nome de domínio "localhost". É usado para resolver consultas DNS para o servidor local.

* `db.255`: é um arquivo de exemplo incluído com o BIND9 que contém uma zona de DNS reverso para o endereço IP 255.0.0.0.

* `db.local`: é um arquivo de zona especial que mapeia o nome de domínio "localhost" para o endereço IP 127.0.0.1. É usado para resolver consultas DNS para o servidor local.
  
* `db.empty`: é um arquivo de zona especial que é usado para configurar uma zona vazia no servidor DNS.

* `rndc.key`: é um arquivo que contém uma chave compartilhada entre o daemon do BIND9 e o utilitário rndc (Remote Name Daemon Control). O rndc é um utilitário usado para enviar comandos ao daemon do BIND9 de forma remota, permitindo que os administradores gerenciem o servidor DNS sem precisar se conectar diretamente à máquina em que o servidor está sendo executado.

* `zones.rfc1918`: é um arquivo de configuração que define as zonas de endereços IP reservadas para uso interno, conforme definido na especificação RFC 1918.

## Configuração

1. Configurar as zonas

O bind9 usa arquivos de zonas para associar nomes de domínios a endereços IP. Precisamos criar um arquivo de zona para cada domínio que desejamos configurar e para isso vamos abrir o arquivo de configuração com o seguinte comando `sudo nano /etc/bind/named.conf.local`. Em seguinda adicionar as seguintes linhas nesse arquivo:

```
zone "meusite.com.br" {
    type master;
    file "/etc/bind/db.meusite.com.br";
};
```
Substituindo "meusite.com.br" pelo nome de domínio que você está configurando. Esse arquivo define uma zona mestre (master) para o domínio "meusite.com.br" e aponta para o arquivo de zona /etc/bind/db.meusite.com.br. Depois de tudo configurado salve com `ctrl+o`.<br>

Veja como configurei o meu:<br>
![image](https://user-images.githubusercontent.com/104470835/229353721-efc61765-c31a-4e45-b96e-8b20132af2d3.png)

2. Criar os arquivos de zonas

Agora para cada zona configurada no passo anterior, devemos criar um arquivo de zona com a extensão `.db` que é onde fica os registros DNS. Vamos fazer isso no diretório `/etc/bind/` com o comando `sudo nano /etc/bind/db.meusite753.com.br` (lembre-se que esse é o arquivo que está sendo apontado no passo anterior e por isso ele deve ter o nome do seu dominio). <br>
Dentro do arquivo, você deve incluir os registros DNS para o domínio e seus subdomínios. Os registros DNS incluem informações como o endereço IP associado a um nome de domínio e o servidor de e-mail para o domínio(opcional, assim como o root). Veja abaixo um exemplo simples desse arquivo:<br>

```
$TTL    604800
@       IN      SOA     ns1.meusite.com.br. admin.meusite.com.br. (
                              2         ; serial
                         604800         ; refresh 
                          86400         ; retry 
                        2419200         ; expire 
                         604800         ; minimum TTL 
;
@       IN      NS      ns1.meusite.com.br.
@       IN      NS      ns2.meusite.com.br.

www       IN      A       192.168.1.10
admin     IN      A       192.168.1.20

```
Não entrarei em detalhes do que cada termo significa, você pode estar lendo esse [artigo](https://docs.oracle.com/pt-br/iaas/Content/DNS/Reference/formattingzonefile.htm) muito bem detalhado da Oracle que aborda cada parte dessa documentação.<br>

Veja como configurei o meu:<br>

![image](https://user-images.githubusercontent.com/104470835/229357324-23d895b1-8687-4735-8bfa-18a31a3fa1a1.png)<br>

Após configurar o arquivo de zona, reinicie o bind9 com o comando `sudo /etc/init.d/named restart`.<br>

3. Vericar se há erros

Para verificarmos as configurações utilizaremos os seguintes comandos `sudo named-checkconf` e `sudo named-checkzone meusite753.com.br /etc/bind/db.meusite753.com.br` (lembrando que você deve substituir "meusite753" pelo nome do seu site). Veja o resultado:<br>
![image](https://user-images.githubusercontent.com/104470835/229357839-e15759b6-bbdd-4dec-adc3-21753db64969.png)<br>
Tudo certo por aqui :)

4. Adicionar outro DNS

Se caso nosso servidor não conseguir resolver consultas localmente ele pode buscar em outro DNS. Para configurar basta abrir o seguinte arquivo com o comando `sudo nano /etc/bind/named.conf.options` e descomentar as linhas de `forwarders` e adicionar o servidor que você quer, no meu caso será o DNS 1.1.1.1 da Cloudflare.<br>
![image](https://user-images.githubusercontent.com/104470835/229358953-4864f568-e2b2-4a78-b01a-556b0eb248e1.png)<br>
**Lembre-se de reiniciar o serviço!**<br>

5. Testar a configuração 

Para testarmos se o nosso servidor está resolvendo os nomes corretamente, vamos utilizar dois caminhos. O primeiro é com o utilitário `nslookup` e o segundo é atráves do navegador com a url `www.meusite753.com.br`. Mas antes de prosseguir, devemos configurar nossa máquina real para que responda ao DNS do nosso servidor, no meu caso o IP do meu servidor é **192.168.0.131** e vou configurar no arquivo `/etc/resolv.conf`. Veja esse [tutorial](https://tiflux.com/blog/como-mudar-o-dns-em-varias-plataformas-e-os/) de como alterar o DNS na sua maquina.

* Com `nslookup`:<br>
![image](https://user-images.githubusercontent.com/104470835/229359087-1b13ac93-2a7c-44f7-aefe-88f99bfe77b7.png)

* No navegador (veja a url):<br>

![image](https://user-images.githubusercontent.com/104470835/229359147-35021674-c17b-4fe0-b437-fd97659aac81.png)

## DNS reverso

O DNS reverso é usado para encontrar o nome de domínio associado a um endereço IP. Ao contrário do DNS convencional, que mapeia um nome de domínio para um endereço IP, o DNS reverso mapeia um endereço IP para um nome de domínio. Para configurarmos, seguimos basicamente os mesmos passos anteriores com poucas mudanças.<br>

* No arquivo `named.conf.local` vamos adicionar as informações da zona, seguindo o mesmo padrão. Porém aqui utilizaremos o IP reverso, por exemplo, se o endereço IP da rede for 192.168.1.0/24, o nome da zona reversa deve ser chamado de `1.168.192.in-addr.arpa`. No nosso caso, o IP é 192.168.0.101 e o nome da zona reversa ficará `0.168.192.in-addr.arpa`:<br>
![image](https://user-images.githubusercontent.com/104470835/229360387-b8f04ade-5f21-4d5a-b213-4fb2e2aa22da.png)

* Em seguida vamos criar o arquivo `0.168.192.in-addr.arpa.db` no diretório `/etc/bind/` e configurar o arquivo conforme apresentado no ponto 2 acima:<br>
![image](https://user-images.githubusercontent.com/104470835/229361448-f5d1f51d-7ebc-4b19-996e-dbc908434c3d.png)

**Lembre-se de reiniciar o serviço!**<br>

* Fazemos as checagens das configurações com os comandos `sudo named-checkconf` e `sudo named-checkzone meusite753.com.br /etc/bind/db.0.168.192.in-addr.arpa`.<br>
![image](https://user-images.githubusercontent.com/104470835/229361067-56c7df2e-ce55-484c-858f-ed5c85c16434.png)

* Agora é a hora de testarmos, veja o resultado com o `nslookup`:<br>
  ![image](https://user-images.githubusercontent.com/104470835/229361209-202d7575-8b83-415a-a66a-9b1af577a810.png)<br>
  Isso significa que está tudo configurado corretamente!
