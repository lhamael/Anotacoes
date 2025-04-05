# [[Camada de Transporte]]
* É implementada na ponta de cada enlace físico que conecta um host/roteador/comutador a outro.
* Implementa o serviço de comunicação logica entre processos de aplicação rodando em diferentes dispositivos
* Ela utiliza os os serviços da [[Camada de Rede]] para implementar os seus serviços.
* Os principais protocolos adotados são:
	* **[[TCP]]**
		* O **tamanho mínimo** do cabeçalho é **20 bytes**.
		* Adota sockets diferentes para cada mensagem recebida da camada superior.
		* O Timer é calculado com sendo o RTT estimado + uma margem de segurança.
			![[Pasted image 20250203112947.png]]
			Alpha = ~0.125
			![[Pasted image 20250203113627.png]]
		* Tem como função fazer o **Controle de Fluxo**, que é o processo de não enviar mais pacotes do que a Camada de aplicação é capaz de processar.
		* O tamanho da janela de recepção do TCP muda ao longo da duração da conexão. Ela é inicializada com um valor ínfero e vai crescendo gradativamente até que seja detectado um congestionamento na rede.
		* O **[[Controle de Congestionamento]]** do **[[TCP]]** pode ser realizado por dois algoritmos distintos:
			* **TCP AIMD:** Começa com uma janela pequena e vai aumentando a medida que recebe pacotes de confirmação. Se o tempo estourar o **TCP** seta a janela para o tamanho inicial da conexão e volta a crescer gradativamente.
				![[Pasted image 20250214084149.png]]
			* **TCP CUBIC:** Começa com uma janela pequena e vai aumentando a medida que recebe pacotes de confirmação. Se o tempo estourar o **TCP** seta a janela para a metade do tamanho atual e faz o envio de pacotes exponencial até chegar no limite anterior.
				![[Pasted image 20250214084118.png]]
		* O **3-ways Handshake** é o processo de desconexão de sessão em que o cliente envia um segmento de controle FIN para o servidor e o servidor retorna um ACK, seguido pelo servidor enviando um segmento de controle FIN para o cliente e o cliente retorna um ACK.
	* **[[UDP]]**
		* O **tamanho mínimo** do cabeçalho é **8 bytes**.
		* Adota um mesmo socket para todas as mensagens recebidas da camada superior.
		* Uma versão super simplificada do **TCP** responsável unicamente por gerenciar os segmentos e realizar a verificação de erros.
* Ambos os protocolos adotam o sistema de verificação de erros **Checksum**, que realiza a soma do tamanho do cabeçalho com o tamanho do IP e faz o seu complemento.
* A **[[Demultiplexação]]** é a tarefa de entregar os dados contidos em um segmento da camada de transporte à porta correta.
*  A **[[Multiplexação]]** é a tarefa transformar uma mensagem recebida pela camada superior em um segmento e envia-lo a camada inferior.
* O **[[round trip time (RTT)]]** é o tempo que um segmento leva para chegar ao host de destino e o host de origem receber o **ACK**. Ele pode ser calculado como n(L/R) + n(K/R) + 2n(D/C) + T_p
	* L = Tamanho em bits do pacote
	* R = Taxa de Transmissão do enlace (bps)
	* K = Tamanho em bits do ACK
	* D = Distancia física do enlace
	* C = Velocidade de propagação do enlace
	* T_p = tempo total de processamento dos pacotes e ACKs.]
	* n = quantidade de segmentos enviados na rede.
* O **[[RDT]]** foi a forma implementada **Camada de Transporte** para conseguir estabelecer um controle do envio e recepção de pacotes pela rede. Todas as versões do **[[RDT]]** utilizam Checksum, Número de sequencia do segmento e Timer. Atualmente, são adotadas 3 versões de **[[RDT]]**
	* **Stop-and-Wait**
		A utilização é dada por: (L/R)/RTT + (L/R)
		* Emissor:
			![[Pasted image 20250203094424.png]]
		* Receptor:
			![[Pasted image 20250213183811.png]]
	* **Go-Back-N**
		A utilização é dada por: N(L/R)/RTT + (L/R)
		Essa versão adota um sistema de Janela deslizante.
		* Emissor:
			![[Pasted image 20250203102129.png]]
		* Receptor:
			![[Pasted image 20250203102452.png]]
	* **Selective Repeat**
		Igual ao **Go-Back-N** só que agora o receptor passa a também ter uma janela deslizante.
* O número de sequencia dos pacotes é calculado como sendo o número de sequencia do segmento anterior + o tamanho bytes do segmento.
# [[Camada de Rede]]
* Algumas das funções da **Camada de Rede** incluem:
	* O roteamento (protocolos para a seleção e divulgação de caminhos fim-a-fim).
	- O encaminhamento de Datagramas (tabela de encaminhamento nos roteadores ).
	- A definição do formato dos Datagramas (protocolo IP).
	- A definição de um esquema de endereçamento (protocolo IP).
	- A definição de tratamento dos Datagramas (protocolo IP).
	- A reportação de erros (protocolo ICMP).
	- A troca de informações de controle (protocolo ICMP).
* O cabeçalho de um Datagrama IPv4 possui comprimento do cabeçalho, tipo de serviço, comprimento do datagrama e endereços IPs de origem e destino.
* A **Camada de Rede** possui duas categorias distintas, a categoria do **[[Plano de Dados]]** e a categoria do **[[Plano de Controle]]**.
	* O **[[Plano de Dados]]** trata-se das ações locais em cada roteador, incluindo a função de encaminhamento de Datagramas de uma interface de entrada para uma interface de saída.
	* O **[[Plano de Controle]]** trata-se da lógica de controle de toda a rede, incluindo a função de roteamento de Datagramas do hospedeiro de origem até o hospedeiro de destino, através da definição de um caminho fim-a-fim.
* O protocolo DHCP trata-se de um protocolo da [[Camada de Aplicação]] utilizado para a configuração automática da rede de hospedeiros.
	* Ele oferece o IP do roteador de borda da rede, nome e IP dos servidores DNS e a máscara da subrede. 
	* O processo de requisição de IP é feito por meio de troca de datagramas utilizando endereçamento Broadcast (255.255.255.255).
	* Permite que o host possa manter um mesmo endereço IP.
	* Realiza a alocação dinâmica de IP a um host.
* O processo de aquisição de um Bloco de [[endereço IP]] consiste em:
	* Fornecimento pela ISP responsável.
	* Ele pode ser quebrado em sub-blocos menores.
	* O ISP pode divulgar o seu bloco na internet de forma apropriada. Além de permitir a divulgação de prefixos mais longos, permitindo o endereçamento adequado na Internet.
* Uma aplicação NAT ([[Endereço IP]]), é um método de conversão de Datagramas IPs irreconhecíveis pela internet para datagramas IPs reconhecíveis:
	* Elas oferecem a opção de utilização de redes privadas em uma subrede.
	* Todos os hosts por traz de um NAT são identificados pelo mesmo endereço IP do roteador de borda publico.
	* As aplicações NAT realizam o processo de tradução de datagramas que entram e saem da rede privada por meio de uma tabela de tradução.
* Os **Middleboxes** são caixas intermediarias que desempenham papeis diversos além dos padrões realizados pelos roteadores, é o caso por exemplo dos  NAT, Firewalls, balanceadores de carga.
* A principal função da Internet é:
	* Estabelecer a conectividade de forma simples.
	* Utilizar de um único protocolo (IP) para o transporte dos pacotes entre a origem e o destino.
	* Deixar que a complexidade da rede fique na borda. 

### **Plano de Dados:**
* Serviços que poderiam ser fornecidos aos datagramas:
	Garantia de entrega;
	Garantia de retardo na entrega.
* Serviços que poderiam ser fornecidos a um fluxo de Datagramas:
	Entrega de datagramas em ordem;
	Garantia de vazão mínima;
	Restrição na variação do tempo entre Datagramas;
	Serviços fornecidos a cada datagrama individualmente.
* Mecanismos da Internet que podem mitigar limitações do serviço de melhor esforço:
	Superdimensionamento de banda no núcleo da rede;
	Distribuição de conteúdo próximo das redes de acesso;
	Controle de congestionamento nos sistemas finais.
* As interfaces de entrada nos roteadores possuem Módulo de terminação de linha, Módulo de Camada de Enlace, Módulo de pesquisa de endereçamento e encaminhamento, Módulo de Camada de Transporte, Módulo de sinalização.
* O processo de encaminhamento de datagramas envolve o uso de uma tabela de encaminhamento, Avaliação da correspondência entre o endereço de destino e o prefixo mais longo existente na tabela de encaminhamento, Fila de entrada para armazenar os Datagramas que aguardam processamento. 
* Os roteadores podem possuir 5 tipos de matrizes de comutação:
	Baseadas em memória;
	Baseadas em barramento;
	Baseadas em redes de interconexão;
	Baseadas em filas paralelas;
	Baseadas em descarte.
* Na interface de saída dos roteadores, acontece o processo de gerenciamento da fila que envolve Políticas de descarte/marcação de Datagramas, Políticas de escalonamento de Datagramas, Escalonamento baseado em classes, Pesquisa na tabela de encaminhamento e Checagem da integridade do Datagrama.

### **Plano de Controle:**
 * Os protocolos de [[Roteamento]] tem como principal função definir rotas entre hospedeiro emissor e hospedeiro receptor além da sua divulgação para todos os roteadores da rede. Além disso, são classificados quanto a visão do estado da rede, podendo ser global (**Link-State**) ou Decentralizada (**Distance Vector**). Além de trocarem informações entre roteadores com uma certa periodicidade para ver o estado do enlace.
 * Existem duas abordagens adotadas no plano de controle:
	 * **Protocolo de Roteamento de Estado de Enlace:**
		 Possui uma visão completa do estado da rede em todos os roteadores;
		 Realiza a difusão (Datagrama IP pela rede requisitando o estado dos roteadores e dos links) do estado da rede para obter uma visão centralizada.
		 Como principal abordagem, adota o **Algoritmo de Dijkstra** que realiza o calculo dos caminhos de menor custo a partir do roteador de origem.
		 Tanto o **Algoritmo de Dijkstra** quanto o **algoritmos de difusão de mensagens de estado da rede** tem como complexidade **O(n²)**.
	* **Protocolo de Roteamento Vetor de distâncias:**
		* Adota como principal algoritmo a **equação de Bellman-Ford** ( Dx(y) = minv { cx,v + Dv(y) }).
		* O funcionamento do Algoritmo se dá da seguinte forma:
			* Um nó aguarda que ocorra uma alteração nos custos de seus enlaces ou à chegada de uma mensagem de um nó vizinho, para iniciar uma nova computação de seu **Vetor de Distâncias**.
			* Em um nó na rede, um novo **Vetor de Distâncias (VD)** poderá ser computado a partir das informações locais e/ou do VD recebido de um nó vizinho.
			* Em um nó na rede, se após uma etapa de computação o Vetor de Distâncias tiver sido modificado para qualquer destino, o nó divulga o novo Vetor de Distâncias para os seus vizinhos.
* Como uma forma de tornar a internet mais escalável é hierarquizando a rede ([[Camada de Rede]]):
	* A Rede passa a ter **Sistemas Autônomos (Domínios)** com conjuntos de roteadores, que são dividindo em Roteadores de borda de AS e roteadores de núcleo.
	* Cada **Sistema Autônomo** pode utilizar protocolos diferentes.
	* O roteamento na Internet está organizado em uma hierárquia de duas camadas:
		- Roteamento **intra-Sistema Autônomo** (dentro dos domínios):
			- Possui 3 diferentes protocolos: RIP, EIGRP e [[OSPF]], sendo o último o mais adotado.
		- Roteamento **inter-Sistemas Autônomos** (entre os domínios).:
			- O [[BGP]] é o protocolo padrão adotado na internet. As decisões de roteamento são baseadas em específicas. Este protocolo é dividido em **eBGP** (comunicação entre Sistemas Autônomos diferentes) e **iBGP** (comunicação entre roteadores de um mesmo Sistema Autônomo).
- O [[ICMP]] é o protocolo padrão adotado pela internet para comunicar informações sobre o estado da rede. As informações são transportadas em Datagramas IP e cada mensagem pode conter Tipo da mensagem, Código (subtipo) da mensagem, Os primeiros 8 bytes do Datagrama que causou o erro.