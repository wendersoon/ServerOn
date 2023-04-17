
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

## Um Pouco Antes da Configuração

***A configuração é logo após essa secção***

Antes de iniciarmos as configurações, é recomendável fazermos uma cópia do arquivo de configuração squid.conf. Isso se deve ao fato de que, como veremos mais adiante, é um arquivo extenso e muito bem documentado. Para fazer uma cópia de backup, utilize o comando `sudo cp /etc/squid/squid.conf /etc/squid/squid.conf.backup`. 

Depois de ter feito isso, abra o arquivo de configuração principal com o comando `sudo nano squid.conf` (lembre-se de estar no diretório correspondente), pare um momento e leia-o. Verá que dos serviços que trabalhamos até agora é o que apresenta uma documentação muito extensa e detalhada. Não abordaremos todos os aspectos desse documento, mas acho interessante darmos uma olhada nos principais para termos um entendimento geral do arquivo.

![image](https://user-images.githubusercontent.com/104470835/232581249-92d56fc0-477b-486f-a55c-28b808d971b8.png)

Conforme a imagem acima, são as recomendações de configurações mínimas que são necessárias para iniciar e usar o Squid como um servidor proxy. Preste atenção na diretiva `acl` que acompanha todas as linhas desmarcadas desse trecho. Essa diretiva (`acl` - Access Control List) é usada para definir uma **lista de controle de acesso** que pode ser usada para **permitir ou negar** o acesso a determinados recursos. Elas podem ser usadas em vários contextos, isto é, as `acl` podem ser definidas de várias maneiras no arquivo `squid.conf`, isto é, definidas por endereço IP, nome de domínio, expressões regulares, por lista de palavras-chave, por hora do dia, por tipo de arquivo etc., realmente a lista é longa. Por exemplo, a diretiva `acl Safe_ports port 21` está criando uma ACL chamada `Safe_ports` que permite ou nega o acesso à porta 21. 

![image](https://user-images.githubusercontent.com/104470835/232587024-b5fc43c1-932a-4902-8d6a-e70c9a4bd752.png)

Ademais, a diretiva `acl` é usada em conjunto com outra chamada `http_access` que é a que define as regras de acesso aos recursos da web com base em uma ou mais ACLs definidas **anteriormente** no arquivo `squid.conf` - veja que destaquei que precisa ser anterior porquê o arquivo de configuração é lido de cima para baixo - essa diretiva é usada para permitir ou negar o acesso aos recursos, também conta com dois parâmetros principais que são `allow` e `deny` (fazem justamente o que nome sugere). Por exemplo, temos na imagem acima o seguinte trecho `http_access deny !Safe_ports`, isso está me dizendo que a diretiva nega(`deny`) o acesso a todas as portas que **não**(`!`) estão listadas em `Safe_ports`. Exemplificando menlhor, se a ACL `Safe_ports` incluir as portas 80, 443, 8080 e 8443, a diretiva `http_access deny !Safe_ports` negará o acesso a todas as outras portas, exceto essas quatro portas seguras.

![image](https://user-images.githubusercontent.com/104470835/232590264-5156e9f0-d553-4179-9102-284f9d0f9778.png)

Nesse print temos duas diretivas que trabalham em conjunto e são muito importantes no uso de um servidor proxy. São elas as `cache_swap_low` e `cache_swap_high`, essas duas diretivas especificam, respectivamente, o limite inferior e superior para o uso do disco rígido em relação ao cache de objetos do Squid. Os valores definidos em cada uma são porcentagens do espaço total do disco dedicado ao cache, o primeiro valor significa que o Squid começará a remover objetos do cache quando o uso do disco atingir 90% e o segundo valor diz que o Squid começará a recusar novas solicitações de cache quando o uso do disco atingir ou ultrapassar 95%.

São muitas as personalizações que o Squid permite-nos, vimos apenas um punhado das principais configurações e não é meu objetivo, neste projeto, abordar cada uma delas e sim para que tenhamos um servidor proxy minimamente funcional. Então por isso, vamos as configurações!

## Configuração






