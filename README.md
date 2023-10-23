Entendendo as decisões estruturais e de arquitetura do projeto: 

Requisitos para rodar o projeto:  

Virtual Box https://www.virtualbox.org/wiki/Download_Old_Builds_6_1 

Oracle https://yum.oracle.com/oracle-linux-downloads.html?source=:ow:o:p:nav:121620LinuxHero&intcmp=:ow:o:p:nav:121620LinuxHero 

 
Requisitos para instalação do Linux: 

o Virtual Box na sua maquina. 

Ter 4 gigas de memoria ram ( Recomendado) 

Baixar a iso Oracle Linux 8 

 

Configurações da VM: 

 Criar uma Nova Máquina Virtual: 

Abra o VirtualBox após a instalação. 

Clique no botão "Novo" na barra de ferramentas. 

Siga as etapas: 

Nomeie sua máquina virtual  no meu caso foi : Oracle Linux e a versão no meu caso foi: Oracle (64-bit). 

Defina a quantidade de memória RAM (RAM) Coloquei 4gb 

Criar um Disco Rígido Virtual: 

Selecione "Criar um disco rígido virtual agora" e clique em "Criar". 

Escolha o tipo de disco rígido que você deseja criar. O padrão é "VDI (VirtualBox Disk Image)" 

Escolha se deseja um disco rígido dinamicamente alocado (o tamanho do disco aumentará à medida que você adiciona dados) ou de tamanho fixo (o tamanho do disco é definido no momento da criação). 

Escolha o local onde deseja armazenar o disco rígido virtual e o tamanho do disco. 

Clique em "Criar" para criar o disco rígido virtua 

 

Configurar as Configurações da Máquina Virtual: 

No VirtualBox, selecione a máquina virtual recém-criada e clique em "Configurações". 

Nas configurações,  

boot order, 

a placa de rede 

compartilhamento de pastas, etc.  

Deixei algumas  para uma configurações como  padrão. 

Certifique-se de que a unidade de CD/DVD está configurada para o seu meio de instalação com sua iso OracleLinux. 

Iniciar a Máquina Virtual: 

Na janela principal do VirtualBox, selecione a máquina virtual e clique no botão "Iniciar". 

A máquina virtual iniciará e, se estiver usando um arquivo ISO, você será solicitado a selecionar o arquivo de instalação do sistema operacional. 

Configurações da VM: 

SO: 64-bit 

Memoria principal: 1024Mb 

Aceleração: Padrão 

Tela: configuração padrão 

Armazenamento: Recomendado pelo sistema 

Rede: Intel PRO/1000 MT Desktop (Modo Brigde) 

 

Configurando o IP fixo: 

Antes de começar atualize os pacotes pois alguns serão necessarios ao decorrer do processo. 

Sudo yum update 

Atualizado localize sua pasta de redes, para isso vc pode usar o comando ls ou procurar diretamente no seu /etc 

Utilize o comando: 

sudo vi /etc/sysconfig/network-scripts/ifcfg-enp0s3 

Edite e acrecente os seguintes campos: 

BOOTPROTO =static e adicione as configurações  

IPADDR= 192.168.1.100/ 192.168.1.101 

NETMASK=255.255.255.0 

GATEWAY=192.168.1.1 

PEERROUTES=yes 

PEERDNS=yes 

(Os dois IP è uma para cada maquina) 

Repita o mesmo processo na segunda maquina. 

Para garantir que seu ip está correto utilize o comando : 

Ip addr 

Para localizar seu ip fixo que está localizado no segundo campo do resultado do comando e de um ping para verificar se os pacotes estão sendo recebidos de forma correta 

Ping 192.168.1.105

Ping 192.168.1.101 

 

Clonando a VM: 

Para o proximo passo entre no VirtualBox, clique em cima da VM que você pretende clonar, vá em maquinas e escolha clonar. 

 

Depois reconfigure o IP para ter um servidor e um cliente. 

 

Alteração do Hostname: 

Fiz a alteração do nome da VM 1 para server utilizando os comandos abaixo: 

sudo vi /etc/hostname 

sudo hostnamectl set-hostname server 

vi /etc/sysconfig/network 

Logo apos a mudança não conseguir me conectar a rede para baixar pacotes dava o seguinte erro: 

Error: curl#6 – “Could not resolve host: mirrorlist.centos.org; Unknown error” 

Resolvi adicionando um novo IP de DNS para a resolução dos nome, utilizei os seguintes comandos: 

cat /etc/resolv.config 

Ping 8.8.8.8 

Vi /etc/resolv.config 

Adicionei: nameserver 8.8.8.8 

Reboot 

Instalação do mariaDB-Server: 

Para a instalação do mariadb eu utlizei esses comandos  

Yum install php –y 

Yum install epel-reload -y 

sudo yum install mariadb-server 

sudo systemctl start mariadb 

sudo systemctl enable mariadb 

Para as configurações de segurança utilizei o seguinte comando: 

sudo mysql_secure_installation 

E para acessar depois : 

mysql -u root -p 

A parte mais dificil foi instalar o phpmyadmin, não conseguir entrar em nenhuma instalação, tive varios erros diferente, em alguns dela consguia entrar no index.php, e na segunda vez que fiz do zero deu erro 403 e ultima erro 404 

 

Instalação do Wordpress: 

Na instalação do wordpress na primeira vez deu tudo certo, eu seguir os senguintes passo: 

sudo yum install php php-mysql php-gd php-xml php-mbstring 

sudo systemctl start httpd 

sudo systemctl enable httpd 

sudo systemctl start mysqld 

sudo systemctl enable mysqld 

cd /var/www/html 

sudo wget https://wordpress.org/latest.tar.gz 

sudo tar -xzvf latest.tar.gz 

sudo mv wordpress nomedoseusite 

sudo chown -R apache:apache wordpress 

cd wordpress 

sudo cp wp-config-sample.php wp-config.php 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 



 

 
