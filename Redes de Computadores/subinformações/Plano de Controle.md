O plano de controle trata-se de uma visão geral da rede, que envolve o controle da rede. Ele está ligado a determinação do [[Roteamento]] do pacote.
A sua implementação adota duas abordagens:
* **Abordagem tradicional (pear-route):** É uma abordagem distribuída em que cada roteador possui o seu próprio plano de controle.
	![[Pasted image 20250204093544.png]]
* ***Abordagem definida em Software (SDN):** Em que haja uma mecanismo, dispositivo, centralizado de controle das rotas.
	![[Pasted image 20250204093610.png]]

O protocolo de roteamento tem como maior objetivo determinar "bons" caminhos entre os hosts. Esses "bons" caminhos podem variar de acordo com o custo, a velocidade, o tempo de congestionamento.

A construção de algoritmos de roteamento, pode variar do tipo de roteamento que será feito. Em uma abordagem global, os roteadores tem acesso completo a topologia da rede, neste caso o algoritmo de seguirá o estado de enlace.
Já em um modelo descentralizado, o algoritmo implementado seguirá um modelo interativo, Ou seja, o roteador troca informações com seus vizinhos para saber o estado do roteador e redefinir a sua tabela de roteamento.

### Estado de enlace (Link-state)
Neste estado, o algoritmo mais utilizado é o **Algoritmo de Dijkstra**,  que se trata de um algoritmo interativo para realizar o calculo do plano de controle de cada roteador:

*  Cx,y: Informa o custo do link que conecta o roteador x ao roteador y.
* D(v): Atual estimativa de custo do menor caminho da origem até um roteador v.
* p(v): Armazena o nó que vem antes do nó v no caminho de menor custo.
* N´: Conjunto de nós que o algoritmo já sabe o menor custo.
![[Pasted image 20250210094125.png]]

***Exemplo:*** ![[Pasted image 20250210094226.png]]
Neste modelo, cada roteador executa o seu próprio **Algoritmo de Dijkstra** e encontra o menor caminho para todos os roteadores da Rede. Este algoritmo é mais eficiente em redes grandes, uma vez que cada roteador elabora a sua própria tabela de roteamento individualmente.
Por um lado negativo, caso o peso da aresta se modifique, o algoritmo deverá ser executado novamente, fazendo com que haja uma constante oscilação dos caminhos, o que não é algo benéfico para uma rede de alta demanda.

### Vetor de distancia
Este algoritmo é baseado na equação de **Bellman-Ford**. Trata-se de uma equação que depende dos demais nós para ser encontrar o caminho mais rápido. Em suma, a equação calcula o menor custo do enlace de x até v somado ao custo de v até Y, sendo y o nó vizinho.
![[Pasted image 20250210164005.png]]
Exemplo:
![[Pasted image 20250210164130.png]]
![[Pasted image 20250210164205.png]]

Assim, esse processo vai se repetindo até encontrar que a tabela de roteamento de todos os roteadores sejam completamente preenchidos.
Ou seja, a medida que o tempo passa, vai encontrando novos caminhos para cada um dos roteadores até que no final todos os menores caminhos são descobertos.
![[Pasted image 20250210164632.png]]

