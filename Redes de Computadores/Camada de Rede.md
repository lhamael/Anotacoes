A principal função da camada de rede, que em suma trabalha no núcleo da rede, é fornecer uma comunicação entre um Host de origem até um host de destino. Além disso, a camada de Rede é implementada em todos os dispositivos desde o dispositivo de origem até o dispositivo de destino.
Os [[Roteadores]] são um elemento essencial da camada de rede presentes no núcleo da internet e nas bordas da rede de acesso.
Para haver esse roteamento, a camada de rede implementa duas funções básicas, o [[Encaminhamento]] e o [[Roteamento]]. Além disso, a camada de rede pode ser organizada em dois planos bases, são eles os [[Plano de Dados]] e [[Plano de Controle]].
A internet, principal modelo de serviço de rede ofertado hoje, utiliza-se do conceito de melhor esforço, em que nenhum Datagrama terá prioridade sobre outro. Isso faz com que o núcleo da rede seja mais igual de forma a não prejudicar nem beneficiar nenhum pacote. Isso faz com que a internet seja um modelo simples e fácil de ser implementado que disponibiliza serviços muito mais rápidos do que outros modelos criados.

Ao receber da [[Camada de Transporte]] um segmento, esse segmento é transformado em um Datagrama:
![[Pasted image 20250205164939.png]]
Para a circulação desse protocolo pela rede, um dos recursos mais importantes é o [[Endereço IP]].

Na camada de Rede, existe um conceito que são as **Middleboxes**, que como o próprio nome sugere, se trata de uma caixa intermediaria que realizam funções distintas das realizadas por roteadores IPs no caminho de dados.
Exemplos: NAT, Firewalls, balanceamento de carga (realizado por corporações para evitar que diferentes níveis de hierarquia tenham acesso a partes da rede), servidores cache, entre outros.

Na camada de Rede, as duas principais funções executadas por todos os dispositivos são o [[Encaminhamento]], que é o processo de mover pacotes da entrar de um roteador para a saída apropriada, realizado pelo [[Plano de Dados]] e o [[Roteamento]], que é a determinação da rota que o pacote realizará no núcleo da rede, calculado pelo [[Plano de Controle]].

Na rede, um grande problema é a escalabilidade que a rede pode tomar. Fazer um plano de controle da rede completa para cada roteado não é muitas vezes o melhor cenário possível. Para resolver esse problema de definição de Planos de controle e organização da rede, à a definição de uma hierarquia da rede. Assim, começa a realizar a agregação de um conjunto de roteadores em **sistemas autônomos** ou **domínios**. Esses **AS** (Autonomous Systems) passa a ser divididos em dois sistemas:
* **Intra-AS:**
	Passa haver uma gestão dos roteadores que estão dentro de um mesmo AS.
	Neste modelo, todos os roteadores dentro de um mesmo AS devem executar um mesmo protocolo Intra-dominio.
	Passa a existir também um roteador de borda, que é responsável por comunicar-se tanto com a sua própria AS quanto com roteadores de borda de outros AS.

* **Inter-AS:**
	É o roteamento entre sistemas autônomos.
	Esse roteamento passa a ser executado pelos roteadores de borda de cada AS.

Neste modelo, dentro de cada AS executa-se um algoritmo especifico de definição de rotas internas. E quando precisa-se enviar um segmento entre os AS, os roteadores de borda enviam esses pacotes sem necessariamente saber o caminho que ele irá fazer no outro AS.
![[Pasted image 20250210170933.png]]

O protocolo mais utilizado para o roteamento intra-AS é o [[OSPF]].

Com a expansão exponencial das redes, uso cada vez maior da Internet, fez se necessário adotar uma nova estratégia de gerenciamento do plano de controle dos roteadores. Assim, surgiu o [[SDN]].