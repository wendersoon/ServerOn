
<div align = "center">

<img src=https://user-images.githubusercontent.com/104470835/233728802-87489d86-361d-486c-8199-8fc3d15ff07a.png width="400"/>

</div>

Uma VPN (**Virtual Private Network**) é uma rede privada virtual que permite que os usuários se conectem à internet de forma segura e privada. Ela cria um canal **criptografado** entre o dispositivo do usuário e a rede VPN, protegendo assim as informações transmitidas de interceptações e violações de privacidade. 

Ao se conectar a uma VPN, o tráfego de internet do usuário é redirecionado por um servidor VPN, que funciona como um intermediário entre o dispositivo do usuário e a internet, tornando o endereço **IP do usuário anônimo**. Isso permite que o usuário acesse a internet de forma segura, sem o risco de ser rastreado ou monitorado por terceiros, além de acessar conteúdos restritos geograficamente, como serviços de streaming, que podem ser bloqueados em sua localização. 

As VPNs são amplamente utilizadas em empresas e organizações que precisam garantir a segurança das informações transmitidas pela internet, bem como por usuários individuais que desejam proteger sua privacidade online e acessar conteúdos restritos.

<div align = "center">

![image](https://user-images.githubusercontent.com/104470835/236550673-de119d00-d184-433a-80cd-9e8dfabff01c.png)

</div>

E nesta etapa do projeto iremos instalar e configurar nossa rede privada virtual (VPN) utilizando o OpenVPN  que é um software de código aberto, que permite a criação de túneis seguros e criptografados entre dois pontos na Internet, permitindo que o tráfego de rede seja protegido de olhares indiscretos. Ele usa protocolos de criptografia para proteger a conexão, tornando-a segura e confiável para a transmissão de dados sensíveis e também está disponível em vários sistemas operacionais e é compatível com vários dispositivos de rede, como roteadores, firewalls e servidores. Se você quiser saber pode está acessando o [github do projeto](https://github.com/Nyr/openvpn-install).

** Todos os comandos serão feito em modo root!**


## Um Pouco Antes da Configuração

Vou capturar os pacotes do meu tráfego de internet do meu pc real é ver estão os protocolos, veja abaixo:

![Screencast from 05-05-2023 16_29_32](https://user-images.githubusercontent.com/104470835/236552702-6b5d3611-acd2-43cc-a699-d3ebaa6ffe4f.gif)

Após a instalação e configuração do serviço VPN, esses pacotes estarão criptografados.

## Instalação

Para instalar é bem simples, basta seguir o que está disponível no github e que reproduzo aqui para a facilitar. O comando é `wget https://git.io/vpn -O openvpn-install.sh && bash openvpn-install.sh`. Depois disso serão solicitados algumas informações que são bem intuitivas, veja abaixo um print das entradas pedidas:

![image](https://user-images.githubusercontent.com/104470835/236560430-f0fe8672-1394-49cf-809a-8af6619a1f89.png)

Depois disso você pode abrir o arquivo gerado no mesmo diretório e colocar as configurações nas informações de VPN da sua máquina. No meu caso, como uso ubuntu, basta que eu pegue esse arquivo e importe ele para as configurações. Se você estiver com dúvidas no Windows veja esse [tutorial](https://marcoandrade.com.br/como-configurar-uma-conexao-vpn-no-windows/).

## Teste do Servidor

Após a instalação, veja abaixo os pacotes capturados com wireshark:

![Screencast from 05-05-2023 19_03_00](https://user-images.githubusercontent.com/104470835/236576391-6b825476-e21c-4448-9081-e81022a0c4ed.gif)

Perceba que agora os pacotes estão encapsulados com o protocolo OPENVPN, isto é, estão criptografados. Isso significa que está funcionando.

---

Terminamos por aqui essa parte do projeto, obrigado!

