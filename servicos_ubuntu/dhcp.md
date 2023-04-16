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



