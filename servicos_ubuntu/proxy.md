
# SERVIDOR PROXY

![0_c_9_5AFpbROPZkgM](https://user-images.githubusercontent.com/104470835/232338513-9449c3d1-6f37-4f19-9586-2803d76be3a0.gif)

Um servidor **proxy** é um intermediário entre um cliente e um servidor, que permite que o cliente acesse recursos na Internet por meio do servidor proxy. O servidor proxy atua como um "proxy" para o cliente, encaminhando suas solicitações para o servidor de destino e, em seguida, encaminhando as respostas de volta para o cliente. Isso pode ajudar a melhorar a velocidade de acesso a recursos na Internet, reduzir o uso de banda, fornecer segurança adicional e permitir o acesso a recursos restritos por IP. Se quiser saber mais como funciona o proxy, veja esse [artigo da Hostiger](https://www.hostinger.com.br/tutoriais/servidor-proxy).

<div align="center">
  
![squid-logo-png](https://user-images.githubusercontent.com/104470835/232339288-39561db9-2b0b-414b-b5fb-75da234ad849.png)

 </div>

E nesta etapa do projeto, iremos instalar e configurar nosso servidor proxy. Para isso instalaremos o **Squid** que é um servidor proxy de código aberto que pode ser executado em sistemas Unix, Linux, macOs e Windows. É amplamente utilizado por empresas, organizações governamentais e provedores de serviços de internet para melhorar o acesso à internet de seus usuários, tendo por isso grande relevância quando se pensa em montar um servidor proxy. Se você quiser saber mais sobre o Squid acessa a [página oficial](http://www.squid-cache.org/).

***Todos os comandos serão em modo root***

## Instalação

Para instalar você deve utilizar o seguinte comando `sudo apt-get install squid`. Concluída a instalação vejamos o diretório do serviço no caminho /etc/squid/.

![image](https://user-images.githubusercontent.com/104470835/232340079-405e78f3-d592-42eb-b439-83b88602039b.png)

Antes de prosserguimos é importante termos um resumo dos arquivos que estão nesse diretório que são arquivos de configuração do servidor, são eles:

* `squid.conf`: este arquivo contém todas as configurações principais do Squid, como portas de escuta, diretórios de log, políticas de cache, autenticação de usuários, filtros de conteúdo, entre outras.

* `conf.d`: é um diretório usado por algumas distribuições do Linux, como o Ubuntu, para armazenar arquivos de configuração adicionais do Squid. 

* `errorpage.css` é um arquivo css usado para personalizar a página de erro do Squid. A página de erro é exibida quando um usuário tenta acessar um site bloqueado ou quando ocorre um erro no servidor proxy. Nesse arquivo, podemos ter uma página personalizada com o logo da empresa etc.

Como já dito anteriormente em instalações de outros serviços, aqui irei trabalhar apenas com as configurações básicas do servidor, que se encontram no arquivo `squid.conf`, para que tenhamos um serviço minimamente funcional.

