
<div align="center">

![server-on](https://user-images.githubusercontent.com/104470835/226133741-49727b91-65b7-45d2-a37f-c633ee8c80b0.gif)

</div>

# Introdução

Essa repositório documenta todas as etapas de implementação de um servidor proposto pelo professor [Paulo Henrique Sousa Barbosa](https://github.com/agenteph) na disciplina de Redes de Computadores II do curso de Ciência da Computação do Instituto Federal de Educação, Ciência e Tecnologia do Maranhão - Campus Imperatriz (IFMA). Neste projeto apresentarei as etapas que englobam desde a instalação do sistema operacional que vou utilizar no servidor até aos serviços propostos(por exemplo, DHCP, Web etc.) em sala de aula bem como serviços bônus desde que o tempo me permita. Vejamos as espeficações da minha máquina atual.

# Especificações Técnicas

Processador: Intel i3-1005G1 CPU @ 1.20GHz × 4<br>
Memória ram: 20GB<br>
Sistema operacioal: Ubuntu 22.04.1 LTS<br>
Disco: SSD NVMe 256GB<br>
Placa de rede: Gigabit<br>


# Index
Todas as etapas desse projeto estão listados abaixo:

### Ubuntu Server

* [Instalação do Sistema Operacional - Ubuntu Server](https://github.com/dswendersonmelo/Server/blob/main/SistemaOperacional/UbuntuServer.md)<br>
  * [Utilitários e Configurações Iniciais](https://github.com/dswendersonmelo/ServerOn/blob/main/SistemaOperacional/startconfig.md)
* [SERVIÇO WEB: Apache2 + PHP + MariaDB](https://github.com/dswendersonmelo/ServerOn/blob/main/servicos_ubuntu/apache.md)
* [SERVIÇO DNS: Bind9](https://github.com/dswendersonmelo/ServerOn/blob/main/servicos_ubuntu/bind9.md)
* [SERVIÇO WEB COM CERTIFICAÇÃO SSL: OpenSSL](https://github.com/wendersoon/ServerOn/blob/main/servicos_ubuntu/certificacao.md) 
* [SERVIÇO DE COMPARTILHAMENTO DE ARQUIVOS: Proftpd + SAMBA](https://github.com/wendersoon/ServerOn/blob/main/servicos_ubuntu/ServerArquivo.md)
* [SERVIÇO DHCP: ISC DHCP server](https://github.com/wendersoon/ServerOn/blob/main/servicos_ubuntu/dhcp.md)
* [SERVIÇO PROXY: Squid](https://github.com/wendersoon/ServerOn/blob/main/servicos_ubuntu/proxy.md)
* [SERVIÇO VPN: StrongSwan](https://github.com/wendersoon/ServerOn/blob/main/servicos_ubuntu/vpn.md)
* [SERVIÇO DE GERENCIAMENTO DE SERVIDOR]()

