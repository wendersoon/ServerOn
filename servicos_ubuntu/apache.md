
![apache-logo](https://user-images.githubusercontent.com/104470835/227715165-7a255a95-739c-45b3-b77a-ebe4e7c10f6e.png)

O Apache é um servidor web de código aberto e gratuito, desenvolvido pela [Apache Software Foundation](https://www.apache.org/). Ele é usado para hospedar sites e aplicativos web, permitindo que os usuários acessem esses recursos por meio de seus navegadores. Dentro os servidores desse tipo, ele é o mais utilizado no mundo todo alimentando cerca de [46%](https://www.hostec.com.br/o-que-e-apache-web-server/) de páginas hospedadas na internet. Apesar de o chamarmos de servidor web, ele não é um servidor físico e sim um software executado em um servidor, ou seja, o trabalho dele é fazer a conexão entre os servidores e navegadores de sites enquanto entrega arquivos entre eles.<br>

O apache é um software multiplataforma, isto é, ele roda em diversos sistemas operacionais e também suporta vários protocolos web como HTTP, HTTPS, FTP etc. Uma das principais vantagens do Apache está na sua arquitetura baseada em módulos, o que significa que é possível adicionar ou remover funcionalidades facilmente de acordo com as necessidades do usuário tornando-o assim um software de alta escalibilidade e modularidade. Se caso quiser saber mais informações pode visitar o site [oficial](https://www.apache.org/) ou esse [artigo](https://www.hostinger.com.br/tutoriais/o-que-e-apache) na hostinger.<br>

Neste parte do nosso projeto, iremos fazer a instalação do Apache e ver algumas de suas configurações básicas, lembrando que:

```
Toda vez que for feita uma alteração, reinicie o serviço com o comando: 

```
Assim as alterações entrarão em vigor e se você tiver errado em alguma alteração, irá perceber mais rapidamente.<br>

*Todos os comandos seguintes faça em modo root*<br>

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

Para verificarmos se a instalação deu certo podemos acessar o *localhost* do nosso servidor atráves do navegador. E para isso basta colocar o IP do servidor pois estamos o acessando remotamente. Veja o resultado:<br>
![image](https://user-images.githubusercontent.com/104470835/227719259-1d5f65b5-8a6b-4e6e-ba7c-a06c6ba62e46.png)

**It works!**



