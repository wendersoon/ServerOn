
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








