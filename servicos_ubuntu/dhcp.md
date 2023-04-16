<div align="center">

![istockphoto-1287005334-170667a-removebg-preview](https://user-images.githubusercontent.com/104470835/232313043-ad7be7d3-28e5-43da-8403-ac9f8d311ac1.png)

</div>

O DHCP (**Dynamic Host Configuration Protocol**) é um protocolo de rede que permite que um servidor atribua automaticamente endereços IP e outras configurações de rede para dispositivos em uma rede. Em vez de ter que configurar manualmente cada dispositivo com um endereço IP, gateway padrão, servidor DNS, entre outras configurações, o DHCP simplifica esse processo, tornando-o mais automatizado e eficiente.

Quando um dispositivo é conectado a uma rede que usa o DHCP, ele solicita uma configuração de rede ao servidor DHCP. O servidor DHCP responde com um endereço IP disponível, que é então atribuído ao dispositivo que solicitou. Além do endereço IP, o servidor DHCP também pode fornecer outras configurações de rede, como o gateway padrão, a máscara de sub-rede, os servidores DNS, entre outros.

Imagina se não houvesse o DHCP, essas configurações seriam todas manuais. Em um computador é tranquilo configurar, mas em redes que existem muitos dispositivos torna-se um trabalho cansativo e demorado. 

<div align="center">

![be9bc8030987bc5d0337f2df3ed30b53bc743173cf4d0606ea3282be32a07b5b_1](https://user-images.githubusercontent.com/104470835/232315731-7fb60fd2-7541-4001-a604-c001f7b715a7.jpg)

</div>

E nesta etapa do projeto, iremos instalar e configurar nosso próprio servidor DHCP e para isso instalaremos o **ISC DHCP Server**. Ele é um software open-source que implementa o protocolo DHCP e é muito utilizado em sistemas baseados em Unix, têm a característica de ser altamente configurável e personalizável. Se você quiser saber, pode estar acessando a [página oficial](https://www.isc.org/dhcp/).


***Todos os comandos serão em modo root***

## Instalação

Para instalar você deve utilizar o seguinte comando `sudo apt-get install isc-dhcp-server`. Concluída a instalação vejamos o diretório do serviço no caminho `/etc/dhcp/`.

![image](https://user-images.githubusercontent.com/104470835/232315843-2ac199e2-cdce-4252-9c58-abfe02ac8dbb.png)

Antes de prosserguimos é importante termos um resumo dos arquivos que estão nesse diretório que são arquivos de configuração do servidor, são eles:

* `ddns-keys`: este diretório é usado para armazenar chaves de atualização dinâmica DNS (DDNS) usadas pelo servidor DHCP.

* `dhclient-enter-hooks.d`: este diretório é usado para armazenar scripts que são executados quando o cliente DHCP entra em um novo estado (quando é iniciado ou recebe um novo endereço IP).

* `dhclient-exit-hooks.d`: este diretório é usado para armazenar scripts que são executados quando o cliente DHCP sai de um estado (quando é desligado ou perde a conexão com a rede).

* `dhclient.conf`: este arquivo contém as configurações do cliente DHCP, que é usado para solicitar um endereço IP de um servidor DHCP na rede.

* `dhcpd6.conf`: este arquivo contém configurações para o servidor DHCPv6, que é usado para distribuir endereços IPv6 em uma rede.

* `dhcpd.conf`: este arquivo é o arquivo de configuração principal do servidor DHCP ISC, que é usado para configurar o servidor DHCP, incluindo opções de rede, faixas de endereços IP e configurações de clientes DHCP.

* `debug`: este arquivo contém as mensagens de depuração geradas pelo servidor DHCP.

Nesta etapa do projeto irei trabalhar apenas com as configurações básicas do arquivo `dhcpd.conf`. Talvez, se o tempo permitir, irei fazer uma parte 2 com as configurações avançadas e num maior detalhe.

## Configuração 

1. Primeiro vamos editar o arquivo que está no caminho `sudo nano /etc/default/isc-dhcp-server`. Este arquivo é usado para definir as opções de configuração padrão do nosso servidor dhcp. Entenda ele como o arquivo que contém as variáveis de ambiente que são usadas pelo script de inicialização do serviço dhcp. Vamos fazer apenas duas alterações nele, isto é, primeiro descomentar a linha `DHCPDv4_CONF=/etc/dhcp/dhcpd.conf` e segundo adicionar a interface de rede em `INTERFACESv4=""` (lembre-se de aqui verificar sua interface com o comando `ifconfig`). Veja como ficou o meu:

![image](https://user-images.githubusercontent.com/104470835/232319073-1ff0a794-6c57-4d7d-8306-b6908b0d132a.png)

Feito essas alterações salve o arquivo (imagino que ja saiba o comando, mas em todo caso é `ctrl + o`) e reinicie o serviço com o comando `sudo /etc/init.d/isc-dhcp-server restart`.

2. Agora vamos abrir o arquivo de configuração principal do serviço dhcp com o comando `sudo nano /etc/dhcp/dhcpd.conf` e editar as configurações globais do servidor, iremos alterar as seguintes:

* `default-lease-time`: é o tempo padrão, em segundos, que um endereço IP permanecerá associado a um determinado cliente DHCP, a menos que o cliente renove sua concessão antes que o tempo expire;
* `max-lease-time`: especifica o tempo máximo, em segundos, de concessão para os endereços IP atribuídos pelo servidor DHCP. Isto é, o tempo máximo que o cliente DHCP poderá ficar com aquele IP antes que precise renovar a concessão;
* `option domain-name`: especifica o nome do domínio usados pelo clientes DHCP; 
* `option domain-name-servers`: especifica o endereço IP do servidor DNS que será atribuído aos clientes DHCP.

Sabido o significado dessas diretivas veja como configurei o meu:

![image](https://user-images.githubusercontent.com/104470835/232330647-a782f007-7b19-400d-a78a-023f297fdd6f.png)

Defini que o servidor DNS padrão será da Cloudflare `1.1.1.1` e `1.1.1.3`, e que um cliente poderá ter um IP associado a ele por 30 min e o máximo é 2 horas. E importante perceber que essa configurações dependem dos casos onde são aplicados. Imagina se você configurar a associação do IP por no máximo 45 min em uma rede pública de um parque onde existe um tráfego intenso de pessoas, com certeza teria problemas na conexão.

3. Se você parar um instante para ler o o arquivo `dhcpd.conf`, verá que nele estão comentados muitos exemplos de como configurar o servidor, têm-se desde de configurações personalizadas para uma máquina específica até as configurações para grandes redes. Mas como disse anteriormente, irei focar no mais básico para que tenhamos um servidor funcional nesse primeiro momento. 

Agora quero meu servidor entregue IP's na faixa de `192.168.5.100` à `192.168.5.200`, que use os tempos padrões do servidor porém o servidor DNS primário será o `8.8.8.8` da Google. Então vou adicionar as seguintes diretivas no final do arquivo `dhcpd.conf`:

![image](https://user-images.githubusercontent.com/104470835/232332340-c2efe05b-dda2-499a-887b-a939cc51cde6.png)

A diretiva `option routers` é usada para especificar o endereço IP do gateway padrão e a `option broadcast-address` é usada para especificar o endereço de broadcast para a rede do cliente. Depois de feito isso, salve o arquivo.

4. Depois de feito essas alterações, utilize o comando `sudo dhcpd -t -cf /etc/dhcp/dhcpd.conf` para verificar quaisquer erros de sintaxes que possam haver no arquivo de configuração. Irá aparecer uma imagem semelhante a de baixo, de outro modo pode conter algum erro.

![image](https://user-images.githubusercontent.com/104470835/232332795-69f24922-2ef4-4f3b-94e4-0b13762f80e4.png)

Agora reinicie o serviço com o comando `sudo /etc/init.d/isc-dhcp-server restart`.







