# Bootcamp-Cyber
Exercícios relativos a Cibersegurança realizados dentro do Bootcamp Santander em parceria a empresa DIO.

Estarei colocando abaixo um passo a passo de como usar a ferramenta HYDRA para qubrar a senha de algum usuário na web. Este passo a passo poderá ser seguido por qualquer pessoa, desde que a mesma tenha acesso ao mesmo laboratório da hackviser, ou consiga explorar uma vulnerabilidade parecida ao do laboratório em questão.

Primeiramente conectei-me a vpn para ter acesso as máquinas vulneráveis do laboratório. A Vpn encontra-se disponível no próprio site da Hackviser.

<img width="1363" height="369" alt="2025-11-04_16-11" src="https://github.com/user-attachments/assets/1a0d8b54-08b8-402e-9796-aa4abef5fc85" />

Como o enunciado do laboratório diz, devemos quebrar a senha do usuário admin. Neste caso já sabemos que o usuário é admin e apenas precisamos da senha dele. Eu optei por usar o Hydra. Vamos seguir com o passo a passo:
1º --> Peguei o site disponível e com a VPN consegui fazer o acesso ao mesmo. Damos de cara com uma página de login:
<img width="1402" height="880" alt="2025-11-04_16-00" src="https://github.com/user-attachments/assets/c41462c7-53bb-4e22-ae6a-43e7cf80c019" />
A primeira coisa que vamos fazer neste caso é observar como se comporta a página de login. Neste caso tentei admin e uma senha aleatória, mas nestes casos é interessante procurar pela aplicação que está rodando no site e tentar credenciais padrões. Aqui apenas tentei teste123.
<img width="736" height="686" alt="2025-11-04_16-00_1" src="https://github.com/user-attachments/assets/bddc2c2f-8c6c-4ef3-989a-a0ea5dce226d" />
Observe que o erro veio e apresentou como resposta: "Wrong username or password" como a seta vermelha indica. Observe que os parâmetros da barra de endereço não mudaram, indicando possibilidade do site estar utilizando o método POST. Para ter certeza, inspecionei a página web e confirmamos a hipótese.

<img width="729" height="172" alt="2025-11-04_16-01" src="https://github.com/user-attachments/assets/c9362467-e736-4001-ae7a-28112235c195" />

2º --> Por se tratar de um método Post, utilizei o BurpSuite para interceptar chamados entre cliente e servidor. Esses dados serão úteis para que eu possa usar o Hydra posteriormente.
<img width="1008" height="325" alt="2025-11-04_16-02" src="https://github.com/user-attachments/assets/bd7f6155-2365-49f4-aff4-8e65e99fc7cf" />

3º --> Dentro do BurpSuite, depois de interceptado as requisiçoes cliente servidor utilizando o usuário admin e uma senha aleatória, encontramos os dados a seguir:
<img width="768" height="584" alt="2025-11-04_16-42" src="https://github.com/user-attachments/assets/0211f9d9-1d5c-4734-9c3e-234a05b6454e" />

4º --> Agora vamos montar toda nossa estrutura com essas informações anteriores encontradas com o BurpSuite e montar o nosso ataque de credencial utilizando o Hydra.
<img width="806" height="169" alt="2025-11-04_16-18_1" src="https://github.com/user-attachments/assets/75813d87-fd8a-4926-88ac-9ac9e0876394" />

Usamos então o Hydra desta forma:
hydra -l admin -P /usr/share/wordlists/rockyou.txt novel-microbe.europe1.hackviser.space https-post-form "/login.php:username=^USER^&password=^PASS^:password" -I -F -V
--> Neste caso usamos -l minúsculo pois sabe-se que o usuário é admin
--> -P para buscar a chave dentro da wordlist
--> o site com a página de login em questão
--> https-post-form pois se trata de um site https usando o método POST
--> O diretório do domínio de ataque: /login
--> O método post envia usuário e senha conforme apresentado. Neste caso o PASS indica onde vai ser realizado a quebra de senha com a wordlist.
--> Caso a senha testada não apresente password de Wrong username or password, ele mostrará a senha!

5º --> Agora rodando o comando obtemos o resultado do ataque e chegamos a senha final: "superman"

<img width="808" height="600" alt="2025-11-04_16-18" src="https://github.com/user-attachments/assets/8b68fc5a-4aba-4ad7-a9d7-097c823ad762" />

Com o resultado vamos testar no site o login com as credenciais admin:superman.

<img width="1514" height="795" alt="2025-11-04_16-19" src="https://github.com/user-attachments/assets/d9a7ffd9-5ca0-4d01-983e-3d1d18f4c2b9" />


Chegamos ao final do laboratório. Usuário admin logado e agora temos as informações dele! Fica o aprendizado de sempre buscar o uso de senhas fortes e que estas não estejas vazadas em wordlists que possam fazer a quebra da autenticação, expondo nossos dados ou de clientes em banco de dados de servidores. 
Até Breve!





