A principal função dos roteadores é examinar os cabeçalhos dos datagramas(Pacotes enviados pela camada de rede), extrair a informação de destino do pacote e tomar a decisão para qual interface de saída este pacote será encaminhado.

A Arquitetura de um roteador comum, de forma genérica, pode ser dividida em duas partes, uma parte de [[Plano de Controle]] e roteamento, que está ligada ao processamento e roteamento do roteador, e outra parte relacionada ao [[Plano de Dados]] que é composta por [interfaces de entrada](#Interface de entrada) uma [fabrica/matriz de comutação](#Fabrica de Comutação) e uma interface de saída de dados.
![[Pasted image 20250204172734.png]]
O maior limitador do roteador normalmente está no Plano de Controle.
### Interface de entrada
A interface de entrada, possui três módulos que são o modulo da interface de enlace que tem a função de conectar o enlace a interface. O segundo modulo é da camada de enlace que executa os diversos protocolos, como o Ethernet. E então possui a camada de comutação decentralizada que realiza o enfileiramento dos pacotes no roteado.
![[Pasted image 20250204173225.png]]

### Fabrica de Comutação
Tem a função de transferir os pacotes recebidos na interface de entrada para a interface de saída roteando o caminho que o pacote deverá seguir segundo o plano de controle do roteador.
O processo de comutação dos pacotes é dividido em três tipos:
* Comutação com memória: em que os pacotes são armazenados em uma memória local e depois transferidos para a interface de saída. O problema desse modelo é que ele demora de mais para armazenar os pacotes na memória e depois disponibiliza-los na interface de saída.
	![[Pasted image 20250205141945.png]]
* Comutação por barramento: Em que todas as interfaces de entrada dividem um mesmo barramento. O problema desta abordagem é que ela precisa fechar as entradas e saídas inutilizadas para evitar o embaralhamento dos bits.
	![[Pasted image 20250205142009.png]]
* Comutação por interconexão: Que consiste em todas as interfaces de entrada e saída dividirem uma teia de conexões, permitindo que mais de um pacote seja retransmitido ao mesmo tempo.
	![[Pasted image 20250205142042.png]]

### Interface de saída
Um grande problema nos roteadores é o enfileiramento de pacotes na interface de saída. Isso se dá pois cada pacote deve ser enviado individualmente pela rede. Ou seja, em uma interface de saída, dois pacotes não podem sair ao mesmo tempo. Isso pode escalar a partir do momento que a taxa de chegada de pacotes em todas as interfaces de entrada for igual ou maior que a taxa de saída de cada interface de saída. Isso gerá um congestionamento no buffer de saída do roteador.
![[Pasted image 20250205163638.png]]
Para resolver esse problema, o ideal é pensar em um buffer de um tamanho médio que suportaria esse excesso de pacotes em uma mesma interface de saída. O cálculo do tamanho do buffer seria o tempo médio para o pacote ir e volta pela rede (RTT) vezes a capacidade do enlace dividido pela raiz quadrada do numero de fluxos que estão sendo transmitidos.
![[Pasted image 20250205164035.png]]
