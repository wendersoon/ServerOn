<div align = "center">

![selossl](https://user-images.githubusercontent.com/104470835/235451273-7f503c14-db97-49ea-a806-b975355833fa.png)

</div>

SSL (*Secure Sockets Layer*) é um protocolo de segurança que permite a criptografia de comunicações de rede entre um cliente e um servidor. Uma certificação SSL é um certificado digital que é emitido por uma autoridade de certificação (CA) confiável e serve para verificar a identidade do proprietário do site e garantir que as informações transmitidas estejam seguras. 

Quando um site possui um certificado SSL, ele utiliza uma chave criptográfica para criptografar as informações que são enviadas do cliente para o servidor e vice-versa, tornando praticamente impossível que um terceiro intercepte e leia essas informações. Além disso, o certificado SSL também exibe um ícone de cadeado verde na barra de endereço do navegador, o que indica que o site é confiável e seguro.

Já uma **certificação privada** (ou certificado de servidor autoassinado) é um tipo de certificado digital usado para proteger as comunicações em uma rede. Diferente dos certificados emitidos por autoridades de certificação confiáveis, uma certificação privada é emitida pelo próprio proprietário do servidor, sem a intervenção de uma terceira parte confiável. Se você quiser saber mais detalhes sobre as certificações, acesse esse [artigo do Serasa](https://serasa.certificadodigital.com.br/blog/mercado/o-que-e-o-certificado-digital-ssl-e-como-ele-pode-proteger-informacoes/) que detalha muito bem esse tema.

![OpenSSL_logo svg](https://user-images.githubusercontent.com/104470835/235452740-38f52a3d-28cf-4036-b8ef-54306be97d06.png)

E nessa parte do projeto iremos gerar um certificado privado para a página que hospedamos no nosso servidor (acredito que você tenha passado pelos dois tópicos anteriores) utilizando o OpenSSL que é um utilitário de criptografia de código aberto que fornece implementações de protocolos de segurança, como SSL/TLS (Secure Socket Layer/Transport Layer Security), PKI (Public Key Infrastructure), criptografia de chave simétrica e assimétrica, hash e outros algoritmos de criptografia. Se você quiser saber mais sobre esse projeto, acesse a [página oficial](https://www.openssl.org/).

***Todos os comandos serão em modo root***

## Um Pouco Antes da Configuração

Vou acessar a página que criei no tutorial do Apache2 atráves do domínio `www.meusite753.com` que configurei no tutorial do DNS. E vamos ver como está a certificação dele antes de implementarmos o SSL no servidor WEB:

![Screencast from 01-05-2023 10_33_47](https://user-images.githubusercontent.com/104470835/235458887-ccf315e3-1a9a-434a-8e75-5af1aacdf861.gif)

Perceba que não existe nenhum certificado ativo na página, após a configuração isso deve mudar.

## Instalação

1 . Para instalar você deve usar o comando `sudo apt-get install openssl`. A instalação não cria nenhum diretório específico mas pode criar alguns arquivos de configuração em diretórios existentes como, por exemplo, `etc/ssl/` que é onde geralmente são armazenados os arquivos de certificado e chave. Então, por isso, vamos dá uma olhada nesse diretório:

![image](https://user-images.githubusercontent.com/104470835/235455061-a707a3d9-1393-485b-8297-39e1970f6a43.png)

Vejamos um resumo:

* `certs`: é o diretório que é geralmente usado para armazenar os arquivos de certificado, que são usados para autenticar a identidade de um servidor ou cliente em uma conexão SSL/TLS;
* `openssl.cnf`: é o arquivo de configuração padrão do OpenSSL, que é usado para definir as opções de configuração do utilitário OpenSSL.;
* `private`: é geralmente usado para armazenar os arquivos de chave privada, que são usados para criptografar e descriptografar os dados transmitidos em uma conexão SSL/TLS.

Não vamos trabalhar com esse diretório.

2. O segundo passo é habilitar o módulo SSL no Apache2 e para isso use o comando `sudo a2enmod ssl`. Em seguida, reinicie o serviço com `sudo /etc/init.d/apache2 restart`.

3. Vamos criar um novo diretório para guardar o certificado privado que vamos gerar, isto é, os pares de chaves pública e privada. Para criar o diretório, utilize o comando `sudo mkdir /etc/apache2/ssl`. E para gerar o certificado utilize o comando `sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/apache2/ssl/apache.key -out /etc/apache2/ssl/apache.crt`. Vamos ver o que significa esse comando:

* `openssl`: é o comando do bash utilizado para acessar as funcionalidades do OpenSSL;

* `req`: é utilizado para gerar um certificado autoassinado;

* `-x509`: essa opção é utilizada para especificar que um certificado autoassinado deve ser gerado;

* `-nodes`: essa opção indica que a chave privada gerada não deve ser criptografada com uma senha;

* `-days 365`: define a validade do certificado autoassinado por 365 dias;

* `-newkey rsa:2048`: essa opção define que um novo par de chaves RSA deve ser gerado com um comprimento de 2048 bits;

* `-keyout /etc/apache2/ssl/apache.key`: define o caminho e o nome do arquivo onde a chave privada gerada será armazenada;

* `-out /etc/apache2/ssl/apache.crt`: define o caminho e o nome do arquivo onde o certificado autoassinado gerado será armazenado.

Após você digitar o comando, será solicitado algumas informações para o certificado, preencha-os. Veja como preenchi o meu:

![Screencast from 01-05-2023 10_39_55](https://user-images.githubusercontent.com/104470835/235459996-6bea397f-4cf7-425d-878b-32f12e44aee8.gif)

4. Por fim, vamos adicionar o caminho das chaves geradas no arquivo de configuração do módulo SSL/TLS do Apache2 chamado de `default-ssl.conf`, esse arquivo fica no diretório `/etc/apache2/sites-available/`. Se você têm dúvidas como abrir o arquivo use o comando `sudo nano /etc/apache2/sites-available/default-ssl.conf`. Após abrir, encontre essas diretivas:

![image](https://user-images.githubusercontent.com/104470835/235463932-1d6d09e9-5d83-451d-bede-f217b555eaa0.png)

E altere para os caminhos para as chaves que criamos no passo 3, veja o resultado:

![image](https://user-images.githubusercontent.com/104470835/235473308-c5c69c69-a3ab-4395-bb44-3bbbd460b7f7.png)

E também vamos alterar o caminho do DocumentRoot para o nosso site como na imagem abaixo: 

![image](https://user-images.githubusercontent.com/104470835/235505344-222c00a4-4ece-4e37-aef0-8491365520f3.png)

Salve o arquivo, habilite a configuração com `a2ensite default-ssl.conf` e reinicie o apache com `/etc/init.d/apache2 restart`. Lembrando que essa é uma das diversas formas de configurar o SSL para nossa página, poderíamos configurar no próprio arquivo do nosso site ou poderíamos habilitar o SSL para todos os sites de uma vez, mas optei pela opção mais simples por fins demonstrativos.

***CUIDADO AO ADICIONAR OS CAMINHOS E NÃO COMPARTILHE SUA CHAVE PRIVADA(.key)***

## Teste do Servidor

Para o teste, vou abrir o site no navegador usando o protocolo `https` e em seguida irei ver o certificado que geramos, observe o gif abaixo:

![Screencast from 01-05-2023 15_33_31](https://user-images.githubusercontent.com/104470835/235507704-dc083dce-d75a-4e3e-b4dc-f437df11843c.gif)

*OBS: O site está diferente porque mudei, devido a imagem de fundo não está mais disponível*

Perceba que os dados que informamos no passo 3 da configuração estão ali, agora temos um certificado no nosso site. Lembre-se que para ser um certificado válido para o público em geral, deve-se procurar uma autoridade certificadora (CA) para que ela possa autentificar como legal.

Muito obrigado se você leu até aqui, até o próximo!



