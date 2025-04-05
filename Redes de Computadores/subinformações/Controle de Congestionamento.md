	O Congestionamento consiste em o buffer dos roteadores estarem estar lotado, causando uma perda de pacotes ou atraso na emissão de pacotes.

Existem dois cenários que podem causar o congestionamento:
* Em um roteador ideal que possui buffer infinito, o problema do congestionamento será justamente a demora no processamento da informação pelo roteamento. Isso faz com que o Delay aumente exponencialmente ao ponto de o tempo de espera ser infinito.
	![[Pasted image 20250203121948.png]]

* Em um roteador mais real, em que o roteador possui um buffer finito, neste caso, o emissor ele irá retransmitir o pacote perdido, quando der timout enchendo cada vez mais o buffer do roteador e prolongando a congestão
	![[Pasted image 20250203122253.png]]
	Como acontecem essas retransmissões, não será alcançado um rendimento máximo. Ou seja, a rede passará a ser utilizada de forma desnecessária causando uma congestão ainda maior.
	Caso os pacotes duplicados sejam enviados e ambos cheguem ao receptor, esse rendimento vai ser ainda menor, e o emissor continuará utilizando a rede de forma desnecessária, uma vez que um dos pacotes automaticamente será destacado pelo receptor.
	Esses desperdícios de rede causam um maior trabalho na retransmissão, e uma subutilização da rede.

 Consequências do congestionamento na rede são:
 * O aumento do atraso de transmissão a medida que a transmissão se aproxima da capacidade disponível no enlace
 * O rendimento da rede é prejudicado com esses excessos de pacotes duplicados/retransmissões
 * Duplicados desnecessário também impactaram diretamente a eficiente da rendimento da rede
 * A capacidade do buffet de armazenamento será desperdiçada para pacotes duplicados.

Algumas formas de evitar o congestionamento da rede são:
* O Controle de congestionamento Fim-a-fim, realizado por protocolos como o TCP. Isso consiste em observar que está acontecendo perdas e atrasos na rede, fazendo assim uma diminuição da quantidade de pacotes enviados(diminuição do tamanho da janela) na rede para diminuir esse congestionamento.