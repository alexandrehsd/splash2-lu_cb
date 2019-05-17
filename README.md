# Profiling no super computador

## Configurando o ambiente e o arquivo makefile

Na pasta do projeto parsec, para habilitar o comando parsecmgmt no ambiente do sistema deve ser executado o comando

$ source env.sh

Na pasta .../lu_cb/src/ no arquivo makefile, deve ser escrito:

CFLAGS := $(CFLAGS) -Wall -O1 -pg -W -Wmissing-prototypes -Wmissing-declarations -Wredundant-decls -Wdisabled-optimization

CFLAGS := $(CFLAGS) -Wpadded -Winline -Wpointer-arith -Wsign-compare -Wendif-labels

LDFLAGS := $(LDFLAGS) -pthread -lm 

Onde, 

* -O1 representa a flag de otimização, recomenda-se usar -O1 ou -O0 ao invés de -O2 ou -O3.
* -pthread deve ser adicionado para executar as funções da biblioteca pthread.

## Construindo e executando o projeto

Após alteração caso já tenha feito o build antes disso:
 
$ parsecmgmt -a uninstall -p splash2.lu_cb

Caso não tenha feito a build ainda:

$ parsecmgmt -a build -p splash2.lu_cb

Depois executar normalmente: 

$ parsecmgmt -a run -p splash2.lu_cb -i 'native'

O arquivo será gerado dentro da pasta /run dentro do projeto.

Pasta projeto: /parsec-3.0/ext/splash2/kernels/lu_cb/

## Preparando a execução do profiling

na pasta *.../lu_cb/run/* foi criado um novo arquivo chamado *gmon.out*. Mova esta arquivo para a pasta *.../lu_cb/inst/amd64-linux.gcc/bin*, onde haverá um arquivo executável *lu_cb*.

## Profiling

executar o comando 

$ gprof lu_cb gmon.out

# Profiling na máquina local

na pasta *.../lu_cb/obj/amd64-linux.gcc* haverá um arquivo *lu.c*. Você também pode executar o profiling apenas com este arquivo na sua máquina local. Para tal, execute:

* Compilação:

$ gcc -pg -Wall -pthread -g -o lu lu.c -lm

* Execução:

$ ./lu

* Profiling

$ gprof lu gmon.out

# Auto vetorização

Para habilitar a auto vetorização do algoritmo e salvar o _output_ em um arquivo _vectorization.txt_, compile o código com a seguinte instrução:

gcc -pthread -g -o lu.o lu.c -O3 -ftree-vectorize -funsafe-math-optimizations -msse2 -ftree-vectorizer-verbose=2 > vectorization.txt -lm