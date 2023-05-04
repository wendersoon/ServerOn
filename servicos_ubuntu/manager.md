# GERENCIAMENTO DE SISTEMA

<div align="center" >

![servidor-gerenciado-800x450](https://user-images.githubusercontent.com/104470835/235524202-28f50cbf-6d7b-4019-9002-d34f0a67c382.jpg)

</div>


Chegamos ao "ápice" do nosso projeto, isto é, iremos implementar um serviço que organiza todos os demais serviçoes que implementamos anteriormente em um só lugar. De todos os tutoriais, esse será o mais curto mas não menos importante. 

Sabemos que gerenciar um servidor com um ou mais serviços não é uma tarefa fácil, pensando nisso desenvolveram-se muitas ferramentas que ajudam nessa gestão. E nessa parte do projeto instalaremos duas ferramentas que podem ser muito úteis, são elas: Webmin e/ou Monitorix.

## Webmin

<div align="center" >

![Webmin_Logo](https://user-images.githubusercontent.com/104470835/235525732-19c79c3f-30a4-4913-9949-5f25a17e8299.png)

</div>

O Webmin é uma ferramenta de gerenciamento de sistema baseada na web, que permite que os usuários administrem vários aspectos de um servidor através de uma interface gráfica de usuário. Ele é executado em um servidor web e fornece uma interface fácil de usar para tarefas administrativas de rotina, como gerenciamento de arquivos, configuração de redes, gerenciamento de usuários e permissões, gerenciamento de serviços de sistema etc. O Webmin é gratuito e de código aberto, se você quiser saber mais acesse a [página oficial](https://webmin.com/).

### Instalação

Para instalação iremos seguir o [tutorial disponível](https://webmin.com/download/) no site o qual irei replicar aqui para facilitar o trabalho.

1. Copie o script disponível no github do Webmin que irá realizar todas as instalações e atualizações necessárias, o link é:

```
https://raw.githubusercontent.com/webmin/webmin/master/setup-repos.sh
```

2. Crie um arquivo chamado `setup-repos.sh` e cole tudo o que apareceu na página do link.

3. Agora digite o comando `sh setup-repos.sh` para adicionar o repositório do webmin no sistema.

4. E por fim, digite `sudo apt-get install webmin`

### Teste do Serviço

Após a conclusão da instalação, podemos acessar a ferramenta no navegador atráves da seguinte sintaxe `https://<IP_do_servidor>:10000`. No meu caso fica assim `https://192.168.0.101:10000`. Veja a primeira tela do serviço:

![image](https://user-images.githubusercontent.com/104470835/235533794-d12dbd1d-e94e-42eb-9ffe-b4c3a365ec56.png)

As credenciais de acesso são as mesmas do usário root do sistema. Agora daqui em diante você pode está explorando a ferramenta, eu irei mostrar apenas algumas telas abaixo.

* Mudar a linguagem do sistema de gerenciamento:

![image](https://user-images.githubusercontent.com/104470835/235534543-91e35cc8-1663-4775-876d-a815b4f0df36.png)

* Serviços/Servidos implementados na máquina:

![image](https://user-images.githubusercontent.com/104470835/235534879-f4949af1-f3dd-4441-9ae7-74eaacde23c3.png)

* Uma página dedicada a mostrar todas as atualizações disponíveis dos utilitários e bibliotecas do servidor:

![image](https://user-images.githubusercontent.com/104470835/235535119-ed8908ec-de64-4fa6-b38b-36119bf1908d.png)

* Telas dos servidores com configurações completas ou quase completas, na imagem abaixo está o DNS Bind que configuramos:

![image](https://user-images.githubusercontent.com/104470835/235535490-bbb200c6-ef31-44fc-9e30-35024d2fcefc.png)

* Um painel de controle muito útil:

![image](https://user-images.githubusercontent.com/104470835/235535697-61934285-c252-4556-9f50-69a6c51b4b74.png)

## Monitorix

<div align="center" >

![monitorixlogo-removebg-preview](https://user-images.githubusercontent.com/104470835/236288565-3c3d43fa-aefe-4b1d-9aae-d25d87b42fd4.png)

</div>

O Monitorix é um software livre de monitoramento de sistema que exibe informações em tempo real sobre o desempenho do sistema e dos componentes, como CPU, memória, disco, rede e temperatura. Ele pode ser usado em sistemas baseados em Linux e BSD. Ele apresenta uma interface bem "retrô" e é bem fácil de utilizar. Ele coleta dados do **sistema em intervalos regulares** e exibe essas informações em gráficos e tabelas, também possui recursos avançados, como alertas de limite e notificações por e-mail. Se você deseja saber mais acesse a [página oficial](https://www.monitorix.org/).

### Instalação e Teste

Para instalação utilize o comando `sudo apt install monitorix`. Terminado a instalação, já está pronto para uso basta que você acesse no navegador o seguinte link `http://localhost:8080/monitorix`, lembrando que no lugar de `localhost` você deve susbtituir pelo IP do servidor. Irei mostrar algumas telas do software logo abaixo:

* Tela inicial

Terminamos por aqui essa parte do projeto. Obrigado pela leitura!

<div align = "center"> 

![giphy (1)](https://user-images.githubusercontent.com/104470835/235536391-526ac11f-4554-4683-b810-c3a9cf77240e.gif)

</div>




