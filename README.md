# Tutorial_instala-o_COAWST
Tutorial em Português instalação do modelo acoplado COAWST












MANUAL INSTALAÇÃO DO COAWST




LUIS FELIPE MENDONÇA

LFELIPEM@MSN.COM

















Versão 2.0
Setembro de 2022
INTRODUÇÃO

O objetivo deste tutorial é ajudar novos usuários a instalar o modelo acoplado COAWST (desenvolvido por John Warner - USGS). A instalação garantirá que usuário possa rodar os casos teste e ter certeza que tudo está correto com a configuração do modelo.

Gostaria de enfatizar que o objetivo deste tutorial é ajudar de forma simples e objetiva, complementando o manual do COAWST, para que novos usuários brasileiros consigam acessar esta ferramenta numérica sensacional.

Inicialmente tenho duas considerações pessoais a fazer:

1 - Modelos baseados em linguagem FORTRAN, como os utilizados no COAWST (ROMS+WRF+SWAN), não são softwares de fácil instalação como estamos acostumados a utilizar diariamente em nossas máquinas. Compreender estes pacotes e sua linguagem demanda dedicação e seu sucesso está diretamente associado ao tempo que demandamos em cima dele. Logo, quanto mais você trabalhar com ele, maior será o seu conhecimento e domínio sobre o mesmo. Particularmente minha dica é que você abra uma cerveja e vá com calma.

2 – Por se tratar de um sistema “novo”, seus criadores ainda não o dominam completamente. Aversões do COAWST são atualizadas com muita frequência e as correções de erros são praticamente constantes. Pense que trabalhar com apenas um modelo é difícil, trabalhar com 3 é ainda mais. Não tenha medo nem vergonha de pedir ajuda, nem de procurar soluções no google. Os modelos ROMS e WRF são amplamente difundidos e possuem um suporte on line muito amplo e funcional.


Dica 1: Se possível comece com o Linux zerado (recém instalado), siga o passo a passo que está descrito no tutorial abaixo e deve dar tudo certo.

Lembre-se que as etapas são praticamente todas desenvolvidas via terminal do Linux.

Dica 2: Durante a compilação do modelo, passarão várias linhas de comando rapidamente pela tela. Minha dica é subir o Scroll do mouse e acompanhar vagarosamente os comandos executados a procura de erros. Assim é possível solucionar os problemas de configuração e compilação. Se você deixar os textos passarem rapidamente sem acompanhá-los, você não conseguirá seguir a evolução dos processos e consequentemente não consegui achar os erros.

Dica 3: Apareceu um erro??? Lembre-se: GOOGLE É MEU PASTOR E NADA ME

FALTARÁ.

Bom, chega de papo e vamos começar. 
1.	INSTALANDO OS COMPONENTES BÁSICOS

Crie uma pasta /Programas no seu home, é lá que baixaremos e descompactaremos todos os programas. Execute o comando abaixo com o nome de origem do seu home.

mkdir /home/luis/Programas

Para que o COAWST funcione corretamente precisamos instalar os seguinte pacotes: subversion, gfortran, g++, M4, make (já vem no ubuntu), perl (também já vem instalado), zlib, hdf5, netcdf para C, netcdf para fortran e finalmente baixar o COAWST.

Começamos com o básico, abrindo o Terminal e instalado o subversion, gfortra, g++ e o M4.

sudo apt-get install subversion

sudo apt-get install m4

# Por questões práticas as versões do GCC, G++ e gfotran 9 são as mais estáveis para mim e não constuma dar problemas. Por esse motivos instalaremos está versão.

sudo aptitude install gcc-9 g++-9 gfortran-9

# Deixando gcc-9 como default
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-9 10
sudo update-alternatives --install /usr/bin/cc cc /usr/bin/gcc 30
sudo update-alternatives --set cc /usr/bin/gcc

# Deixando g++-9 como default
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-9 10
sudo update-alternatives --install /usr/bin/c++ c++ /usr/bin/g++ 30
sudo update-alternatives --set c++ /usr/bin/g++

# Deixando gfortran-9 como default
sudo update-alternatives --install /usr/bin/gfortran gfortran /usr/bin/gfortran-9 40 
sudo update-alternatives --config gfortran

# o make deve ser 3.8 ou mais recente
make -v

# verifique se o perl está instalado
perl –v

Instalando a biblioteca zlib:

wget http://zlib.net/fossils/zlib-1.2.11.tar.gz

tar -xvzf zlib-1.2.11.tar.gz

cd zlib-1.2.11

./configure --prefix=/usr/local/

make

sudo make install

Instalando a biblioteca hdf5:

wget https://support.hdfgroup.org/ftp/HDF5/releases/hdf5-1.14/hdf5-1.14.0/src/hdf5-1.14.0.tar.gz

tar xzf hdf5-1.14.0.tar.gz

cd hdf5-1.14.0/

./configure --with-zlib=/usr/local --prefix=/usr/local

make check install

sudo make install


Agora instale o netcdf para C e depois para fortran:

wget https://downloads.unidata.ucar.edu/netcdf-c/4.9.0/netcdf-c-4.9.0.tar.gz

tar xzf netcdf-c-4.9.0.tar.gz

cd netcdf*

./configure --prefix=/usr/local

make check

sudo make install

Instalação netcdf Fortran

wget https://downloads.unidata.ucar.edu/netcdf-fortran/4.6.0/netcdf-fortran-4.6.0.tar.gz

tar xzf netcdf-fortran-4.6.0.tar.gz

cd netcdf-fortran-4.6.0/

./configure --prefix=/usr/local

sudo make install

Agora vamos instalar o MPI (Message Passing Interface) para poder usar o COAWST em paralelo. Eu escolhi utilizar o Open-MPI.

wget https://download.open-mpi.org/release/open-mpi/v4.1/openmpi-4.1.4.tar.gz

tar xvzf openmpi-4.1.4.tar.gz

cd openmpi-4.1.4

./configure --prefix=/usr/local

sudo make install

Instalação do Jasper

wget http://www.ece.uvic.ca/~mdadams/jasper/software/jasper-1.900.1.zip

unzip jasper-1.900.1.zip

cd jasper-1.900.1.zip

./configure --prefix=/usr/local

make

sudo make install

Se tudo deu certo até agora, vamos baixar a última versão do COAWST usando o subversion, mas para isso você precisa de uma conta no COAWST. Para adquiri-la mande um e-mail para (jcwarner@usgs.gov). Ao usar o svn será necessário usar seu usuário e senha. Escolha um local a seu gosto para instalar o COAWST. Minha dica é um ambiente que não necessite ROOT e seja de fácil acesso (ex: /home/luis/COAWST).

1	Download COAWST			
				
2	svn	checkout	--username	seuusername
	https://coawstmodel.sourcerepo.com/coawstmodel/COAWST	
		

Os pré-requitos básicos estão feitos, passamos a configuração do COAWST. Está etapa é apenas um acréscimo do que já foi descrito no manual do COAWST.

Gostaria de acrescentar que possuo algumas manias e atos que podem ser simplificados e feitos de maneira diferente, mas que para o correto funcionamento do modelo acho interessante serem seguidos, pelo menos em primeira estância.

 
2 – CONFIGURAÇÃO DO COAWST

Está etapa consiste em configurar o arquivo (.bashrc). Para os iniciantes em Linux o arquivo .bashrc é responsável por mostrar ao sistema raiz onde as pastas com os arquivos e bibliotecas serão chamados. O arquivo .bashrc encontra-se na pasta pessoal do usuário do Linux (Ex: /home/luis/.bashrc). Abrindo o arquivo no editor de texto do Linux (comando gedit .bashrc na sua pasta pessoal) deve abrir o arquivo de interesse. Desça ao fim do documento de texto e acrescente as seguintes linhas de comando:

export MCT_INCDIR=/home/luis/COAWST/Lib/MCT/include

export MCT_LIBDIR=/home/luis/COAWST/Lib/MCT/lib 

export NETCDF_INCDIR=/usr/local/include

export NETCDF_LIBDIR=/usr/local/lib 

export PATH=$PATH:/usr/local/bin 

export NETCDF=/usr/local

export LD_LIBRARY_PATH="/usr/local/lib:/usr/local/lib/:${LD_LIBRARY_PATH}" 

export INCLUDE="/usr/local/include:/usr/local/include/:${INCLUDE}"

export JASPERLIB=/usr/local/lib

export JASPERINC=/usr/local/include

*****Lembre-se de mudar o nome da pais raiz, para a sua. (/home/luis/...)

Feche o editor de texto.

Para atualizar o .bashrc execute o seguinte comando:

source .bashrc

3 – CONFIGURAÇÃO DO MCT

Esta etapa é idêntica a do manual do COAWST, porém o Dr. Warner indica que copiemos as pastas mct/lib e mct/include para a pasta /usr/local. Eu não acho necessário este procedimento, então iremos direto ao ponto. Entre na pasta /COAWST/Lib/MCT e execute o seguinte comando:

./configure

Este comando irá gerar o arquivo Makefile.conf, configure-o da seguinte forma:

# COMPILER, LIBRARY, AND MACHINE MAKE VARIABLES

# FORTRAN COMPILER VARIABLES #

# FORTRAN COMPILER COMMAND
FC		= mpif90

# FORTRAN AND FORTRAN90 COMPILER FLAGS
FCFLAGS		= -O2  

# FORTRAN90 ONLY COMPILER FLAGS 
F90FLAGS        = 

# FORTRAN COMPILE FLAG FOR AUTOPROMOTION 
# OF NATIVE REAL TO 8 BIT REAL
REAL8           = -r8

# FORTRAN COMPILE FLAG FOR CHANGING BYTE ORDERING
ENDIAN          = -convert big_endian

# INCLUDE FLAG FOR LOCATING MODULES (-I, -M, or -p)
INCFLAG         = -I

# INCLUDE PATHS (PREPEND INCLUDE FLAGS -I, -M or -p)
INCPATH         = -I/usr/local/include -I/home/luis/COAWST/Lib/MCT/mpeu

# MPI LIBRARIES (USUALLY -lmpi)
MPILIBS         = /usr/local/lib

# PREPROCESSOR VARIABLES #

# COMPILER AND OS DEFINE FLAGS
DEFS            = -DSYSLINUX -DCPRGNU

# SET A SEPARATE PREPROCESSOR COMMAND IF FORTRAN COMPILER 
# DOES NOT HANDLE PREPROCESSING OF SOURCE FILES

# FORTRAN PREPROCESSOR COMMAND
FPP		= cpp

# FOTRAN PREPROCESSOR FLAGS
FPPFLAGS	= -P -C -N -traditional -I/usr/local/include

# C COMPILER VARIABLES #

# C COMPILER
CC		= cc

# C COMPILER FLAGS - APPEND CFLAGS
ALLCFLAGS       = -DFORTRAN_UNDERSCORE_ -DSYSLINUX -DCPRGNU -O 

# LIBRARY SPECIFIC VARIABLES #

# USED BY MCT BABEL BINDINGS
COMPILER_ROOT = 
BABELROOT     = 
PYTHON        = 
PYTHONOPTS    = 

# USED BY MPI-SERIAL LIBRARY

# SIZE OF FORTRAN REAL AND DOUBLE
FORT_SIZE = 

# SOURCE FILE TARGETS #

# ENABLE RULE FOR PROCESSING C SOURCE FILES
# BY SETTING TO .c.o
CRULE           = .c.o

# ENABLE RULE FOR PROCESSING F90 SOURCE FILES 
# BY SETTING TO .F90.o AND DISABLING F90RULECPP
F90RULE         = .F90.o

# ENABLE RULE FOR PROCESSING F90 SOURCE FILES
# WITH SEPARATE PREPROCESSING INVOCATION
# BY SETTING TO .F90.o AND DISABLING F90RULE
F90RULECPP      = .F90RULECPP

# INSTALLATION VARIABLES #

# INSTALL COMMANDS
INSTALL         = /home/luis/COAWST/Lib/MCT/install-sh -c
MKINSTALLDIRS   = /home/luis/COAWST/Lib/MCT/mkinstalldirs

# INSTALLATION DIRECTORIES
abs_top_builddir= /home/luis/COAWST/Lib/MCT
MCTPATH         = /home/luis/COAWST/Lib/MCT/mct
MPEUPATH        = /home/luis/COAWST/Lib/MCT/mpeu
EXAMPLEPATH     = /home/luis/COAWST/Lib/MCT/examples
MPISERPATH      = 
libdir          = /home/luis/COAWST/Lib/MCT/lib
includedir      = /home/luis/COAWST/Lib/MCT/include

# OTHER COMMANDS #
AR		= ar cq
RM		= rm -f


Feito isto, execute o comando:

make

make install


4 – CONFIGURANDO O COMPILERS

Antes de começarmos a configurar o arquivo bash do COAWST, vamos configurar o compilador. Entre na pasta COAWST/Compilers e procure o arquivo Linux-gfortran.mk. Abra-o e configure da seguinte maneira:

5 – CONFIGURANDO O WRF

Eu, diferentemente do John, gosto de configurar o WRF sempre antes de compilar o COAWST. Então se você quiser fazer como eu siga sempre estas etapas.

Abra a pasta COAWST/WRF.

Execute o comando:

./clean –a (este comando irá limpar todas as suas configurações anteriores).

./configure (este comando irá gerar o arquivo configure.wrf)

Execute o editor de texto do Linux:

gedit configure.wrf

Neste arquivo devemos mudar apenas duas coisas muito importantes e fundamentais para que o modelo funcione.

1 – Configure a Flag MCT_LIBDIR para a sua pasta lib do MCT.

MCT_LIBDIR ?= /home/luis/COAWST/Lib/MCT/Lib

2- Essa é uma dica minha para o John que ele precisa colocar em seu tutorial, 90% dos problemas dos usuários não conseguirem compilar o COAWT está associado a este comando. Se você usa OpenMPI deve ser acrescentado a FLAG “DM_CC” a linha de comando - DMPI2_SUPPORT .

DM_CC	=	mpicc -DMPI2_SUPPORT


6 – CONFIGURANDO O BASH

Passamos a etapa de configuração do arquivo coaswt.bash. Segue abaixo a copia do meu arquivo para que vocês possam configurar de acordo com as suas pastas.

7 – COMPILAR O COAWST.BASH

Após tudo configurado, passemos a etapa de compilação do modelo.

No terminal execute o seguinte comando:

./coawst.bash –nocleanwrf

Aguarde o final da compilação e veja se o arquivo coawstM foi gerado.

8 – RODAR O COAWSTM

Se tudo deu certo até aqui (e torço para que tenha dado) vamos rodar o coawstM com o mpirun e ver se os ventos estão a nosso favor.

Antes de executar o mpirun, entre na pasta /COAWST/Projects/JOE_TCs e copie os arquivos wrfinput wrfbdy e o namelistinput. e cole-os na pasta raiz do COAWST.

Feito isto, execute a seguinte linha de comando:

mpirun –np 3 ./coawstM Projects/JOE_Ts/coupling_joe_tc.in

*** -np é o número de processadores utilizados para a compilação.

Se tudo der certo ele deve começar a rodar. E os resultados deverão ser gerados.

Espero que este tutorial seja útil, ele está em versão de atualização e estou aberto a sugestões e criticas para que eu possa melhorar as próximas versões.
