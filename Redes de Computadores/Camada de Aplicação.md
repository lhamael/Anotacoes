Trata-se de um programa que irá abstrair o processo de comunicação pela rede, para dar a impressão ao usuário que ele está se comunicando diretamente com o destino.
* Existem dois tipos de paradigma mais utilizados:
	[[Paradigma Cliente-Servidor]]
	[[Paradigma P2P]]

O paradigma utilizado pela aplicação pode utilizar um dos paradigmas ou ambos a depender da necessidade.
Para realizar essa comunicação, ainda no dispositivo, existe os [[Sockets]] para auxiliarem essa comunicação e transmitirem a mensagem.
Para receber e enviar as mensagens pela rede, é necessário a existência de uma identificação. Os responsáveis por essa identificação serão o [[Endereço IP]] e o [[Numero da porta]], que funcionaram como endereços para identificar de onde a mensagem saiu e para onde ela vai.
Para estabelecer qualquer tipo de comunicação, é necessário ser bem definido o [[protocolo da camada de aplicação]], só assim dois dispositivos diferentes serão capazes de conseguir estabelecer uma conexão.
Além do protocolo, uma aplicação precisa estabelecer algumas características importantes
* Integridade dos dados
	Algumas aplicações precisam receber 100% dos seus dados para funcionar adequadamente, é o caso das Lives em que a perda de pacotes pode resultar na diminuição da qualidade ou até mesmo no corte de pedaços da live. Já em casos como o download de arquivos, a perda de algumas partes do pacote não necessita em um impacto tão grande no resultado final.
	
* Tempo
	Algumas aplicações requerem um tempo mínimo de espera para funcionarem adequadamente, é o caso das ligações, em que se tiver uma espera muito grande na recepção dos pacotes, pode causar uma fala mais truncada e atrasada afetando a compreensão.
	
* Vazão dos dados pela rede
	Algumas aplicações requerem uma quantidade mínima de arquivos enviados. Caso envie uma quantidade inferior de arquivos, isso pode afetar diretamente o funcionamento do app.
	
* Segurança
	Algumas aplicações requerem uma grande nível de segurança, é o caso por exemplo de aplicações de banco.

Para conseguir realizar todos os processos de envio e recepção de mensagens, a camada de aplicação precisa de alguns serviços da camada de transportes, dois deles são o [[TCP]] e o [[UDP]].

Uma das aplicações mais utilizadas é a web, que consiste em um conjunto de objetos organizados em uma "folha em branco". Um objeto pode ser um arquivo HTML, uma imagem JPEG, um vídeo WAV ou qualquer outro tipo de arquivo que pode estar presente em uma pagina.
Cada pagina na web é composto por um host name e um path name.

O protocolo mais utilizado para estabelecer uma comunicação com um servidor e receber os objetos é o [[HTTP]].
Como uma forma de armazenar o estado anterior do usuário em um Site, foi criado o [[Cookie]].
Em alguns casos, existe a necessidade de utilizar um [[servidor proxy]] como uma forma de diminuir a quantidade de requisições feitas na rede interna. 

Como uma forma de facilitar ao usuário de conseguir acessar um site sem precisar ter que ficar lembrando sempre do IP do site, uma vez que lembrar de um nome é mais fácil do que lembrar de um conjunto de letras e números, implementou-se então o [[DNS]].

Além do [[Paradigma Cliente-Servidor]], também existe o [[Paradigma P2P]], que apesar de não ser muito utilizado, é um protocolo extremamente importante levando em conta a independência do usuário com uma empresa. Além disso, o [[Paradigma P2P]] tende a ser mais rápido no tempo de transferência de arquivos do que o  [[Paradigma Cliente-Servidor]].

Como uma adesão cada vez maior pelos streamings de vídeo, foi necessário criar uma solução para transmitir grandes arquivos de vídeo pela internet causando o mínimo de congestão da rede possível. Para isso, foram implementadas diferentes formas de codificação de vídeo para conseguir envia-lo pela rede.

Uma forma de os Streamings de vídeo disponibilizarem para seus usuários um vídeo de forma dinâmica sem muitas quedas é o por meio do [[DASH]]. Enquanto que o protocolo [[DASH]] tenta resolver o problema de qual o segmento de vídeo que o cliente vai querer baixar na qualidade especifica dada a necessidade dele naquele momento, contudo, ainda existe o problema de o cliente escolher de qual servidor o cliente vai baixar o conteúdo. Para isso surge o [[CDN]] 