
![apache-logo](https://user-images.githubusercontent.com/104470835/227715165-7a255a95-739c-45b3-b77a-ebe4e7c10f6e.png)

O Apache é um servidor web de código aberto e gratuito, desenvolvido pela [Apache Software Foundation](https://www.apache.org/). Ele é usado para hospedar sites e aplicativos web, permitindo que os usuários acessem esses recursos por meio de seus navegadores. Dentro os servidores desse tipo, ele é o mais utilizado no mundo todo alimentando cerca de [46%](https://www.hostec.com.br/o-que-e-apache-web-server/) de páginas hospedadas na internet. Apesar de o chamarmos de servidor web, ele não é um servidor físico e sim um software executado em um servidor, ou seja, o trabalho dele é fazer a conexão entre os servidores e navegadores de sites enquanto entrega arquivos entre eles.<br>

O apache é um software multiplataforma, isto é, ele roda em diversos sistemas operacionais e também suporta vários protocolos web como HTTP, HTTPS, FTP etc. Uma das principais vantagens do Apache está na sua arquitetura baseada em módulos, o que significa que é possível adicionar ou remover funcionalidades facilmente de acordo com as necessidades do usuário tornando-o assim um software de alta escalibilidade e modularidade. Se caso quiser saber mais informações pode visitar o site [oficial](https://www.apache.org/) ou esse [artigo](https://www.hostinger.com.br/tutoriais/o-que-e-apache) na hostinger.<br>

Neste parte do nosso projeto, iremos fazer a instalação do Apache e ver algumas de suas configurações básicas, lembrando que **toda vez que for feita uma alteração, reinicie ou recarregue o serviço com os seguintes comandos (dependerão da sua necessidade)**:<br>

- CAMINHO PADRÂO: `sudo /etc/init.d/apache2 <COMANDOS>`
  - COMANDOS : 
    - `start` - inicia o Apache
    - `stop` - finaliza o Apache
    - `restart` - reinicia o Apache, efetuando uma pausa de 5 segundos entre interrupção do seu funcionamento e reinicio.
    - `reload` - recarrega os arquivos de configuração do Apache, as alterações entram em funcionamento imediatamente sem que o servidor recarregue com queda de conexões.
    - `reload-modules` - recarrega os módulos, o mesmo que um restart no servidor
    - `force-reload` - faz a mesma função que o reload só que de maneira forçada.

Por exemplo, você quer que o serviço recarregue uma configuração nova que você colocou mas não quer que os usuários perdam a conexão, então utilizamos o comando `sudo /etc/init.d/apache2 reload`.<br>

Fazendo isso todas as vezes que ocorre uma alteração, garantimos que erros possam ser detectados mais facilmente.<br>

***Todos os comandos serão em modo root***.<br>

## Instalação

Para instalar o Apache e sua documentação, vamos fazer isso com o seguinte comando:<br>
`sudo apt-get install apache2 apache2-doc`
Podemos o ver seu diretório no caminho `/etc/apache2/`<br>
![image](https://user-images.githubusercontent.com/104470835/227718843-fc1deea9-4123-4a10-8343-830dac5df52a.png)

Antes de prosserguimos é importante termos um resumo dos arquivos que estão no diretório `/etc/apache2/` que são arquivos de configuração do servidor, são eles:<br>

- **apache2.conf**: é o arquivo principal de configuração do Apache e define as configurações globais para o servidor, como os diretórios raiz do servidor, os módulos a serem carregados, o usuário e o grupo que o Apache deve usar, entre outros.

- **ports.conf**: este arquivo define as portas em que o Apache deve escutar por solicitações de clientes. As portas padrão são a 80 para HTTP e 443 para HTTPS.

- **conf-available/** e **conf-enabled/**: são diretórios que contêm arquivos de configuração para módulos adicionais do Apache. Você pode habilitar e desabilitar módulos usando os comandos `a2enconf` e `a2disconf`, respectivamente.

- **mods-available/** e **mods-enabled/**: são diretórios que contêm arquivos de configuração para módulos do Apache. Os arquivos aqui especificam as opções de configuração para cada módulo. Você pode habilitar e desabilitar módulos usando os comandos `a2enmod` e `a2dismod`, respectivamente.

- **sites-available/** e **sites-enabled/**: são diretórios que contêm arquivos de configuração para cada site virtual que o Apache pode servir. Cada arquivo especifica o nome de domínio ou endereço IP, a porta, o diretório raiz do site, entre outras configurações. Você pode habilitar e desabilitar sites usando os comandos `a2ensite` e `a2dissite`, respectivamente.

- **envvars**: é um arquivo que define variáveis de ambiente usadas pelo Apache, como o usuário e o grupo que o Apache deve usar.

- **magic**: é um arquivo de configuração que permite o Apache determinar o tipo MIME de um arquivo com base em seu conteúdo. Isso é importante para garantir que os navegadores possam exibir corretamente o conteúdo do arquivo.
Vamos verificar se o serviço está funcionando no sistema com o comando: sudo `systemctl status apache2`.<br>
![image](https://user-images.githubusercontent.com/104470835/227719951-c1d34272-ddbb-439d-ae64-e6da275af669.png)

Também podemos verificar acessando o IP do nosso servidor no navegador. Veja o resultado:<br>
![image](https://user-images.githubusercontent.com/104470835/227720026-23ecb624-311a-444b-b4f7-1f4459cf8f20.png)<br>
Essa página indica que está funcionado e nela vemos algumas informações básicas sobre arquivos importantes do Apache.<br>

**It works!**

## Faça isso primeiro!

No caminho `/etc/apache2/conf-enabled` há o arquivo **charset.conf** que é um arquivo de configuração usado pelo Apache para definir o conjunto de caracteres padrão no envio de páginas para o navegador. Dentro desse arquivo é especificado o padrão **UTF-8**, que é um conjunto de caracteres universal capaz de representar caracteres de todos os idiomas do mundo. Mas acontece que no arquivo essa configuração vem comentada o que pode ocasionar erros futuros, então a necessidade de verificar-lo primeiro.<br>
Vamos descomentar a linha que possui `#AddDefaultCharset UTF-8`e salvar com `ctrl+o`.<br>
![9d5ie-fdc8m](https://user-images.githubusercontent.com/104470835/227721738-6d38f99d-74fa-4d28-adb6-cbbeb7a10dc7.gif)
**Depois disso reinicie o serviço com o comando dado na introdução.**

## Alterando algo na página padrão
O diretório `/var/www/` é o local padrão para armazenar arquivos HTML, CSS, JavaScript, imagens e outros arquivos relacionados ao conteúdo de um site no Apache. É lá que ficarão os sites que você deseja colocar no servidor. Por exemplo, imaginemos que você queira ter mais um site no servidor chamado de *abacate*, para isso basta criar um novo diretório dentro da raiz no modelo `/var/www/dominio`, isto é, `/var/www/abacate` e dentro dele colocar os arquivos html, css e etc.<br>
Entendido esse ponto, basta que localizemos o arquivo html da página padrão do Apache. Ele está no caminho `/var/www/html/` como pode ser visto no gif abaixo, vamos abrir o arquivo `index.html` e modificar a frase **"It works!"** e depois salvar com `ctrl+o`.<br>

![xeeoz-ps11m](https://user-images.githubusercontent.com/104470835/227785159-dcdfe1a8-1489-4b70-a1fb-43e6cff31f3e.gif)

Feita a modificação, recarreguemos o serviço com `sudo /etc/init.d/apache2 reload` e vejamos a alteração na página padrão.<br>
![image](https://user-images.githubusercontent.com/104470835/227785375-344f6de6-2680-4314-a26b-e9eb061a1cc2.png)

## Adicionando um site no servidor

Agora vamos adicionar um novo site - não nos preocupemos com a questão de *domínio* por enquanto, vamos ver isso mais a frente quando montarmos o DNS - e para isso podemos dentro de `/var/www/` fazer uma cópia do diretório `html` para servir como modelo.<br>
Irei dá o nome ao novo diretório de `NovoSite` e lá dentro vou editar o código html para o site que quero e depois salvar. Para copiar o comando é `cp -r html NovoSite`, depois de criado vou deletar o arquivo `index.html` de dentro de `NovoSite` e criar com o comando `nano newindex.html` o novo arquivo html do nosso site. Veja abaixo: <br>

![1srit-omr9u](https://user-images.githubusercontent.com/104470835/227792356-ea7007e2-5b9d-4c0d-9e1a-87e8d3a7e2c4.gif)

Vamos habilitar nossa página para podermos ver a cara dele. Mas antes de fazermos isso temos de seguir os seguintes passos:<br>
- é preciso que desabilitemos o site atual (página padrão do Apache) com o comando `a2dissite <arquivo de configuração do site>`. Os arquivos de configuração dos sites que estão ativos você pode localizá-los no diretório `/etc/apache2/sites-enabled`, no caso em questão, o arquivo que queremos é o `000-default.conf` que depois de desabilitado é preciso **recarregar o Apache**.<br>
![image](https://user-images.githubusercontent.com/104470835/227792906-794a2567-4a4b-481f-8df1-9af3f28c61fc.png)

- O próximo passo é criarmos um arquivo de configuração para o site novo que criamos. E para isso temos que acessar o diretório `/etc/apache2/sites-available` e lá podemos usar o arquivo `000-default.conf` como modelo para a nossa copia que se chamará `NovoSite.conf` com o comando `cp 000-default.conf NovoSite.conf`. Dentro do arquivo de configuração, editamos o caminho que se encontra os arquivos do site. Veja no gif abaixo o procedimento:<br>
![1thvl-v94su](https://user-images.githubusercontent.com/104470835/227794092-622fca33-2f85-4b61-80f9-66a7ca7a9c1a.gif)

- Agora vamos ativar o novo arquivo de configuração do nosso site com o comando `a2ensite NovoSite.conf` e recarregar o Apache. Depois disso basta acessarmos no navegador e vermos a "cara" do site.<br>
![image](https://user-images.githubusercontent.com/104470835/227794570-fb99b911-6800-49ca-9c94-c0c9eff62929.png)

## Vendo os Logs do Apache

O logs são extremamente necessários pois são esses arquivos que registram todas as ações realizadas no nosso sistema ou aplicativo e por isso mesmo permitem que problemas possam ser solucionados. Se você tiver uma problema na instalação ou em algum arquivo de configuração sempre vá no log que ele terá a resposta.<br>
Para ver os arquivos de logs do apache você deve ir no caminho `/var/log/apache2`, dentro desse diretório temos três arquivos que são:
 - access.log - logs de acessos;
 - error.log - logs de erros;
 - other_vhosts_access.log - usado para registrar solicitações que não correspondem a nenhum dos outros arquivos de log.

Vejamos os logs de acessos com o comando `nano access.log`.<br>
![image](https://user-images.githubusercontent.com/104470835/227796176-e8ab46a9-5d05-44b7-8a7f-20eea87ebcc9.png)
Agora vejamos os logs de erros com o comando `nano error.log`.<br>
![image](https://user-images.githubusercontent.com/104470835/227796231-cbbcc34a-c76e-4eab-861c-18b0abe9bc20.png)
Se você quiser monitorar os acessos em tempo real atráves do log, pode usar o comando `sudo tail -f /var/log/apache2/access.log`.<br>
