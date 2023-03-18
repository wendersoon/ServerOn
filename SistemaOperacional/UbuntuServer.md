# Instalação e Configuração

Irei usar como sistema operacional do meu servidor o **Ubuntu Server 22.04.2** disponível neste [link](https://ubuntu.com/download/server), ele
será instalado no software de virtualização **Oracle VM VirtualBox versão Versão 7.0.4**. Os passos são os seguintes:

1. Baixar arquivo ISO. Se caso você estiver fazendo isso pra estudo, sugiro colocar a ISO na mesma pasta onde será instalado o sistema operacional (fica mais fácil pra apagar depois hehe)
![image](https://user-images.githubusercontent.com/104470835/226125737-7faff83b-d317-4cfd-8d30-5f8b9749005a.png)<br>
No meu caso, baixei o arquivo na pasta Ubuntu que deixei no meu Desktop<br>
2. Baixar o software de virtualização: você pode escolher qualquer um mas nesse tutorial usaremos o Oracle VM VirtualBox para exemplificação<br>
3. Iniciar a configuração da máquina virtual: você vai em *novo* e adiciona as informações do nome, a pasta que deseja instalar e a imagem ISO e em seguida clica em *Próximo*<br>
![image](https://user-images.githubusercontent.com/104470835/226125983-5cf21a60-f345-4738-a69b-28cec4dc775d.png)
![image](https://user-images.githubusercontent.com/104470835/226125995-77c6bb2c-cda1-48b7-a416-64be50bcde31.png)<br>
4. Na próxima tela pode deixar do jeito padrão mesmo e clicar em *próximo*
![image](https://user-images.githubusercontent.com/104470835/226126064-a81f049d-77d6-4e59-a17d-e569fa6e4a3e.png)
5. Nessa etapa você define o quanto de RAM irá ficar disponível para sua máquina virtual. É uma recomendação que esse valor nunca passe da metade da quantidade de memória RAM que sua máquina real possui. No meu caso, como tenho 20GB de RAM colocarei 4GB para o servidor sendo muito mais do que necessário para o que é proposto nesse projeto, depois de feito isso é só clicar em *próximo*.
![image](https://user-images.githubusercontent.com/104470835/226126464-dac6f946-cb6e-4ed4-b255-cfa14d7cf0c6.png)
6. Agora devemos escolher o tamanho do disco disponível para a máquina virtual. Aqui não precisamos de muito, então coloquei apenas 10GB de disco para o servidor, após isso é só clicar em próximo
![image](https://user-images.githubusercontent.com/104470835/226126609-49842479-5087-4474-8111-4bd06100b1f7.png)
7. Nesta parte será apresentado um resumo das configurações selecionadas no decorrer do processo, verifique se está tudo correto. Se estiver tudo ok, é só clica em finalizar e aguardar o processo de instalação do sistema operacional, lembre-se que até aqui foi apenas a configuração da máquina virtual
![image](https://user-images.githubusercontent.com/104470835/226126774-e775cc17-5503-4539-8b1f-2fe40d90844a.png)
8. Aguarde carregar o processo de inicialização. Irá aparecer uma janela semelhante a imagem abaixo, selecione o idioma que deseja usar e tecle *enter*. No meu caso, vou selecionar o português mesmo.
![image](https://user-images.githubusercontent.com/104470835/226129171-34932cba-7622-4588-9702-c55e7d92a94a.png)
9.  Nesta etapa você seleciona o layout do seu teclado, se estiver em dúvida pode usar a opção *identify keyboard* que irá lhe ajudar. Terminado é só ir em concluído e teclar *enter*.
![image](https://user-images.githubusercontent.com/104470835/226129505-82aed1ee-c603-4935-b2ea-fb79f41ac248.png)
10. Aqui você seleciona a versão que deseja, no meu caso será o Ubuntu Server minimized.
![image](https://user-images.githubusercontent.com/104470835/226129649-8f9630b8-e524-47dd-b84e-e29b5593c860.png)
11. Aqui você pode definir a interface de rede que irá utilizar, deixei do mesmo modo que o sistema apresentou
![image](https://user-images.githubusercontent.com/104470835/226129769-a85c6503-98d0-4443-9396-10f7e6362e96.png)
12. As próximas etapas são: *Configure o proxy*, *Configure Ubuntu Arquive Mirror* e *Guided storage configuration* os quais você pode pular se essas configurações não o interessam por agora. No caso desse projeto, não precisamos nos atentar a elas
13. Ao final será exibido um resumo de todas as configurações que selecionamos no caminho, confira se está tudo correto. Se estiver ok, é só selecionar concluído. Irá aparecer um aviso, mas é só selecionar *continue* e processo de instalação vai iniciar
![image](https://user-images.githubusercontent.com/104470835/226130009-13fb8236-1c5b-4a1e-a756-57ce90a9a93f.png)
14. Nessa etapa você irá criar o perfil de root do servidor. Após preencher as informações, selecione concluído. Irá aparecer a opção de upgrade para o Ubuntu Pro, basta selecionar *skip for now* e seguir adiante.
![image](https://user-images.githubusercontent.com/104470835/226130763-c7a15a0f-e7e4-4fd8-9cc7-a7869db1e131.png)
15. Aqui se você quiser pode instalar o SSH. Porém optei por não instalar agora porque farei isso nas etapas seguintes de modo mais detalhado.<br>
![image](https://user-images.githubusercontent.com/104470835/226131143-5650ae61-3aef-431f-8bf8-f6a0c41ce5a7.png)
16. Também vou pular a janela seguinte porque essas instalações não são necessárias para esse projeto.
![image](https://user-images.githubusercontent.com/104470835/226131245-2e26248b-8afe-4817-b6a1-1eb416597def.png)
17. Aguarde a instalação 

