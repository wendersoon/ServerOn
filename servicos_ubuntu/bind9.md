![captura2](https://user-images.githubusercontent.com/104470835/229310772-b7ef102c-b681-4706-9dc6-75572a2eafdb.jpg)


Para começarmos precisamos enteder o **DNS** (Domain Name System) que é um sistema de nomenclatura de domínios que atribui nomes **amigáveis e fáceis** de lembrar a endereços IP numéricos, que são usados para identificar servidores e dispositivos na internet. Em vez de digitar uma sequência de números para acessar um site ou serviço, o DNS permite que os usuários simplesmente digitem um nome de domínio, como "google.com", e o sistema se encarrega de traduzir esse nome em um endereço IP correspondente. Imagina se toda que você precisasse acessar um site tivesse de lembrar o IP dele, atualmente isso é impossível e ainda bem que há o serviço DNS. Para saber mais veja esse [artigo](https://www.cloudflare.com/pt-br/learning/dns/what-is-dns/) da Cloudflare que oferece um dos maiores serviços de DNS do mundo.<br>

Já BIND9 é um servidor de DNS de código aberto amplamente utilizado em sistemas Linux. Ele é capaz de executar funções de servidor DNS primário, secundário ou cache e suporta uma variedade de recursos avançados, como DNS dinâmico, DNSSEC e DNS reverso. O BIND9 também pode ser configurado para gerenciar várias zonas de DNS, permitindo que administradores de rede gerenciem e controlem o acesso a diferentes recursos de rede. Se você quiser saber mais, acesse o site oficial pelo [link](https://www.isc.org/bind/).<br>

E nesta etapa do projeto, iremos instalar e fazer algumas configurações básicas para que tenhamos nosso próprio servidor de DNS com o BIND9. <br>

## Instalação

*Todos os comandos serão executados em modo root.*<br>

Para instalar usamos o seguinte comando `sudo apt install bind9 bind9-utils bind9-doc`. O `bind9` é o pacote principal do nosso servidor DNS, o `bind9-utils` é um pacote que fornece utilitérios adicionais que ajudam a gerenciar o DNS, como por exemplo, o `nslookup` que usaremos mais a frente e por fim `bind9-doc` é o pacote que contém a documentação oficial do BIND9.<br>

Podemos ver seu diretório no caminho `/etc/bind/`<br>
![image](https://user-images.githubusercontent.com/104470835/229319975-fbf96f9d-8d92-4447-b9b0-907f0be0533f.png)

Antes de continuarmos é importante termos um resumo dos arquivos que estão nesse diretório que são arquivos de configuração do servidor, são eles:

* named.conf: é o arquivo principal de configuração do BIND9. Este arquivo define as opções globais para o servidor DNS, bem como as zonas de DNS que o servidor está autorizado a responder.

* named.conf.local: é um arquivo de configuração adicional que inclui zonas de DNS adicionais que não estão incluídas no arquivo named.conf principal.

* named.conf.options: é outro arquivo de configuração adicional que define opções adicionais para o servidor DNS, como servidores DNS forwarders, regras de segurança, entre outras.

* db.<dominio>: são arquivos que contêm a configuração das zonas de DNS, incluindo registros DNS como A, CNAME, MX, entre outros (veremos mais a frente o que significam), que definem como os nomes de domínio são mapeados em endereços IP.

* db.127: é um arquivo de zona especial que mapeia o endereço IP 127.0.0.1 para o nome de domínio "localhost". É usado para resolver consultas DNS para o servidor local.

* db.0: é um arquivo de zona especial que mapeia o endereço IP 0.0.0.0 para o nome de domínio "localhost". É usado para resolver consultas DNS para o servidor local.

* db.local: é um arquivo de zona especial que mapeia o nome de domínio "localhost" para o endereço IP 127.0.0.1. É usado para resolver consultas DNS para o servidor local.
  
* db.empty: é um arquivo de zona especial que é usado para configurar uma zona vazia no servidor DNS.

* named.conf.default-zones: é um arquivo de exemplo incluído com o BIND9 que contém as zonas de DNS padrão que são comuns em muitas configurações de servidor DNS.
