* Transporte confiável de informação
	Garante a confiança no envio e recepção de processos pela internet estabelecendo uma conexão entre o cliente e o servidor.
* Controle de fluxo
	Controla a quantidade de pacotes que serão enviados pela rede para evitar eventuais perdas e ter uma constância no fluxo de dados.
* Controle de congestionamento
	Por estabelecer uma rota até o servidor, o TCP evita congestionamento de dados na rede, garantindo uma fluidez mínima.
* Conexão orientada
	Estabelece uma conexão entre e o cliente e o servidor para enviar o processo.

Por garantir uma serie de controles, o TCP normalmente tende a ser mais lento do que outros protocolos.

### Funcionamento do TCP
Trata-se de um protocolo confiável que trabalha ponto-a-ponto entre o emissor e receptor.
É um protocolo confiável que trabalha com entrega de um conjunto de bits em ordem.
Trabalha em um sistema de fluxo de dados bidirecional em uma mesma conexão que serão manipulados a partir de um segmento que possui um tamanho máximo.
Além disso, ele trabalha com ACKs acumulativos e trabalho com o sistema de pipeline do [[RDT]].
Trabalha com um controle de fluxo implementado a partir de um tamanho de janela especifica.
Além disso, o protocolo precisa estabelecer uma conexão inicial entre emissor e receptor para que haja a troca de dados.
É um protocolo que implementa um controle de fluxo para evitar que o emissor sobrecarregue o receptor.
O **tamanho mínimo** do cabeçalho TCP é **20 bytes**.

 * ***Estrutura do segmento do TCP:**
	![[Pasted image 20250203110326.png]]

Para garantir a comunicação confiável e ordenada, o TCP trabalha com um sistema de envio de numero de sequencia e ACK de confirmação. Isso faz com que o segmento seja organizando segundo o numero de sequencia e o ACK confirmado:
![[Pasted image 20250203112248.png]]
Além disso, como foi abordado no funcionamento do [[RDT]], o TCP trabalha com um sistema atrelado ao Timout, em que reenvia dados caso dê timeout, que se chama [[round trip time (RTT)]]. Para tentar estimar o tempo do RTT, o TCP implementar a **exponetial weighted moving average (EWMA)**:
![[Pasted image 20250203112947.png]]

Em sua, a **EWMA** consiste em estimar o valor aproximado do RTT, sendo que este será 1-alpha(~0.125) vezes o valor antigo estimado de RTT mais alpha(~0.125) vezes a amostra do RTT naquele instante.

Portanto, o Timeout do TCP irá trabalhar com esse valor estimado de RTT mais uma margem de segurança para caso haja uma variação muito grande.
![[Pasted image 20250203113627.png]]

#### Emissor TCP simplicado
Após receber o dado da camada de aplicação, é criado um segmento com um numero de sequencia, que é o numero de bytes do primeiro byte de dados presente no seguimento.
Se não tiver nenhum timer em funcionamento, então ele inicializa o timer e define o intervalo de expiração como sendo o TImeoutInterval.

Caso aconteça o evento de Timeout, o segmento que causou o timeout será retransmitido, e o timer será reinicializado.

Caso receba um ACK, ele verifica se o ACK confirma os seguimentos que ainda não foram confirmados, atualiza os ACKs confirmados e inicializa um timer se ainda existir segmentos não confirmados.

**Cenarios de retransmissão de dados:**
	![[Pasted image 20250203114337.png]]
	![[Pasted image 20250203114355.png]]


Uma das principais diferenças entre o TCP e o UDP é que o TCP faz um gerenciamento de conexão enviando uma requisição de comunicação inicial e informando que deseja estabelecer uma conexão passando algumas informações iniciais sobre o sobre cada um, como [[Endereço IP]], [[Numero da porta]], e uma mensagem informando o desejo de estabelecer uma conexão.
Contudo, nem sempre pode acontecer o "handshake", uma vez que pode demorar bastante tempo para estabelecer uma conexão ou até mesmo estabelecer uma conexão duplicada. Neste caso o Servidor implementa o sistema de aperto de mão por 3 vias
#### 3-way handshake
Nesse sistema, o cliente envia ao servidor um segmento com um cabeçalho que contem um SYNBit = 1 e uma sequencia especifica que irá indicar que ele quer estabelecer uma conexão com o servidor.
Então o servidor envia ao cliente um segmento confirmando a conexão que contem um SYNBit = 1, outro numero de sequencia, um ACKbit = 1 e um ACKnum = sequencia do segmento recebido mais 1;
Finalmente, o cliente recebe o segmento de confirmação, e envia outro segmento confirmando que a conexão foi estabelecida, que contem ACKbit = 1 e ACKnum = numero de sequencia o segmento recebido + 1;
![[Pasted image 20250203120733.png]]
Quando o Cliente e o servidor não quiserem mais trocar informações, eles devem fechar a conexão, enviando um segmento que contem FINbit = 1.
A resposta a esse segmento será outro segmento com o ACK combinado com o FINbit sendo igual a 1.

### Sistema de Controle de fluxo
A camada de transporte deve receber e enviar sockets para a camada de aplicação e segmentos para a camada de rede. Contudo nem sempre a camada de aplicação consegue receber todos os sockets enviados pela camada de transporte a tempo antes que a camada de rede envie mais segmentos. Para isso, o TCP possui um sistema de controle de fluxo.
![[Pasted image 20250203115030.png]]
Para evitar que o buffer fique cheio e perda pacotes recebidos pela camada de rede, ele implementa no cabeçalho do TCP um campo para controle de fluxo que contem o numero de bytes máximo que o receptor pode aceitar.
Ou seja, o controle de fluxo é um controle que o receptor vai encaminhar ao emissor informando a capacidade máxima de pacotes que ele consegue processar para evitar overflow.


### Sistema de Controle de Congestionamento
O protocolo TCP possui um sistema de controle de congestão que é conhecido como algoritmo AIMD (Additive Increase Multiplicative Decrease). A ideia principal do algoritmo consiste em ir adicionando um pacote na janela para envio enquanto não for detectado nenhuma perda de pacotes. Se for identificado alguma perda, será cortado na metade a taxa de envio (tamanho da janela). Esse sistema se repete até que todo o pacote tenha sido enviado e recebido. A ideia principal do AIMD é otimizar a taxa do fluxo de congestionamento da rede.
O AIMD funciona definindo o tamanho da janela como sendo o último byte enviado menos o ultimo byte confirmado:
![[Pasted image 20250204083559.png]]
Essa janela (cwnd) será dinamicamente ajustado em resposa ao congestinamento observado na rede. Assim, a taxa de envio do emissor ao receptor será calculada  como sendo o tamanho da janela dividido pelo RTT:
![[Pasted image 20250204083813.png]]
Quanto menor for a janela, menor será a taxa de envio, quanto maior for a janela, maior será a taxa de envio.
Além disso, o TCP também estabelece um mecanismo de segurança pra iniciar uma comunicação, chamado de Slow Start.
* Slow Start
	O Slow start consiste em definir o tamanho inicial da janela sendo de 1 segmento e o tamanho da janela será duplicado a cada RTT se tudo acontecer bem. O incremento na janela será feito conforme é recebido cada um dos ACKs do receptor.
	![[Pasted image 20250204084301.png]]

No caso em que aconteça perdas, existem dois métodos diferentes de redução do tamanho da janela. No TCP Reno ele reduz pela metade do valor estabelecido dantes da perda, já no TCP Tahoe será reduzido para o tamanho inicial.

Além do TCP AIMD, existe uma versão mais moderna do controle de congestionamento do TCP que é o TCP CUBIC. A ideia dele consiste em, após colocar o tamanho da janela na metade, ele passará a crescer de forma mais acelerada até o tamanho que houve o último congestionamento e depois vai crescer gradualmente até que seja identificada outra congestão.
![[Pasted image 20250204085202.png]]
O mais importante do TCP CUBIC é estabelecer um ponto K que foi o máximo alcançado. Ou seja, haverá um crescimento mais rápido do tamanho da janela quanto mais distante de K ele estiver e diminui-se o aumento quanto mais próximo de K.

Para evitar um congestionamento da rede, o TCP trabalha com um conceito de Justiça. Isso consiste em se um conjunto de cliente compartilharem um mesmo enlace de gargalo (Bottleneck link) haverá um congestionamento daquele enlace.
Para evitar esse congestionamento, o TCP utilizar um algoritmo comunitária. Em suma, se um TCP estiver passando por um congestionamento, assume-se que todos os TCPs de todos os Clientes também estarão passando, logo ele então reduz a sua taxa de emissão de segmentos esperando que os outros clientes também reduzam para diminuir esse congestionamento.
Além disso, o TCP utiliza-se do conceito de neutralidade na rede para estabelecer essa redução, que diz que nenhum cliente terá prioridade no processamento de suas mensagens em detrimento de outro, então se todos serão vistos de forma igual, todos deveram reduzir suas taxas em um congestionamento.
Ou seja, o TCP segue um comportamento justo, acreditando que todos serão justos.
Apesar do TCP ser um protocolo justo, outros protocolos como o UDP não são tão justos assim, fazendo um consumo muito maior e desnecessário da rede do que o TCP. Apesar de a maioria das aplicações multimídia utilizarem o UDP para evitar esse controle de congestionamento, o TCP ainda não passou por grandes problemas com isso.