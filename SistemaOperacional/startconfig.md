<div align="center">

![giphy](https://user-images.githubusercontent.com/104470835/226196198-05907f48-d3ee-4e9c-8f67-58d6f8083a71.gif)
</div>

Nesta parte do projeto irei instalar ferramentas, utilitários e configurações necessárias e úteis para usarmos no nosso servidor. E a medida que avanço na implementação dos serviços, irei atualizar essa página com novas informações.

*OBS: Todas as instalações e configurações serão feitas sempre em root, se você não sabe o que é veja esse [link](https://help.ubuntu.com/kubuntu/desktopguide/pt_BR/root-and-sudo.html)*

## Configurações e atualizações iniciais
Primeiro vamos colocar a placa de rede da máquina virtual em modo bridge (mais sobre o modo bridge veja um resumo nesse [link](https://tecnoblog.net/responde/modo-bridge-ou-router-qual-a-diferenca-e-por-que-usar/#:~:text=O%20que%20%C3%A9%20modo%20Bridge,risco%20de%20problemas%20de%20desempenho.)) com o a placa de rede da máquina real. Você vai em dispositivos, seleciona rede e irá aparecer conforme a imagem abaixo. Selecione *placa em modo brige* e depois ok. 

![image](https://user-images.githubusercontent.com/104470835/226183919-dbbc7e2b-ed3a-415a-b495-19c28a2cfab9.png)

Agora vamos atualizar o gerenciador de pacotes do sistema com o seguinte comando `sudo apt update`, aguarde um pouco e digite `sudo apt upgrade`. Irá ser perguntado ser você deseja prosseguir, digite `y` e aguarde. Pronto, o seu sistema está atualizado e se quiser reiniciar fica a seu critério. Agora podemos prosseguir.

## Nano

O nano é simplesmente um editor de texto de linha comando com uma interface bem intuitiva. Irei utilizar ele diversas vezes no decorrer desse projeto. O comando para instalar segue o mesmo padrão: `sudo apt install nano`.

## Net-tools

O próximo utilitário muito útil (pareceu um pleonasmo kkk) é o net-tools. Ele é um conjunto de ferramentas de rede para sistemas operacionais baseados em Linux e Unix, que permite aos usuários configurarem e diagnosticarem a rede. Contém uma série de comandos que o tornam uma ferramenta muito completa e certamente usaremos diversas vezes no decorrer desse trabalho. O comando para instalar segue o mesmo padrão `sudo apt install net-tools`.<br>
![image](https://user-images.githubusercontent.com/104470835/226186102-459004ce-9f94-4c6a-9ef5-175bc2533dc2.png)

## Inetutils-ping 

Outra ferramenta interessante pra termos é o inetutils-ping. É um utilitário de rede que permite verificar a conectividade entre dispositivos em uma rede IP usando o protocolo ICMP, o famoso *ping*. Para instalar basta digitar o comando `sudo apt update inetutils-ping`.<br>
E como já estamos aqui, vou fazer um teste de ping para verificar se há conectividade entre minha máquina e a máquina virtual e também entre a máquina virtual e o DNS 1.1.1.1. Pra isso vou chamar o comando `ifconfig` para obtermos o IP e em seguida utilizar o comando `ping`.<br>
![image](https://user-images.githubusercontent.com/104470835/226197661-47edc517-31f2-4c0b-a695-13c8a5e71786.png)
Na imagem temos ali em cima o IP do servidor `192.168.100.116`. Veja que o ping para minha máquina real (`192.168.100.124`) e para o Cloudflare(1.1.1.1) está tendo resposta. Isso significa que está tudo ok com a conexão.

## OpenSSH

<div align="center">

![openssh-1](https://user-images.githubusercontent.com/104470835/226182754-280a8395-4da9-465e-bc1b-f6418353a10a.png)

</div>

Agora vamos a nossa ferramenta essencial para podermos conectar remotamente com o servidor. Vou instalar ele, principalmente, para poder usar o terminal da minha máquina real que, diga-se de passagem, é melhor do que vimos até agora no server (kkk). E aqui vai um resumo do que é o OpenSSH:<br>

*O OpenSSH é um conjunto de ferramentas de software livre que permite a conexão segura de redes através do protocolo SSH. Ele inclui um cliente SSH para conexões remotas, um servidor SSH para permitir acesso remoto seguro ao sistema e ferramentas auxiliares para gerenciamento de chaves de criptografia e autenticação de usuários. É amplamente utilizado em sistemas Linux e Unix e é uma das principais soluções para conectar-se com segurança a servidores remotos.*

Se quiser saber mais, pode acessar o site oficial neste [link](https://www.openssh.com/).<br>
A instalação é muito simples, basta digitar o comando `sudo apt install openssh-server` e aguardar instalar.<br>
![image](https://user-images.githubusercontent.com/104470835/226185414-b114d4a1-7846-4fd3-b9b6-becbcf63f61d.png)

Pronto, já podemos conectar remotamente ao servidor mas antes vou fazer uma pequena alteração na porta padrão que irá dar-se essa conexão. E para isso vamos abrir com o `nano` e editar o arquivo `sshd_config`, basta digitar esse comando `nano /etc/ssh/sshd_config`. No arquivo descomente a linha da porta, aqui você pode deixar a padrão 22 ou escolher uma de sua preferência que no meu caso é a porta 222. Depois disso salve o arquivo digitando `ctrl+o`ou `ctrl+x` e depois `y` seguido de enter.<br>

![image](https://user-images.githubusercontent.com/104470835/226198874-87e8023f-9803-4223-b492-6fd7dc27bc22.png)<br>

Depois de feito isso, reinicie o serviço com o seguinte comando `/etc/init.d/ssh restart`.<br>
![image](https://user-images.githubusercontent.com/104470835/226199086-3a2f7d0b-f890-4d59-b455-80ba2691a664.png)<br>
E se tudo deu certo, já podemos conectar remotamente atráves da seguinte sintaxe: **ssh [USUARIO]@[IP-DO-SERVIDOR] -p [PORTA]**. Irei fazer isso no terminal do meu pc:<br>
![image](https://user-images.githubusercontent.com/104470835/226199391-b4179303-5b7e-4a07-b762-070bedeee91f.png)<br>
Primeiro, veja como é bonito meu terminal em comparação ao do servidor (kkkk). Segundo, perceba que é preciso o login e a senha que você usa pra entrar no servidor e que depois de autenticado aquele nome no começo do comando muda. Veja que antes estava `wnot` e agora é o nome do servidor `rdtwo`, isso significa que deu tudo certo e vocẽ pode usar o terminal do servidor de qualquer lugar.<br>

A partir de agora usarei o meu terminal nativo nas próximas etapas porque o acho mais legível.
 
