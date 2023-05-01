## Certificação SSL

<div align = "center">

![selossl](https://user-images.githubusercontent.com/104470835/235451273-7f503c14-db97-49ea-a806-b975355833fa.png)

</div>

SSL (*Secure Sockets Layer*) é um protocolo de segurança que permite a criptografia de comunicações de rede entre um cliente e um servidor. Uma certificação SSL é um certificado digital que é emitido por uma autoridade de certificação (CA) confiável e serve para verificar a identidade do proprietário do site e garantir que as informações transmitidas estejam seguras. 

Quando um site possui um certificado SSL, ele utiliza uma chave criptográfica para criptografar as informações que são enviadas do cliente para o servidor e vice-versa, tornando praticamente impossível que um terceiro intercepte e leia essas informações. Além disso, o certificado SSL também exibe um ícone de cadeado verde na barra de endereço do navegador, o que indica que o site é confiável e seguro.

Já uma **certificação privada** (ou certificado de servidor autoassinado) é um tipo de certificado digital usado para proteger as comunicações em uma rede. Diferente dos certificados emitidos por autoridades de certificação confiáveis, uma certificação privada é emitida pelo próprio proprietário do servidor, sem a intervenção de uma terceira parte confiável. Se você quiser saber mais detalhes sobre as certificações, acesse esse [artigo do Serasa](https://serasa.certificadodigital.com.br/blog/mercado/o-que-e-o-certificado-digital-ssl-e-como-ele-pode-proteger-informacoes/) que detalha muito bem esse tema.

![OpenSSL_logo svg](https://user-images.githubusercontent.com/104470835/235452740-38f52a3d-28cf-4036-b8ef-54306be97d06.png)

E nessa parte do projeto iremos gerar um certificado privado utilizando o OpenSSL que é um utilitário de criptografia de código aberto que fornece implementações de protocolos de segurança, como SSL/TLS (Secure Socket Layer/Transport Layer Security), PKI (Public Key Infrastructure), criptografia de chave simétrica e assimétrica, hash e outros algoritmos de criptografia. Se você quiser saber mais sobre esse projeto, acesse a [página oficial](https://www.openssl.org/).

