Nesta parte do projeto irei instalar ferramentas, utilitários e configurações necessárias muito úteis para usarmos no nosso servidor. E a medida que avanço na implementação dos serviços, irei atualizar essa página com novas informações, sempre mencionando em que parte do projeto fiz a instalação/configuração e se caso não aparecer essa menção é porque serve para todo momento.

*OBS: Todas as instalações e configurações serão feitas sempre em root, se você não sabe o que é veja esse [link](https://help.ubuntu.com/kubuntu/desktopguide/pt_BR/root-and-sudo.html)*

## Configurações e atualizações iniciais
Primeiro vamos colocar a placa de rede da máquina virtual em modo bridge (mais sobre o modo bridge veja um resumo nesse [link](https://tecnoblog.net/responde/modo-bridge-ou-router-qual-a-diferenca-e-por-que-usar/#:~:text=O%20que%20%C3%A9%20modo%20Bridge,risco%20de%20problemas%20de%20desempenho.)) com o a placa de rede da máquina real. Você vai em dispositivos, seleciona rede e irá aparecer conforme a imagem abaixo. Selecione *placa em modo brige* e depois ok. 

![image](https://user-images.githubusercontent.com/104470835/226183919-dbbc7e2b-ed3a-415a-b495-19c28a2cfab9.png)

Agora vamos atualizar o gerenciador de pacotes do sistema com o seguinte comando `sudo apt update`, aguarde um pouco e digite `sudo apt upgrade`. Irá ser perguntado ser você deseja prosseguir, digite `y` e aguarde. Pronto, o seu sistema está atualizado e se quiser reiniciar fica a seu critério. Agora podemos prosseguir.

## OpenSSH

<div align="center">

![openssh-1](https://user-images.githubusercontent.com/104470835/226182754-280a8395-4da9-465e-bc1b-f6418353a10a.png)

</div>

Vamos a nossa primeira ferramenta que é essencial para podermos conectar remotamente com o servidor e aqui vai um resumo do que é o OpenSSH:<br>

*O OpenSSH é um conjunto de ferramentas de software livre que permite a conexão segura de redes através do protocolo SSH. Ele inclui um cliente SSH para conexões remotas, um servidor SSH para permitir acesso remoto seguro ao sistema e ferramentas auxiliares para gerenciamento de chaves de criptografia e autenticação de usuários. É amplamente utilizado em sistemas Linux e Unix e é uma das principais soluções para conectar-se com segurança a servidores remotos.*

Se quiser saber mais, pode acessar o site oficial neste [link](https://www.openssh.com/).<br>
A instalação é muito simples, basta digitar o comando `sudo apt install openssh-server` e aguardar instalar.
![image](https://user-images.githubusercontent.com/104470835/226185414-b114d4a1-7846-4fd3-b9b6-becbcf63f61d.png)

## Nano

O nano é simplesmente um editor de texto de linha comando com uma interface bem intuitiva. Irei utilizar ele diversas vezes no decorrer desse projeto. O comando para instalar segue o mesmo padrão: `sudo apt install nano`.

## Net-tools

O próximo utilitário muito útil (pareceu um pleonasmo kkk) é o net-tools. Ele é um conjunto de ferramentas de rede para sistemas operacionais baseados em Linux e Unix, que permite aos usuários configurarem e diagnosticarem a rede. Contém uma série de comandos que o tornam uma ferramenta muito completa e certamente usaremos diversas vezes no decorrer desse trabalho. O comando para instalar segue o mesmo padrão `sudo apt install net-tools`.
![image](https://user-images.githubusercontent.com/104470835/226186102-459004ce-9f94-4c6a-9ef5-175bc2533dc2.png)


