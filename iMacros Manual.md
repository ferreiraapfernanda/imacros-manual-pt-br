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
Comando <code>SAVEAS</code>. Formatos:
- <b>CPL</b> arquivos e imagens salvas separadamente numa pasta
- <b>MHT</b> página e imagens salvas num só arquivo
- <b>TXT</b> só o texto é salvo, sem tags HTML
- <b>EXTRACT</b> o valor da variável !EXTRACT é salvo num CSV
- <b>BMP</b> uma captura de tela do site é salva. ex: SAVEAS TYPE=BMP FOLDER=* FILE=M.bmp

Downloads de arquivos são controlados pelo comando <code>ONDOWNLOAD</code>. Deve aparecer antes do comando que inicia o download.
Se necessário, aumentar o valor de timeout com <code>SET !TIMEOUT</code>. Ou, fazer um <code>WAIT SECONDS=</code>

Downloads de itens/imagens, através do botão <b>SAVE ITEM</b> na aba de Gravação/Recording
<code>TAG POS=1 TYPE=IMG ATTR=HREF..... CONTENT=EVENT:SAVEITEM</code>

Para salvar imagens, é possível adicionar o parâmetro <code>CONTENT-EVENT: #SAVEPICTURESAS</code> após a TAG que clica na imagem

Para salvar outros tipos de conteúdo, como .WVM, .AVI, .MP3 ou .PDF, clica-se no link que apresente o item (isso gerará um comando TAG) e adicione o parâmetro <code>CONTENT-EVENT:SAVETARGETAS</code>
(Para especificar onde salvar, usar o comando ONDOWNLOAD)

Para algumas outras imagens, é uma melhor estratégia salvar através do parâmetro <code>SAVEPICTUREAS</code>, pois ele simula o salvamento pelo comando do I.E. (e não através do HTML, como o SAVEITEM)

## GERENCIADOR DE DIÁLOGOS
###Login
<code>ONLOGIN</code>, a senha é armazenada no método selecionado na aba de Segurança no diálogo de Opções

###Javascript
<code>ONDIALOG</code> cuida de todas as caixas de diálogo Javascript. Você pode extrair o texto de um diálogo adicionando <code>SET !EXTRACTDIALOG YES</code>.
Algumas páginas abrem uma nova página após a resposta de um diálogo,  caso queira esperar por esse download, adicione o comando WAIT SECONDS= depois da TAG que inicia a caixa de diálogo

###Web Page Dialogs
Exibem HTML no seu conteúdo (e não JS). Eles são controlados pelo comando <code>ONWEBPAGEDIALOG</code>. Como os diálogos podem conter botões ou caixas, você pdoe automatizar utilizando uma lista específica de comandos do teclado para elas. Por exemplo <code>KEYS=Hello{ENTER}{CLOSE}</code>
Durante a execução do macro, <code>ONWEBPAGEDIALOG KEYS={WAIT<sp>2}{CLOSE}</code> é ativado por padrão para fechar diálogos indesejável

###Segurança
<code>ONSECURITYDIALOG.</code> 
Por padrão as configurações são BUTTON=YES e CONTINUE=YES, mesmo sem a especificação do comando.

###Certificados
<code>ONCERTIFICATEDDIALOG</code>

###Page Errors
<code>ONERRORDIALOG. </code> 
BUTTON=YES e CONTINUE=YES por padrão


## PRINT
<code>Impressão: ONPRINT P=3 para selecionar a impressora 3.
P=  ou P=* seleciona a última impressora usada

Se a página utiliza frames e você quer imprimir somente um frame específico, você pode selecionar com WINCLICK antes.

<code>...
SIZE X=644 Y=604<br>
FRAME=6<br>
WINCLICK X=462 Y=206 CONTENT=<br>
ONPRINT P=*<br>
PRINT</code>

##Abas do Navegador
<code>URL GOTO=<br>
TAB OPEN<br>
TAB T=2<br>
TAB CLOSE<br>
TAB T=1</code>

##Frames
Para que seja possível a utilização do iMacros em uma página que se utiliza de frames, é preciso ter certeza de que a TAG ou EXTRACT estejam direcionadas no frame correto.
	Para o EXTRACT, clique antes em um elemento que também esteja nesse frame para posicionar o iMacros corretamente.

	Para uma TAG, pdoe ser que a página nao esteja complemente carregada, para isso, adicione um WAIT SECONDS= que espere o download completo da página
(F=n onde n é a posição do frame na "árvore de objetos" da página)

##Melhorando os comandos TAG
###Wildcards
Algumas páginas são criadas dinamicamente e os links contem números únicos - sessões ID. Para isso, é preciso substituir as partes que mudam pelo símbolo  de asterisco <code>*</code>

##Variáveis
A variável <code>!VAR1</code> é acessada através da declaração <code>{{!VAR1}}</code>
Quase qualquer valor pode ser atribuido a uma variável, mas alguns símbilos precisam ser substituidos porque implicam um outro comportamento no iMacros.
Todos os espaços em branco precisam ser substituidos por <sp> e todas as quebras de linhas substituidas por <br>
duas chaves precisam ser  antecipadas de #NOVAR#. 
<code>#novar#{{}}</code>


Existem 3 variáveis internas, <code>!VAR1, !VAR2, !VAR3</code>.
<code>SET !VAR1 ...</code>

Você pode pedir ao usuário para inserir um valor pelo PROMPT
	<code>PROMPT .... !VAR1</code>

- Variáveis definidas pelo usuário
Podem ser criadas na linha de comando através de:

<code>imacros.exe -macro myMacro -var_ITEM 15</code>
Cria a variável ITEM durante a execução de myMacro e dá o valor á variável 15

A segunda opção é usar a função iimSet  na interface de Scripting. Em um script de Visual Basic, seria:
<code>iret = imacros.iimSet("-var_ITEM", "15")</code>

##Proxy Server
O comando PROXY instrui ao iMacros para se conectar à Internet através de um servidor proxy utilizando as configurações que você espeficificar.

Um servidor proxy age como um intermediário entre sua rede interna e a Internet.

Proxy para http e https
<code>PROXY ADDRESS=192.1.8.1:8080</code>

Proxys diferentes para http e https
<code>PROXY ADDRESS=http=192.1.8.1:8080<sp>https=192.1.8.2:8080 BYPASS=NULL</code>

Proxy não utiliza em URLs que incluam a palavra "iopus"
<code>PROXY ADDRESS=66.98.229.110:8080 BYPASS=*iopus*</code>

Ao invés do IP, pdoe usar uma URL pro proxy também

##Enviando múltiplos Datasets para Web Sites
A fonte de dados (datasource) pdoe tanto ser um arquivo de texto com uma lista de variáveis e seus valores, como:
<code>key=value</code>
Ou pode ser um arquivo no formato CSV, com uma virgula separando os valores.
<code>fernanda, aparecida, ferreira</code>

Uma sugestão seria utilizar uma <b>lista de variáveis</b> se você tiver várias variáveis mas somente um ou poucos valores para cada variável. (Por exemplo, um endereço, com rua, número e cep)
O <b>formato CSV</b> é recomendado para a utilização de poucas variáveis com vários valores diferentes(Por exemplo, informações de uma longa lista de CDs)

###Input por um arquivo CSV
<code>SET !DATASOURCE OnlineAuction.csv
SET !DATASOURCE_COLUMNS 3
SET !DATASOURCE_LINE{{!LOOP}}

TAG ... CONTENT={{!COL1}} </code>

###Input por uma arquivo com uma lista de variáveis
A primeira linha do arquivo deve ser
<code>[iOpus]</code>

No macro:
<code>SET !DATASOURCE lunch.txt
TAG ... CONTENT={{name}}
TAG ... CONTENT = {{main}}</code>

Onde <code>name</code> e </code>main</code> são as chaves (keys) do arquivos


<b>Vários conjuntos de chave/valor</b>

Para arquivos com valores diferentes de uma mesma chave, é reciso adicionar um número após o nome da chave, e adicionar <code>!LOOP</code> após o nome no macro.

<code>[iOpus]

 name1=Tom
 name2=Ann
 name3=Nicole
 main1=2
 main2=1
 main3=3
</code>

No macro:
	<code>TAG ... CONTENT={{name!LOOP}}</code>

###Input pelo Banco de dados
Só funciona com Scripting Edition.

Pode ler de qualquer banco de dados Windows

##Extrair dados dos sites
###Extrair elementos únicos
Possui o botão Extract Data
O comando <code>EXTRACT</code> é controlado por três parâmetros diferentes: a âncora, a posição e o tipo de extração.
O mais importante é a âncora. Você deve usar <b>*</b> no final da âncora de extração.
Se o código inserido na âncora aparecer mais de uma vez na página, o parâmetro de posição determina qual ocorrência será extraída.
O tipo de extração determina se o resultado é um texto, HTML, URL, um titulo ou um texto alternativo de uma imagem.

Todos os resultados extraidos podem ser acessados no macro pela variável interna <code>!EXTRACT</code>




