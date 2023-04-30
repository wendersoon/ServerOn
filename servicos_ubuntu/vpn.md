# SERVIDOR VPN

<div align = "center">

<img src=https://user-images.githubusercontent.com/104470835/233728802-87489d86-361d-486c-8199-8fc3d15ff07a.png width="400"/>

</div>

Uma VPN (**Virtual Private Network**) é uma rede privada virtual que permite que os usuários se conectem à internet de forma segura e privada. Ela cria um canal **criptografado** entre o dispositivo do usuário e a rede VPN, protegendo assim as informações transmitidas de interceptações e violações de privacidade. 

Ao se conectar a uma VPN, o tráfego de internet do usuário é redirecionado por um servidor VPN, que funciona como um intermediário entre o dispositivo do usuário e a internet, tornando o endereço **IP do usuário anônimo**. Isso permite que o usuário acesse a internet de forma segura, sem o risco de ser rastreado ou monitorado por terceiros, além de acessar conteúdos restritos geograficamente, como serviços de streaming, que podem ser bloqueados em sua localização. 

As VPNs são amplamente utilizadas em empresas e organizações que precisam garantir a segurança das informações transmitidas pela internet, bem como por usuários individuais que desejam proteger sua privacidade online e acessar conteúdos restritos.


<div align = "center">
  
![download](https://user-images.githubusercontent.com/104470835/233728511-c468bfb2-3772-4d19-a1b9-b82585b17224.png)

</div>


E nesta etapa do projeto, iremos instalar e configurar nosso próprio serviço VPN e para isso instalaremos o ***strongswan***. Ele é um software de código aberto que implementa o protocolo **IPSec** para fornecer segurança em comunicações de rede, permite criar uma rede privada virtual (VPN) segura e criptografada entre dois ou mais dispositivos, é muito implementado em sistemas Linux além de suportar outros sistemas operacionais como o Windows e Android. Se você se interessou e quer saber mais sobre o ***strongswan*** pode está acessando a (página oficial)(https://www.strongswan.org/). 

Antes de prosseguirmos a instalação e configuração, é importante entendermos o que é o IPSec (**Internet Protocol Security**), que nada mais é que um conjunto de protocolos e técnicas utilizadas para proteger a comunicação através da internet. Ele fornece segurança no **nível do protocolo de internet (IP)** para garantir que os dados sejam transmitidos de forma segura e protegida contra interceptação, alteração ou falsificação por terceiros. Nesse [artigo da UFRJ](https://www.gta.ufrj.br/ensino/eel878/redes1-2016-1/16_1_2/vpn/vpn_ipsec2/vpn_ipsec/ipsec.html) você pode está se aprofundado no tema e sugiro que faça isso, pois é assunto muito interessante.

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

Para instalar use o comando `apt-get install strongswan`. Diferente dos outros serviços que vimos antes, o **strongswan** instala mais de um diretório, além de arquivos de configuração que se localizam dentro do diretório `/etc/`. Vamos ver cada um deles, começando pelos diretórios criados com a instalação

### `/etc/strongswan.d`:

![image](https://user-images.githubusercontent.com/104470835/233783661-2b0d5705-09f4-4547-bd08-5f6e19ed9231.png)

*Se você quiser ver as informações sobre esses arquivos na documentação, acesse esse [link](https://docs.strongswan.org/docs/6.0/config/config.html)*

* `/charon`: esse diretório contém arquivos de configuração relacionados ao *daemon charon*, que é o componente central do strongswan. Os arquivos contém trechos de **configuração comentados** para todos os plugins ativados e instalados. Não vou aprofundar, pois como vê na imagem abaixo são muitos os arquivos

![image](https://user-images.githubusercontent.com/104470835/233784519-0776cdc9-4323-4ccc-8725-6899bc0b5f3c.png)

* `charon-logging.conf`: esse arquivo contém configurações para registrar a saída gerada pelo *daemon charon*, incluindo o nível de log e o destino (por exemplo, console, arquivo);
* `charon.conf`: esee arquivo contém as principais configurações do daemon charon;
* `starter.conf`: esse arquivo contém configurações usadas pelo script de inicialização strongswan, que é responsável por iniciar e parar o serviço VPN. Ele contém opções como o endereço IP e a porta usada pela VPN, as interfaces de rede usadas e os algoritmos preferidos usados.

### `/etc/ipsec.d/`:

![image](https://user-images.githubusercontent.com/104470835/233785482-86948350-4581-42b4-8070-cd9538980783.png)

O diretório `/etc/ipsec.d` é usado pelo strongswan para guardar arquivos de configuração e certificados usados em conexões IPSec. Se você abrir esses diretórios verá que estão vazios. Então não entraremos em detalhes aqui mas se sentiu-se curioso, acesse a [documentação](https://wiki.strongswan.org/projects/strongswan/wiki/IpsecDirectory).

### Arquivos No Diretório `/etc/`:

* `strongswan.conf`: esse arquivo é usado para configurar as opções globais que se aplicam a toda a instalação do strongswan;
* `ipsec.conf`: é o arquivo de configuração usado pelo strongSwan para configurar conexões VPN baseadas em IPsec;
* `ipsec.secrets` é um arquivo de configuração usado para armazenar senhas/chaves compartilhados usados para autenticação em conexões VPN baseadas em IPsec. Para mais detalhes, [veja aqui](https://wiki.strongswan.org/projects/strongswan/wiki/IpsecSecrets).


Bom, esse foram os arquivos e diretórios que foram criados com a instalação do *strongswan*. Agora vamos a configuração!

## Configuração

1. Vamos abrir o arquivo de configuração do IPSec com o comando `nano /etc/ipsec.conf`. Perceba que o arquivo é dividido em 3 secções que são:

![Screencast from 22-04-2023 10_55_23](https://user-images.githubusercontent.com/104470835/233789209-9669d813-cca5-4197-8797-22a367179039.gif)

* `config`: essa secção contém opções de configuração global para o strongswan;
* `conn`: essa secção contém opções de configuração para uma **conexão VPN específica**. Cada secção `conn` inclui opções relacionadas aos endereços IP locais e remotos, o método de autenticação, os algoritmos criptográficos a serem usados e outras opções específicas de conexão. Várias seções "conn" podem ser incluídas no arquivo "ipsec.conf" para definir várias conexões VPN*.
* `ca`: essa secção é usada para definir uma autoridade de certificação (CA) usada para autenticar certificados usados em conexões VPN baseadas em IPsec.

Antes de tudo crie um arquivo de backup com o comando `cp /etc/ipsec.conf /etc/ipsec.conf.backup`, pois se tudo der errado podemos restaurar a versão padrão. Agora apague tudo de `ipsec.conf` com o comando `> ipsec.conf` e vamos adicionar as seguintes configuraçãos no arquivo padrão(`ipsec.conf`), lembrando que as configurações são distintas no servidor e cliente:

* **SERVIDOR**:

```
config setup
    charondebug="all"
    strictcrlpolicy=yes
    uniqueids=no

conn my_ipsec_connection
    authby=psk
    keyexchange=ikev2 
    left=192.168.0.101
    right=192.168.0.107
    auto=start
    type=transport
    esp=aes128-sha1-modp2048
```

* **CLIENTE**:
```
config setup
    charondebug="all"
    strictcrlpolicy=yes
    uniqueids=no

conn my_ipsec_connection
    authby=psk
    keyexchange=ikev2 
    left=192.168.0.107
    right=192.168.0.101
    auto=start
    type=transport
    esp=aes128-sha1-modp2048
```

Você deve está se perguntado, onde está a diferença nessas configurações? Perceba nas diretivas `left` e `right` que os IP's estão "invertidos". Isso acontece porque essas diretivas referem-se aos endereçoes IP dos pontos finais da conexão IPsec a partir da perspectiva do própio host que está sendo configurado. Ou seja, o `left` é o endereço IP do host local e o `right` é o endereço IP do host remoto, a partir da perspectiva do host em que a configuração está definida. Fiz uma imagem que esclarece essa explicação, veja abaixo:

![image](https://user-images.githubusercontent.com/104470835/235372943-2482ccf5-2ad0-4a6f-9316-960dffaa5c78.png)

Certos desse detalhe, vamos entender o que cada diretiva significa na configuração que implementamos:

* `config setup`: como já falamos, é a diretiva que define as opções globais de configuração para o conjunto do arquivo de configuração;

* `charondebug="all"`: define o nível de depuração do daemon do IPSec, o charon, para "all", exibindo todas as mensagens de depuração, muito bom se você que uma depuração completa;

* `strictcrlpolicy=yes`: define a política de verificação de certificado para ser rigorosa;

`uniqueids=no`: define se os identificadores únicos são permitidos ou não;

* `conn my_ipsec_connection`: essa diretiva define uma nova conexão do IPSec chamada "my_ipsec_connection". As configurações específicas da conexão são definidas abaixo dela;

* `authby=psk`: especifica que a autenticação será feita usando Pre-Shared Key (PSK). PSK é um método de autenticação em que as duas partes envolvidas na comunicação compartilham uma chave secreta que é usada para autenticar a conexão, logo abaixo vamos definir a chave que usaremos;

* `keyexchange=ikev2`: essa diretiva informa que o protocolo de troca de chaves a ser usado na negociação de segurança é o *Internet Key Exchange version 2* (IKEv2);

* `left`: como mencionei antes, essa diretiva especifica o endereço IP do host local, ou seja, o host que está executando o arquivo de configuração;

* `right`: e também abordado mais acima, essa diretiva nos diz o endereço IP do host remoto, isto é, o host com o qual o host local está se comunicando;

* `auto=start`: define que a conexão deve ser iniciada automaticamente quando o daemon do IPSec é iniciado.

* `type=transport`: define que o modo de operação do IPSec é o modo transporte. No modo transporte, o cabeçalho original do pacote IP é mantido, mas os dados do pacote são criptografados e protegidos com uma integridade de mensagem. Esse modo é geralmente usado quando se quer proteger o tráfego de dados entre hosts individuais em uma rede. O outro modo disponível é modo túnel que encapsula todo o pacote IP original dentro de um novo pacote IP, adicionando um novo cabeçalho ao pacote encapsulado e é frequentemente usado para conectar duas redes remotas, onde todo o tráfego entre as redes é protegido;

* `esp=aes128-sha1-modp2048`: e por fim, essa diretiva define o conjunto de algoritmos de criptografia e autenticação que serão usados para proteger a comunicação. No caso, o conjunto especificado usa o algoritmo de criptografia AES com chave de 128 bits, o algoritmo de autenticação SHA-1 e um grupo de modos de diferença finita de 2048 bits (MODP2048) para a troca de chaves Diff.

2. Vamos adicionar a chave compartilhada PSK que será usada para autenticar nossa conexão VPN. O arquivo onde é configurando chama-se `ipsec.secrets` e como mostrei mais acima, ele também se localiza no diretório `/etc/`. O padrão para adicionar a senha é `<IP-LOCAL IP-REMOTO: PSK "senha">`, lembrando que o IP local é sempre o que está configurado na diretiva `left` como expliquei acima.

Portanto, após abrir o arquivo com `nano /etc/ipsec.secrets`, adcionei as seguintes configurações:

* **SERVIDOR**:

![image](https://user-images.githubusercontent.com/104470835/235376528-224f6e88-35e7-4fcd-b680-ca1316e980d4.png)

* **CLIENTE**:

![image](https://user-images.githubusercontent.com/104470835/235376541-3897575b-9ea8-4392-9343-5ab495f9a5bd.png)

Salve os arquivos e reinicie o serviço com o comando `sudo /etc/init.d/ipsec restart`.

3. Vamos verificar os status do serviço em ambas as máquinas com o comando `sudo /etc/init.d/ipsec status`:

* **SERVIDOR**:

![image](https://user-images.githubusercontent.com/104470835/235377090-dacd5294-dd36-4b08-bf5a-547ff4fc3a8a.png)

* **CLIENTE**:

![image](https://user-images.githubusercontent.com/104470835/235377140-33daeb68-e07d-40bd-adca-f6dbdefd638f.png)

Perceba que a conexão vpn usando o ipsec foi estabelecida entre as duas máquinas e o nome da conexão é como definimos na diretiva `conn my_ipsec_connection`. 

## Teste do Servidor

Vou utilizar o comando `watch ipsec statusall` que serve para monitorar o status de todas as conexões IPsec ativas no sistema em tempo real. Quando executamos esse comando, ele exibe uma tabela que mostra as informações de cada conexão, incluindo seu estado (estabelecido, iniciado, fechado etc.), os endereços IP das extremidades (left e right), a política de segurança (SA) e outras informações. Vejamos como é apresentado:

* **SERVIDOR**

![Screencast from 30-04-2023 19_17_13](https://user-images.githubusercontent.com/104470835/235378738-ba2c9211-9591-4dae-adb9-a5ae998cc2bb.gif)

* **CLIENTE**

![Screencast from 30-04-2023 19_24_20](https://user-images.githubusercontent.com/104470835/235378772-d77ca4e1-71d0-4088-957d-4f3d7e42da69.gif)

Agora, como segundo teste, vou dá ping da máquina cliente para o servidor com `ping IP-DO-SERVIDOR` e capturar os pacotes com wireshark. Veja como os pacotes aparecem:

![image](https://user-images.githubusercontent.com/104470835/235379092-8d933b71-3efe-4c97-abf5-f6db56825574.png)

Perceba que os pacotes de PING estão com o protocolo ESP (*Encapsulating Security Payload*) que é um dos protocolos usados no IPsec para fornecer serviços de segurança de rede, ele protege os dados transmitidos entre as extremidades da conexão VPN, encapsulando-os em um pacote criptografado.

E por fim, como teste final, vou acessar o meu site `www.meusite753.com` e capturar os pacotes com wireshark:

![Screencast from 30-04-2023 20_19_01](https://user-images.githubusercontent.com/104470835/235380586-18bfc0e5-b3e0-4a22-a936-8d845d085b55.gif)

Perceba que cada vez que atualizo a página, os pacotes que são detectados pelo wireshark são do protocolo ESP. Isso significa que toda minha comunicação com o meu servidor está criptografada.

**ESTÁ FUNCIONANDO!**

Muito obrigado se você leu até aqui!

<div align="center">

![46eae4c4ff91c03514a7096538740b42](https://user-images.githubusercontent.com/104470835/235380869-dd87cc58-1b86-4f5a-8619-385076110b41.gif)

</div>








