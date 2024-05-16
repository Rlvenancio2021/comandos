# Hierarquia de Diretórios no Linux

## Diretório / ou root 

Diretório raiz do sistema de arquivos;

## Diretório /bin

Contêm os comandos que podem ser utilizados pelos usuários e pelo administrador do sistema. São requeridos no modo monousuário ou de manutenção (single-user mode) e também podem conter comandos que são utilizados indiretamente por alguns scripts. Nele ficam os arquivos executáveis, tais como: cat, chgrp, chmod, chown, cp, date, dd, df, dmesg, echo, hostname, kill, ln, more, mount, mv, ps, pwd, rm, rmdir, sed, su, umount e uname;

## Diretório /boot

Arquivos estáticos necessários à carga do sistema. É onde fica localizada a imagem do Kernel, initramfs e alguns arquivos do Grub. Este diretório contém tudo que é necessário para carregar o sistema, exceto os arquivos de configuração e o gerenciador de boot. O /boot inclui o setor master de carga do sistema (master boot sectors) e arquivos de mapa de setor.

## Diretório /dev

Abstração do Kernel onde ficam os arquivos para acesso dos dispositivos do sistema, como discos, cd-roms, pendrives, portas seriais, terminais, etc. Os arquivos são descritores que facilitam o acesso aos dispositivos. Este diretório é um pseudo-filesystem, e não existe no disco. Seu conteúdo, por exemplo, tem descritores de dispositivos como /dev/sda, /dev/cdrom, etc.

## Diretório /etc

Arquivos necessários à configuração do sistema. São necessários para a carga do sistema operacional. Ele possui arquivos importantes tais como: fstab, exports, passwd, shadow, group, hosts, hosts.allow, hosts.deny, inittab, ld.so.conf, mtab, profile, services, etc. Nele também residem os arquivos de configuração das interfaces de rede.

Tipicamente /etc possui dois subdiretórios:

**/etc/X11**: Arquivos de configuração para a interface gráfica do Linux (X Window System);

**/etc/skel**: Esqueletos da configuração usuários. No diretório /etc/skel ficam localizados os arquivos de “modelo” para os usuários. O conteúdo deste diretório é replicado para o diretório home dos novos usuários quando são criados no sistema.


## Diretório /home

Geralmente é neste diretório onde ficam os diretórios home dos usuários. Nestes diretórios localizam-se scripts de carga de perfil e do shell bash de cada usuário;

## Diretório /lib

Arquivos de bibliotecas essenciais ao sistema, utilizadas pelos programas em /bin e módulos do Kernel. É comum existir um diretório /lib[arquitetura]. Nos processadores de 64 bits, existe o diretório /lib64. Nos processadores de 32 bits, deve existir um diretório /lib32;

## Diretório /mnt

Diretório vazio utilizado como ponto de montagem de dispositivos na máquina. Usado pelo administrador para montar discos;

## Diretório /media

Diretório vazio utilizado como ponto de montagem de dispositivos na máquina, tais como cdrom, dvd, pendrives, etc;

## Diretório /proc

Informações do Kernel e dos processos. É um pseudo-filesystem e não existe realmente no disco. Neste diretório ficam as abstrações de descritores de tudo quanto há no Kernel do sistema. É possível não só ler os dados, bem como fazer alterações no comportamento do Kernel alterando o conteúdo de arquivos em /proc. Fazendo uma abstração do Windows, este diretório seria o “registro” do sistema;

## Diretório /opt

Neste diretório ficam instalados os aplicativos que não são da distribuição do Linux, como por exemplo, banco de dados de terceiros, software de vetores Cad, etc;

## Diretório /root

Diretório home do superusuário root. Dependendo da distribuição pode estar presente ou não;

## Diretório /run

Este diretório contém informações do sistema desde a última carga. Os arquivos deste diretório são apagados ou zerados no início do processo de boot. Arquivos como aqueles que indicam o PID do processo em execução devem residir neste diretório;

## Diretório /sbin

Arquivos essenciais ao sistema, como aplicativos e utilitários para a administração da máquina. Normalmente só o superusuário tem acesso a estes arquivos, tais como: fdiskm fsck, ifconfig, init, mkfs, mkswap, route, reboot, swapon e swapoff;

## Diretório /tmp

Diretório de arquivos temporários. Em algumas distribuições este diretório é montado em memória. Recomenda-se que seu conteúdo seja apagado de tempos em tempos ou a cada reboot;

## Diretório /usr

Arquivos pertencentes aos usuários e a segunda maior hierarquia de diretórios no Linux. Seu conteúdo pode ser distribuído via rede para diversos sistemas Linux da mesma distribuição sem problema algum. Ele tem alguns subdiretórios a saber:

**/usr/bin**:  Ferramentas e comandos auxiliares de usuários, bem como interpretadores de programação, como o perl, python, etc;

**/usr/include**:  Cabeçalhos e bibliotecas da linguagem C;

**/usr/lib e /usr/lib64**: bibliotecas de aplicações de usuários;

**/usr/libexec**:  binários que não são executados normalmente por usuários;

**/usr/local**:  hierarquia de diretórios para instalação de aplicativos locais no sistema. Possui vários subdiretórios como bin, etc, include, lib, man, sbin, share e src;

**/usr/sbin**:  contém ferramentas não essenciais, mas exclusivas da administração do sistema.

**/usr/share**:  arquivos de somente leitura de arquitetura independente. São arquivos que podem ser compartilhados entre distribuições de Linux iguais, independente da arquitetura. O diretório man por exemplo é onde residem os manuais dos comandos e fica em /usr/share.

**/usr/src**:  pode conter arquivos de código fonte de programas;

## Diretório /var

Diretório onde são guardadas informações variáveis sobre o sistema, como arquivos de log, arquivos de e-mail etc. Possui subdiretórios importantes, a saber:

**/var/cache**:  mantém informações de cache das aplicações como cálculos, etc;

**/var/lib**:   mantém informações de estado das aplicações;

**/var/lock**:  mantém arquivos de trancamento que retém dispositivos para uso exclusivo de alguma determinada aplicação;

**/var/log**:   mantém os arquivos de log do sistema, tais como messages, lastlog e wtmp.

**/var/mail**:   mantém os diretórios de contas de email dos usuários;

**/var/opt**:   mantém os arquivos variáveis das aplicações;

**/var/run**:  a funcionalidade deste diretório foi movida para o /run.

**/var/spool**:  mantém dados de processos que mantém filas de arquivos, tais impressão e saída de email;

**/var/tmp**:  mantém os arquivos temporários das aplicações que precisem ser preservados entre o reboot do sistema.
