#iMacros Manual
(http://pt.slideshare.net/gonzalezjjose/i-macros-manual)

##Opções de gravação:
- rápida (default) ou
- velocidade original (com WAIT)

##!REPLAYSPEED
- FATS
- MEDIUM (espera 0.25s entre cada comando)
- SLOW (1 segundo entre cada comando)

Quadro azul durante a execução, indicando onde foi selecionado: SET !POINTER YES

Todos os espaços em branco do valores, por exemplo, co parâmetro CONTENT, devem ser substituidos por um <SP>. Já a quebra de linha, por <BR>

##iMacros Password Manager: armazena nomes de usuário e senhas nos macros.
1. Sem criptografia
2. Senhas de sites criptografadas
3. Senhas de sistes criptografadas e pergunta pela senha Master
As senhas são gravada na criptografia AES, mas não a senha master. Ela só é mantida temporariamente na memória enquanto roda-se os macros. VocÊ precisa redigita-la, a cada execução.

##Salvamento de páginas web
Comando SAVEAS. Formatos:
- CPL arquivos e imagens salvas separadamente numa pasta
- MHT página e imagens salvas num só arquivo
- TXT só o texto é salvo, sem tags HTML
- EXTRACT o valor da variável !EXTRACT é salvo num CSV
- BMP uma captura de tela do site é salva. ex: SAVEAS TYPE=BMP FOLDER=* FILE=M.bmp

Downloads de arquivos são controlados pelo comando ONDOWNLOAD. Deve aparecer antes do comando que inicia o download.
Se necessário, aumentar o valor de timeout com SET !TIMEOUT. Ou, fazer um WAIT SECONDS=

Downloads de itens/imagens, através do botão SAVE ITEM na aba de Gravação/Recording
TAG POS=1 TYPE=IMG ATTR=HREF..... CONTENT=EVENT:SAVEITEM

Para salvar imagens, é possível adicionar o parâmetro CONTENT-EVENT: #SAVEPICTURESAS após a TAG que clica na imagem

Para salvar outros tipos de conteúdo, como .WVM, .AVI, .MP3 ou .PDF, clica-se no link que apresente o item (isso gerará um comando TAG) e adicione o parâmetro CONTENT-EVENT:SAVETARGETAS
(Para especificar onde salvar, usar o comando ONDOWNLOAD)

Para algumas outras imagens, é uma melhor estratégia salvar através do parâmetro SAVEPICTUREAS, pois ele simula o salvamento pelo comando do I.E. (e não através do HTML, como o SAVEITEM)

## GERENCIADOR DE DIÁLOGOS
###Login
	ONLOGIN, a senha é armazenada no método selecionado na aba de Segurança no diálogo de Opções

###Javascript
	ONDIALOG cuida de todas as caixas de diálogo Javascript. Você pode extrair o texto de um diálogo adicionando SET !EXTRACTDIALOG YES
	Algumas páginas abrem uma nova página após a resposta de um diálogo,  caso queira esperar por esse download, adicione o comando WAIT SECONDS= depois da TAG que inicia a caixa de diálogo

###Web Page Dialogs
	Exibem HTML no seu conteúdo (e não JS). Eles são controlados pelo comando ONWEBPAGEDIALOG. Como os diálogos podem conter botões ou caixas, você pdoe automatizar utilizando uma lista específica de comandos do teclado para elas. Por exemplo KEYS=Hello{ENTER}{CLOSE}
	Durante a execução do macro, ONWEBPAGEDIALOG KEYS={WAIT<sp>2}{CLOSE} é ativado por padrão para fechar diálogos indesejável

###Segurança
	ONSECURITYDIALOG. Por padrão as configurações são BUTTON=YES e CONTINUE=YES, mesmo sem a especificação do comando.

###Certificados
	ONCERTIFICATEDDIALOG

###Page Errors
	ONERRORDIALOG. BUTTON=YES e CONTINUE=YES por padrão


## PRINT
	Impressão: ONPRINT P=3 para selecionar a impressora 3.
	P=  ou P=* seleciona a última impressora usada

	Se a página utiliza frames e você quer imprimir somente um frame específico, você pode selecionar com WINCLICK antes.

	...
	SIZE X=644 Y=604
	FRAME=6
	WINCLICK X=462 Y=206 CONTENT=
	ONPRINT P=*
	PRINT

##Abas do Navegador
URL GOTO=
TAB OPEN
TAB T=2
TAB CLOSE
TAB T=1

##Frames
Para que seja possível a utilização do iMacros em uma página que se utiliza de frames, é preciso ter certeza de que a TAG ou EXTRACT estejam direcionadas no frame correto.
Para o EXTRACT, clique antes em um elemento que também esteja nesse frame para posicionar o iMacros corretamente.
Para uma TAG, pdoe ser que a página nao esteja complemente carregada, para isso, adicione um WAIT SECONDS= que espere o download completo da página
(F=n onde n é a posição do frame na "árvore de objetos" da página)

##Melhorando os comandos TAG

###Wildcards
Algumas páginas são criadas dinamicamente e os links contem números únicos - sessões ID. Para isso, é preciso substituir as partes que mudam pelo símbolo  de asterisco *

##Variáveis
A variável !VAR1 é acessada através da declaração {{!VAR1}}
Quase qualquer valor pode ser atribuido a uma variável, mas alguns símbilos precisam ser substituidos porque implicam um outro comportamento no iMacros.
Todos os espaços em branco precisam ser substituidos por <sp> e todas as quebras de linhas substituidas por <br>
duas chaves precisam ser  antecipadas de #NOVAR#. #novar#{{}}


- Existem 3 variáveis internas, !VAR1, !VAR2, !VAR3.
SET !VAR1 ...

Você pode pedir ao usuário para inserir um valor pelo PROMPT
PROMPT .... !VAR1

- Variáveis definidas pelo usuário
Podem ser criadas na linha de comando através de:
	imacros.exe -macro myMacro -var_ITEM 15
Cria a variável ITEM durante a execução de myMacro e dá o valor á variável 15

A segunda opção é usar a função iimSet  na interface de Scripting. Em um script de Visual Basic, seria:
	iret = imacros.iimSet("-var_ITEM", "15")

##Proxy Server
O comando PROXY instrui ao iMacros para se conectar à Internet através de um servidor proxy utilizando as configurações que você espeficificar.

Um servidor proxy age como um intermediário entre sua rede interna e a Internet.

Proxy para http e https
	PROXY ADDRESS=192.1.8.1:8080

Proxys diferentes para http e https
	PROXY ADDRESS=http=192.1.8.1:8080<sp>https=192.1.8.2:8080 BYPASS=NULL

Proxy não utiliza em URLs que incluam a palavra "iopus"
	PROXY ADDRESS=66.98.229.110:8080 BYPASS=*iopus*

Ao invés do IP, pdoe usar uma URL pro proxy também

##Enviando múltiplos Datasets para Web Sites
A fonte de dados (datasource) pdoe tanto ser um arquivo de texto com uma lista de variáveis e seus valores, como:
	key=value
Ou pode ser um arquivo no formato CSV, com uma virgula separando os valores.
	fernanda, aparecida, ferreira

Uma sugestão seria utilizar uma lista de variáveis se você tiver várias variáveis mas somente um ou poucos valores para cada variável. (Por exemplo, um endereço, com rua, número e cep)
O formato CSV é recomendado para a utilização de poucas variáveis com vários valores diferentes(Por exemplo, informações de uma longa lista de CDs)

###Input por um arquivo CSV
	SET !DATASOURCE OnlineAuction.csv
	SET !DATASOURCE_COLUMNS 3
	SET !DATASOURCE_LINE{{!LOOP}}

	TAG ... CONTENT={{!COL1}}

###Input por uma arquivo com uma lista de variáveis
A primeira linha do arquivo deve ser
	[iOpus]

No macro:
	SET !DATASOURCE lunch.txt
	TAG ... CONTENT={{name}}
	TAG ... CONTENT = {{main}}
 Onde name e main são as chaves (keys) do arquivos

 - Vários conjuntos de chave/valor
 Para arquivos com valores diferentes de uma mesma chave, é reciso adicionar um número após o nome da chave, e adicionar !LOOP após o nome no macro.
 	[iOpus]

 	name1=Tom
 	name2=Ann
 	name3=Nicole
 	main1=2
 	main2=1
 	main3=3

No macro:
	``TAG ... CONTENT={{name!LOOP}}´´

###Input pelo Banco de dados
Só funciona com Scripting Edition

Pode ler de qualquer banco de dados Windows

##Extrair dados dos sites
###Extrair elementos únicos
Possui o Ex




