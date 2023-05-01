## Certificação SSL

<div align = "center">

![selossl](https://user-images.githubusercontent.com/104470835/235451273-7f503c14-db97-49ea-a806-b975355833fa.png)

</div>

SSL (*Secure Sockets Layer*) é um protocolo de segurança que permite a criptografia de comunicações de rede entre um cliente e um servidor. Uma certificação SSL é um certificado digital que é emitido por uma autoridade de certificação (CA) confiável e serve para verificar a identidade do proprietário do site e garantir que as informações transmitidas estejam seguras. 

Quando um site possui um certificado SSL, ele utiliza uma chave criptográfica para criptografar as informações que são enviadas do cliente para o servidor e vice-versa, tornando praticamente impossível que um terceiro intercepte e leia essas informações. Além disso, o certificado SSL também exibe um ícone de cadeado verde na barra de endereço do navegador, o que indica que o site é confiável e seguro.

Já uma **certificação privada** (ou certificado de servidor autoassinado) é um tipo de certificado digital usado para proteger as comunicações em uma rede. Diferente dos certificados emitidos por autoridades de certificação confiáveis, uma certificação privada é emitida pelo próprio proprietário do servidor, sem a intervenção de uma terceira parte confiável. Se você quiser saber mais detalhes sobre as certificações, acesse esse [artigo do Serasa](https://serasa.certificadodigital.com.br/blog/mercado/o-que-e-o-certificado-digital-ssl-e-como-ele-pode-proteger-informacoes/) que detalha muito bem esse tema.

![OpenSSL_logo svg](https://user-images.githubusercontent.com/104470835/235452740-38f52a3d-28cf-4036-b8ef-54306be97d06.png)

E nessa parte do projeto iremos gerar um certificado privado para a página que hospedamos no nosso servidor (acredito que você tenha passado pelos dois tópicos anteriores) utilizando o OpenSSL que é um utilitário de criptografia de código aberto que fornece implementações de protocolos de segurança, como SSL/TLS (Secure Socket Layer/Transport Layer Security), PKI (Public Key Infrastructure), criptografia de chave simétrica e assimétrica, hash e outros algoritmos de criptografia. Se você quiser saber mais sobre esse projeto, acesse a [página oficial](https://www.openssl.org/).

***Todos os comandos serão em modo root***

## Instalação

1 . Para instalar você deve usar o comando `sudo apt-get install openssl`. A instalação não cria nenhum diretório específico mas pode criar alguns arquivos de configuração em diretórios existentes como, por exemplo, `etc/ssl/` que é onde geralmente são armazenados os arquivos de certificado e chave. Então, por isso, vamos dá uma olhada nesse diretório:

![image](https://user-images.githubusercontent.com/104470835/235455061-a707a3d9-1393-485b-8297-39e1970f6a43.png)

Vejamos um resumo:

* `certs`: é o diretório que é geralmente usado para armazenar os arquivos de certificado, que são usados para autenticar a identidade de um servidor ou cliente em uma conexão SSL/TLS;
* `openssl.cnf`: é o arquivo de configuração padrão do OpenSSL, que é usado para definir as opções de configuração do utilitário OpenSSL.;
* `private`: é geralmente usado para armazenar os arquivos de chave privada, que são usados para criptografar e descriptografar os dados transmitidos em uma conexão SSL/TLS.

Não vamos trabalhar com esse diretório.

2. O segundo passo é habilitar o módulo SSL no Apache2 e para isso use o comando `sudo a2enmod ssl`. Em seguida, reinicie o serviço com `sudo /etc/init.d/apache2 restart`.

3. Vamos criar um novo diretório para guardar o certificado privado que vamos gerar. Para criar o diretório, utilize o comando `sudo mkdir /etc/apache2/ssl`. E para gerar o certificado utilize o comando `sudo openssl req -x509 -nodes -days 365 -newkeyrsa:2048 -keyout /etc/apache2/ssl/apache.key -out /etc/apache2/ssl/apache.crt`

