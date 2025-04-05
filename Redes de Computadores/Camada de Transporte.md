A principal função da [[Camada de Transporte]] é prover  uma comunicação logica entre processos de aplicação rodando em diferentes dispositivos.

Essa camada realiza duas ações principais:
* **Enviar** as mensagens recebidas da camada de aplicação quebrando-as em segmentos e enviando pela rede.
* **Receber** os segmentos da rede e verificar, ou não, se todos os pacotes foram recebidos adequadamente.
O processo de **envio** na camada de transporte acontece da seguinte forma:
	A [[Camada de Transporte]] recebe por meio do socket uma mensagem gerada pela aplicação, então é alocado um cabeçalho a esta mensagem e assim cria-se um Segmento que é entregue a próxima camada.
Já o receptor da camada de transporte realizar a seguinte ação:
	A [[Camada de Transporte]] recebe esse seguimento da [[Camada de Rede]] e realiza a leitura do cabeçalho. Identificado o cabeçalho, ela então remove esse cabeçalho e entre a [[Camada de Aplicação]] a mensagem por meio do Socket.

Nesta camada, 2 protocolos foram definidos como principais protocolos de envio e recebimento de seguimentos, são eles o [[TCP]] e o [[UDP]].

O principal diferencial da [[Camada de Transporte]] para a [[Camada de Rede]] se dá no fato de que essa camada é responsável por verificar se a mensagem não terá problema ao ser enviada e definir os endereços do IP e da porta. Enquanto que a [[Camada de Rede]] é responsável por identificar o caminho entre Hosts.
Fazendo uma analogia com o sistema de correios, a [[Camada de Transporte]] é como se fosse o verificador dos correios para saber para que cidade e para qual casa uma encomenda vai, enquanto que a [[Camada de Rede]] é o responsável logístico para saber qual o caminho que essa encomenda terá de percorrer para chegar integro em seu destinatário.

Assim como um servidor é capaz de enviar requisições para seus clientes, ele também deve ser capaz de recebe-las. Contudo, um mesmo servidor pode receber inúmeras requisições simultaneamente, então como saber de qual usuário pertence uma determinado requisição. Para isso, é usado o processo de [[Multiplexação]] e [[Demultiplexação]].

Na camada de Transporte, aprofundamos ainda mais como se dá o funcionamento do [[TCP]] e do [[UDP]].

Quando os segmentos são enviados de um dispositivo para outro dispositivo não é algo visível para ambos os dispositivos. O processo de transmissão de dados é feito por canais não confiáveis em que o emissor não sabe se o dado vai ser entregue corretamente e o emissor também não sabe se vai receber todos os dados. É quase como uma comunicação por trás de uma cortina, em que o emissor não sabe quem está recebendo os pacotes nem se o pacote foi recebido corretamente, muito parecido com o envio de cartas. 
Para isso, a camada de Transporte trabalha com o principio da transferência de dados Confiável, conhecido como [[RDT]]. Importante ressaltar que o RDT é um sistema que é utilizado em especial no protocolo [[TCP]], uma vez que o mesmo possui um sistema de confirmação de envio, controle de fluxo e controle de congestionamento.

Durante a comunicação entre um servidor e um cliente, pode acontecer alguns congestionamentos na rede, para isso, existem algumas formas de como a rede pode estabelecer um [[Controle de Congestionamento]]. Para evitar esse congestionamento na rede, o [[TCP]] possui um sistema de controle de congestionamento interno. 