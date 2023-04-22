<div align = "center">

<img src=https://user-images.githubusercontent.com/104470835/233728802-87489d86-361d-486c-8199-8fc3d15ff07a.png width="400"/>

</div>

Uma VPN (**Virtual Private Network**) é uma rede privada virtual que permite que os usuários se conectem à internet de forma segura e privada. Ela cria um canal **criptografado** entre o dispositivo do usuário e a rede VPN, protegendo assim as informações transmitidas de interceptações e violações de privacidade. 

Ao se conectar a uma VPN, o tráfego de internet do usuário é redirecionado por um servidor VPN, que funciona como um intermediário entre o dispositivo do usuário e a internet, tornando o endereço **IP do usuário anônimo**. Isso permite que o usuário acesse a internet de forma segura, sem o risco de ser rastreado ou monitorado por terceiros, além de acessar conteúdos restritos geograficamente, como serviços de streaming, que podem ser bloqueados em sua localização. 

As VPNs são amplamente utilizadas em empresas e organizações que precisam garantir a segurança das informações transmitidas pela internet, bem como por usuários individuais que desejam proteger sua privacidade online e acessar conteúdos restritos.


<div align = "center">
  
![download](https://user-images.githubusercontent.com/104470835/233728511-c468bfb2-3772-4d19-a1b9-b82585b17224.png)

</div>


E nesta etapa do projeto, iremos instalar e configurar nosso próprio serviço VPN e para isso instalaremos o ***strongswan***. Ele é um software de código aberto que implementa o protocolo **IPsec** para fornecer segurança em comunicações de rede, permite criar uma rede privada virtual (VPN) segura e criptografada entre dois ou mais dispositivos, é muito implementado em sistemas Linux além de suportar outros sistemas operacionais como o Windows e Android. Se você se interessou e quer saber mais sobre o ***strongswan*** pode está acessando a (página oficial)(https://www.strongswan.org/). 

Antes de prosseguirmos a instalação e configuração, é importante entendermos o que é o IPsec (**Internet Protocol Security**), que nada mais é que um conjunto de protocolos e técnicas utilizadas para proteger a comunicação através da internet. Ele fornece segurança no **nível do protocolo de internet (IP)** para garantir que os dados sejam transmitidos de forma segura e protegida contra interceptação, alteração ou falsificação por terceiros. Nesse [artigo da UFRJ](https://www.gta.ufrj.br/ensino/eel878/redes1-2016-1/16_1_2/vpn/vpn_ipsec2/vpn_ipsec/ipsec.html) você pode está se aprofundado no tema e sugiro que faça isso, pois é assunto muito interessante.

Lembrando, novamente, que ***Todos os comandos serão em modo root!***

## Um Pouco Antes da Configuração

*OBS: Essa secção é mais útil se você tiver um site com domínio no seu servidor onde será instalado o strongswan, pois analisaremos os pacotes da comunicação em rede ante e pós implementação do serviço VPN*

Vou capturar alguns pacotes da rede na comunicação da minha máquina real com o meu servidor DNS, pois quero ver se o pacote está criptografado ou não. Lembre-se que o servidor DNS aqui é o que implementamos alguns serviços atrás, no caso, eu irei acessar o domínio que crie chamado de `www.meusite753.com.br`.

Para você capturar esses pacotes, recomendo instalar o **wireshark** que é um software de análise de rede gratuito e de código aberto que permite capturar, visualizar e analisar o tráfego de rede em **tempo real**. Se caso você nunca ouviu falar, veja esse [tutorial](https://www.varonis.com/pt-br/blog/how-to-use-wireshark) que irá ajudá-lo. Daqui em diante considero que saiba o que é e que já usou ao menos uma vez.

Bom, inicie o wireshark e coloque para escutar a interface de rede que você está usando, talvez aqui seja necessário abrir o programe em modo root(admin para Windows). 

Na parte superior da interface do wireshark, digite a seguinte linha no padrão `ìp.addr == <IP DO SERVIDOR DNS>`. No meu caso ficou assim:

![image](https://user-images.githubusercontent.com/104470835/233750995-48fe8a9f-9ac5-47ac-8d70-21e11ae33a43.png)

Agora acesse seu domínio e atualize a página diversas vezes, para que tenhamos alguns pacotes capturados. No meu caso, já tenho alguns:

![image](https://user-images.githubusercontent.com/104470835/233751463-58b0cb44-2a93-4cac-8fd2-025e9a4d7442.png)

Vou abrir um desses pacotes e veja como consigo ver o conteúdo:

![Screencast from 21-04-2023 21_18_53](https://user-images.githubusercontent.com/104470835/233751876-056f85fd-124f-4f7a-94e3-fbc0efb6ea1b.gif)

O que esperamos ao implementar o servidor VPN é que esse conteúdo esteja criptogrado e, consequetemente, não seja vísivel para quem quer que seja, a não ser o servidor e o cliente. Então, vamos a instalação!!

## Instalação 

Vamos instalar o *strongswan* no **servidor** e na **máquina cliente** (no meu caso será minha máquina real). Todo o processo de instalação e configuração é semelhante nas duas máquinas, porém existe alguns detalhes que os difereciam e quando isso ocorrer vou mostrar separadamente.

Para instalar use o comando `apt-get install strongswan`. Diferente dos outros serviços que instalamos, o **strongswan** não têm somente um diretório mas também arquivos de configuração que se localizam dentro do diretório `/etc/`. Vamos ver cada um deles, começando pelo diretório criado com a instalação `/etc/strongswan.d`:

![image](https://user-images.githubusercontent.com/104470835/233783661-2b0d5705-09f4-4547-bd08-5f6e19ed9231.png)

*Se você quiser ver as informações sobre esses arquivos na documentação, acesse esse [link](https://docs.strongswan.org/docs/6.0/config/config.html)*

* `/charon`: este diretório contém trechos de configuração comentados para todos os plugins ativados e instalados.

![image](https://user-images.githubusercontent.com/104470835/233784519-0776cdc9-4323-4ccc-8725-6899bc0b5f3c.png)

* `charon-logging.conf`: é o arquivo de configuração de logging do StrongSwan, usado para definir como o daemon deve registrar informações. Você pode usar esse arquivo para ajustar a quantidade e o nível de detalhes dos logs do StrongSwan.
* `charon.conf`: é o arquivo de configuração principal do daemon Charon. Ele contém configurações que afetam diretamente o comportamento da conexão VPN, como a autenticação e a criptografia.
* `starter.conf`: é o arquivo de configuração do processo Starter, que é usado para iniciar o daemon Charon e configurar seu comportamento. Ele contém configurações que afetam a inicialização do StrongSwan e o gerenciamento de conexões VPN.



