Por parte do Receptor, o processo de [[Demultiplexação]] se dá em realizar a leitura do cabeçalho daquele segmento recebido para que possa ser direcionado ao Socket correto.

O funcionamento da [[Demultiplexação]] funciona da seguinte foma:
	Cada host recebe um datagrama IP, cada datagrama possui um [[Endereço IP]] de origem e de destino, sendo esses atrelados a uma localização especifica do remetente e destinatário.
	Cada datagrama também carregará um segmento da camada de transporte.
	Cada segmento possui um numero de  porta do destino.

O formato do segmento [[TCP]] e [[UDP]] é representado da seguinte forma:
![[Pasted image 20250109170007.png]]

O processo de [[Demultiplexação]] é dado de duas formas:
* **Demultiplexação Não Orientado a Conexão**:
	Utiliza-se o protocolo [[UDP]] para este caso.
	Deve ser especificado quem é o [[Endereço IP]] destino e qual a porta destino para qual será encaminhado o segmento.
	Utiliza apenas o numero da porta de destino.
	Ao receber um segmento, será verificado a porta do destino e encaminhar o segmento ao Socket identificado por essa porta.
	Neste caso, datagramas que tiverem o mesmo endereço de porta e IPs diferentes ou endereço IP iguais e portas diferentes serão encaminhados ao mesmo Socket.
	Exemplo:
	![[Pasted image 20250203080833.png]]
	
* * **Demultiplexação Orientado a Conexão**:
	Utiliza-se o protocolo [[TCP]] para este caso.
	Cada Socket possui 4 elementos
	- Endereço IP origem
	- Endereço IP destino
	- Numero de Porta origem
	- Numero de Porta destino
	Um servidor que usa [[TCP]] pode suportar vários Sockets simultâneos. Cada Socket será associado a uma conexão diferente.
	Exemplo:
	![[Pasted image 20250203080647.png]]
	