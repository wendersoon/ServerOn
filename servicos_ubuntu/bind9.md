![captura2](https://user-images.githubusercontent.com/104470835/229310772-b7ef102c-b681-4706-9dc6-75572a2eafdb.jpg)


Para começarmos precisamos enteder o **DNS** (Domain Name System) que é um sistema de nomenclatura de domínios que atribui nomes **amigáveis e fáceis** de lembrar a endereços IP numéricos, que são usados para identificar servidores e dispositivos na internet. Em vez de digitar uma sequência de números para acessar um site ou serviço, o DNS permite que os usuários simplesmente digitem um nome de domínio, como "google.com", e o sistema se encarrega de traduzir esse nome em um endereço IP correspondente. Imagina se toda que você precisasse acessar um site tivesse de lembrar o IP dele, atualmente isso é impossível e ainda bem que há o serviço DNS. Para saber mais veja esse [artigo](https://www.cloudflare.com/pt-br/learning/dns/what-is-dns/) da Cloudflare que oferece um dos maiores serviços de DNS do mundo.<br>

Já BIND9 é um servidor de DNS de código aberto amplamente utilizado em sistemas Linux. Ele é capaz de executar funções de servidor DNS primário, secundário ou cache e suporta uma variedade de recursos avançados, como DNS dinâmico, DNSSEC e DNS reverso. O BIND9 também pode ser configurado para gerenciar várias zonas de DNS, permitindo que administradores de rede gerenciem e controlem o acesso a diferentes recursos de rede. E nesta etapa do projeto, iremos instalar e fazer algumas configurações básicas para que tenhamos nosso próprio servidor de DNS com o BIND9. Se você quiser saber mais, acesse o site oficial pelo [link](https://www.isc.org/bind/).<br>


