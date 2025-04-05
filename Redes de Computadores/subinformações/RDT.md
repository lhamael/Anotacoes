O RDT trabalha da seguinte forma:
* No lado do emissor:
	ele receberá um chamado da camada superior e passará esses esses dados para a camada inferior (rdt_send());
	A camada de transporta tranforma esses dados em um segmento e então envia esse segmento para a próxima camada envia-lo pela rede (udt_send());
* No lado do receptor:
	Ele recebe da camada inferior um segmento (rdt_rcv());
	A camada de transporte vai desempacotar esse segmento e enviar para a camada superior (deliver_data());

Infelizmente, os dados enviados obrigatóriamente passam por um canal não confiavel, isso faz com que nem o emissor saiba o estado do receptor, nem o receptor sabe o estado do emissor. Então, surgiu-se diferentes versões do RDT para resolver esse problema  de confiabilidade. 
Para apresentar essas verções do RDT, trabalha-se com um sistema de maquina de estados:
![[Pasted image 20250203085424.png]]

#### RDT1.0
Nesta versão, o rdt trabalha com um sistema sem erro de bits e sem perda de pacotes, um sistema ideal. Será trabalhado em maquinas de estados separadas entre emissor e receptor
* Emissor:
	Envia dados para a camada inferior
* Receptor:
	Lê os dados que vem da camada inferior

Notas pessoais:
![[Pasted image 20250203090150.png]]
Notas do Slide:
![[Pasted image 20250203090217.png]]
##### Problemas:
No mundo real, não trabalhamos com sistema ideias, então como fazer com que o emissor e receptor sejam resistentes a troca de bits e a perda de pacotes?
#### RDT2.0
Implementa-se o sistema de checksum, para detectar erros.
Utiliza-se o processo de retransmissão e confirmação de pacotes por parte do receptor. Agora o receptor passa a enviar um acknowledgements (ACK) se recebeu bem um pacote ou um negative acknowledgements (NAK) se tiver recebido mal o pacote.
Em suma, agora o RDT trabalha no sistema para e espera (Stop and wait). Basicamente ele envia e espera uma resposta.

**Notas do caderno:**
![[Pasted image 20250203090959.png]] ![[Pasted image 20250203091019.png]]

**Notas do livro:**
![[Pasted image 20250203090741.png]] ![[Pasted image 20250203091106.png]]

##### Problemas:
Se o ACK ou o NAK for corrompido, o emissor não saberá o que aconteceu, e poderá retransmitir um pacote  já recebido, duplicando-o ou simplesmente transmitir o próximo pacote, gerando um erro.
#### RDT2.1
Agora o emissor trabalha com ACK/NAK errados.
Agora, o emissor poderá receber ACKs errados ou embaralhados e o receptor poderá receber pacotes duplicados. O receptor passa a descartar pacotes duplicados ou errados.
Além disso o receptor e o emissor passam a trabalhar com um sistema de sequencia de um bit, 0 ou 1.

**Notas do livro:**
* Emissor:
	![[Pasted image 20250203092106.png]]
* Receptor:
	![[Pasted image 20250203092136.png]]

**Notas do caderno:**
* Emissor:
	![[Pasted image 20250203092420.png]]
* Receptor:
	![[Pasted image 20250203092437.png]]
##### Problemas:
No emissor o sistema passa a ter o dobro de estados anteriores, será que é possivel reduzir essa quantidade de pacotes?
Talvez não seja necessário que o receptor tenha de enviar NAKs para o emissor para indicar que ele recebeu um pacote.
#### RDT2.2
Possui as mesma funcionalidades do rdt2.1 só que agora o receptor não envia mais NAKs. Agora o receptor envia apenas o ACK do último pacote recebido corretamente.
Agora o receptor deve incluir o numero de sequencia no pacote. 
Já o emissor passa a retransmitir os pacotes se ele receber ACKs duplicados. Os ACKs serão duplicados se eles tiverem o mesmo numero de sequencia.

**Notas do livro:**
	![[Pasted image 20250203093525.png]]

**Notas do caderno:**
	![[Pasted image 20250203093648.png]]

##### Problemas:
E se os pacotes forem perdidos, o que deve ser feito? 

#### RDT3.0
A partir de agora, o emissor passará a ter um contador de tempo que vai esperar a resposta do receptor.
	Se o a resposta demorar mais do que o tempo, a retransmissão será duplicada, mas o receptor já possui um sistema de respostas duplicadas então ele conseguirá lidar bem com isso.
	O receptor deverá especificar o numero de sequencia.

**Notas do livro:**
* Emissor:
	![[Pasted image 20250203094424.png]]
**Notas do caderno:**
	![[Pasted image 20250203094529.png]]

A ação do rdt3.0 passa a ser diferente a depender de como ele é transmitido pela rede:
	![[Pasted image 20250203094732.png]]
	![[Pasted image 20250203094757.png]]

#### Desempenho do RDT:
Utiliza-se a métrica de utilização para fazer essa analise. Ela consiste em medir a fração de tempo que o emissor está ocupado enviando o pacote. (U sender)

Exemplo: Enlace de 1Gbps(10^9 bits/segundo), atraso de propagação de 15 ms(0,015) e um pacote de 8000 bits:
![[Pasted image 20250203095335.png]]
Isso se dá da seguinte forma, o emissor envia um pacote no tempo 0 e a pacote é enviado e recebe o ACK a um tempo de 1 RTT. Isso se dá da seguinte forma:
![[Pasted image 20250203095537.png]]
Ou seja, o tempo de utilização será de 0,00027 segundos.
O maior problema é a baixa utilização dessa infraestrutura, uma vez que só poderá ser enviado um pacote por vez. Para isso o protocol passa a trabalhar com um sistema de Pipeline.

### [[Pipeline]]
A ideia do pipeline agora é ao invés de transmitir apenas um pacote por vez, passa-se a utilizar o conceito de "Janela", em que múltiplos pacotes passam a ser enviados por vez. Isso dá um tempo de utilização muito inferior ao do sistema stop and back
![[Pasted image 20250203100205.png]]

#### Go-Back-N
Nesse sistema, o emissor passa a utilizar uma "janela" em que serão enviados N pacotes em um curto espaço de tempo, otimizando assim o envio de dados:
Emissor:
	Agora o ACK passa a ser acumulativo, assim se a janela só será deslocada se for recebido o ACK do pacote mais antigo.
	![[Pasted image 20250203100443.png]]
	**Informações:**
		Verde: Pacotes já enviados que receberam a confirmação;
		Amarelo: enviado que ainda não recebeu a confirmação;
		Azul: Utilizando mas ainda não enviado;
		Branco: Não estão sendo utilizados nem forma enviados.

Receptor:
	Este passa a trabalhar com o sistema de enviar ACKs apenas dos pacotes recebidos corretamente. 
	Caso receba algum pacote fora de ordem, o receptor irá descarta-lo ou armazena-lo em um buffer se ele possuir um, e reenviar o ACK faltante.
	![[Pasted image 20250203101709.png]]
	**Informações:**
		Verde: Pacotes já recebidos e confirmados;
		Rosa: Pacotes fora de ordem;
		Cinza: Pacotes ainda não recebidos
		rcv_base: esperando o pacote chegar e envia o ACK que estive esperando;


Agora a maquina de estados passa a ter algumas mudanças:
Emissor:
	Inclui-se o numero de sequencia e a base da janela, e também como que irá se comportar caso o timer dê 0 ou os dados tenham sido rejeitados;
	![[Pasted image 20250203102129.png]]
Receptor:
	O receptor a enviar o próximo ACK se o ultimo pacote confirmado for o que estiver faltando ou então reenvia o último ACK se o pacote estiver corrompido ou o numero de sequencia estiver incorreto.
	![[Pasted image 20250203102452.png]]

O modo de operação o Go-Back-N de dá da seguinte forma:
![[Pasted image 20250203102716.png]]



#### Repetição seletiva
Trabalha armazenando os pacotes fora de ordem em um buffer e solicitando apenas os pacotes faltantes.
Tanto o emissor quanto o receptor possuem uma janela e isso faz com que os pacotes possam ser confirmados fora de ordem. Desta forma, o receptor confirma os pacotes recebidos e envia apenas o ACK faltante.
![[Pasted image 20250203103701.png]]

No caso do emissor, se acontecer um Timout de algum pacote, o emissor reenvia o pacote e reinicializa o timer. 
Quanto o receptor, o pacote que tiver dentro da Janela e tiver sido recebido, ele vai encaminhar o ACK de confirmação e tratar o pacote recebido se tiver na ordem. Se tiver fora de ordem, ele armazena o pacote no buffer e envia o ACK faltante.

O funcionamento se dá da seguinte forma:
![[Pasted image 20250203104219.png]]

