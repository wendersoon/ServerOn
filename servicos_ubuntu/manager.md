# GERENCIAMENTO DE SISTEMA

<div align = "center" >

![servidor-gerenciado-800x450](https://user-images.githubusercontent.com/104470835/235524202-28f50cbf-6d7b-4019-9002-d34f0a67c382.jpg)

</div>


Chegamos ao "ápice" do nosso projeto, isto é, iremos implementar um serviço que organiza todos os demais serviçoes que implementamos anteriormente em um só lugar. De todos os tutoriais, esse será o mais curto mas não menos importante. 

Sabemos que gerenciar um servidor com um ou mais serviços não é uma tarefa fácil, pensando nisso desenvolveram-se muitas ferramentas que ajudam nessa gestão. E nessa parte do projeto instalaremos uma ferramenta muito útil chamada Webmin.

![Webmin_Logo](https://user-images.githubusercontent.com/104470835/235525732-19c79c3f-30a4-4913-9949-5f25a17e8299.png)


O Webmin é uma ferramenta de gerenciamento de sistema baseada na web, que permite que os usuários administrem vários aspectos de um servidor através de uma interface gráfica de usuário. Ele é executado em um servidor web e fornece uma interface fácil de usar para tarefas administrativas de rotina, como gerenciamento de arquivos, configuração de redes, gerenciamento de usuários e permissões, gerenciamento de serviços de sistema etc. O Webmin é gratuito e de código aberto, se você quiser saber mais acesse a [página oficial](https://webmin.com/).

## Instalação

Para instalação iremos seguir o [tutorial disponível](https://webmin.com/download/) no site o qual irei replicar aqui para facilitar o trabalho.

1. Copie o script disponível no github do Webmin que irá realizar todas as instalações e atualizações necessárias, o link é:

```
https://raw.githubusercontent.com/webmin/webmin/master/setup-repos.sh
```

2. Crie um arquivo chamado `setup-repos.sh` e cole tudo o que apareceu na página do link.

3. Por fim digite o comando `sh setup-repos.sh` para instalar o Webmin.





