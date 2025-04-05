É um conjunto de números que vai identificar fisicamente onde o dispositivo se localiza. Cada comutador e roteador no mundo possuem um Endereço IP ao qual atrelam esse os dispositivos conectados a eles.
Exemplo:
	O IP da minha casa é: 192.168.0.29

O Endereço IP é identificar de INTERFACES com 32-bits. Ou seja, dispositivos com mais de uma interface podem ter mais de um IP.
[[Roteadores]] normalmente possuem mais de uma interface de rede, podendo ela ser física ou não.
O Endereço IP tipicamente possui um formato de 4 bolsões de 8 bits.
![[Pasted image 20250205165526.png]]
Um host estará na mesma rede se os 3 primeiro octetos forem iguais.
![[Pasted image 20250205165639.png]]
O Sistema que interliga os diferentes host a interfaces do roteados pode ser considerado como Subredes. Os endereços IPs possuem uma estrutura, sendo que os bits mais significativos é o endereço de Rede, e os bits menos significativos representam o endereçamento do dispositivo.
Além disso, existem casos em que uma mesma empresa não precisa necessariamente utilizar todo os endereços de IP disponíveis, para isso criou-se o conceito de subnet, em que além de dividir a rede em varias redes menores, ainda há uma divisão de um mesmo IP para multiplos dispositivos, assim adota-se o uso de um padrão similar a 
a.b.c.d/x
=
![[Pasted image 20250205170509.png]]
Assim, adota-se o concetio de mascara de subrede, em que um conjunto de bits será sempre o mesmo, podendo ser passar o valor 255 para mascarar a parte do IP padrão e 0 para revelar a parte do IP especifica.

Esse sistema de endereço é conhecido como CIDR, em que ele é especificado para um tipo de endereçamento não especificado em Classes.
![[Pasted image 20250205170855.png]]
### DHCP
Para um dispositivo ganhar um endereço IP, existe o Dynamic Host Configuration Protocol (DHCP), que se trata de um protocolo de configuração dinamica para hospedeiros, em que quando uma maquina entra em uma rede, essa recebe as configurações necessárias para estabelecer o próprio IP. Esse IP pode mudar ou não a cada vez que esse dispositivos entrar nesta rede. Isso permite em especial que dispositivos moveis, possam se conectar a diferentes redes sem precisarem passar por um processo de configuração manual a cada vez que ele entra em uma rede diferente.

O processo de resolução de IPs realizado pelo DHCP é dividido em 4 etapas:
	O próprio dispositivo final lança uma mensagem na rede consultando se existe algum servido DHCP na rede. Se existir, o servidor responde informando ao host. (Processo opcional)
	O Host faz uma requisição de endereço IP ao servidor DHCP
	O servidor DHCP responde a requisição enviando um endereço ao host.
	Exemplo: ![[Pasted image 20250210083009.png]]

Além do endereço IP do dispositivo, o servidor DHCP ainda envia o IP do servidor de borda da subrede, o nome e IP do servidor DNS e a mascara da subrede.

Falando em um âmbito mais geral, cada subrede recebe o seu endereço IP a partir  de um provedor de acesso a internet. Desta forma, o provedor de acesso a internet (ISP) irá bloquear uma parte do IP, que é o que representa todas as subredes daquele provedor, e disponibiliza apenas uma quantidade de bits que cada subrede poderá utilizar.

* IP bloqueado pelo ISP: ![[Pasted image 20250210083722.png]]
* IPs que poderão ser usados: ![[Pasted image 20250210083822.png]]

Normalmente usuários domésticos disponibilizam ainda menos bits para IPs.

### NAT
O Network Address Translation (NAT) é um protocolo que cria endereços IPs virtuais que não existem na Rede. Neste modelo, um host não consegue enviar pacotes pela rede de forma autônoma, uma vez que o IP que ele recebe não existe na rede. Desta forma, a função do roteador responsável por esse subrede é converter o IP do usuário em uma porta e enviar a requisição utilizando o IP do roteador que é um IP real. 
Esse protocolo é o mais utilizado em especial para disponibilização de IPs domésticos, uma vez que não existem tantos endereços IPs no mundo ao ponto de cada dispositivos poder utilizar um. Assim, um provedor de internet domestica cria uma subrede com IPs que não existem e quando é enviado ou recebido um pacote, o servidor realiza a conversão dos IPs por meio do protocolo NAT.

![[Pasted image 20250210085038.png]]

O funcionamento do NAT se dá da seguinte forma. Cada datagrama que for enviado por um host e tiver de sair da rede tem o seu IP modificado, assim, o datagrama passa a receber o mesmo IP valido da rede e o IP do host é convertido em uma porta. Essa porta é a representação do host na internet, sendo que apenas o servidor com NAT responsável pela subrede saberá a quem deverá enviar o datagrama de resposta. Quando um datagrama for recebido de fora da rede, o servidor central com NAT converte a porta em um endereço IP e porta específicos para aquele dispositivo que irá receber o datagrama.
![[Pasted image 20250210085732.png]]
