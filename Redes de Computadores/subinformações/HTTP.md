O funcionamento do HTTP é baseado em requisição e resposta, e o funcionamento se deve em possuir um servidor como o Apache que armazena todos os objetos de uma pagina web, assim um dispositivo que queira acessar um determinado site envia uma requisição para o servidor apache e este estabelece uma conexão e envia os objetos para o usuário. 
As primeiras versões do HTTP trabalhavam com o protocolo [[TCP]] e utilizam exclusivamente a porta 80 para estabelecer a conexão.
O HTTP é um protocolo "stateless", ou seja, o servidor não mantem o status da conexão.
O protocolo possui dois tipos:
* Não persistente:
	O TPC estabelece uma conexão;
	O servidor envia o objeto requisitado;
	A conexão é fechada.
	É preciso estabelecer uma nova conexão para cada objeto solicitado.
	O tempo de resposta: 2RTT + o tempo de envia do arquivo
* Persistente:
	O TCP estabelece uma conexão;
	Enquanto a conexão estiver aberta, o Servidor envia quantos objetos tiverem sido solicitados
	A conexão é fechada
	O tempo de resposta: 2RTT + o tempo de envio de cada arquivo

O HTTP já passou por algumas atualizações:
* HTTP1.1:
	usava vários pipelines em uma mesma conexão TCP e era capaz de solicitar vários objetos em um único RTT.
* HTTO/2:
	Introduziu o método de enviar os cabeçalhos, no caso, ele envia os objetos requisitados de forma dividida, dividindo grandes objetos em pequenos pacotes e enviando ele junto com outros pacotes de objetos menores. Assim normalmente é carregado primeiro objetos menores em um site e apenas o inicio dos objetos maiores.
* HTTP/3 (Mais recente):
	Introduziu sistemas de segurança e um controle maior de erros e congestão na própria aplicação. Assim, o HTTP passou a utilizar o modelo de conexão UDP, deixando o processo de requisição e resposta mais rápido e fazendo com que a aplicação que implemente os processos de controle antes realizados pelo TCP.