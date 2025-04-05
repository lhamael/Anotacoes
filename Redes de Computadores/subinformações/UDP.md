Apenas envia os pacotes pela rede, sem um controle de fluidez, um controle de congestionamento nem uma confiança no trasnporte de dados. Mas por ser um protocolo muito mais simples, ele garante uma comunicação muito mais rápida entre a origem e o destino.

Trata-se de um protocolo utilizado na camada de transporte do HTTP e ele funciona seguindo a ideia de melhor esforço, em que os segmentos podem ser entregues fora de ordem à aplicação. Trata-se de um protocolo não orientado a conexão e cada segmento é manipulado de forma independente. A sua principal utilidade é para trabalhar com protocolos não orientados a conexão, diferente do TCP. Além do fato de como ele é bem simples normalmente são segmentos menores do que no caso do TCP.
O **tamanho mínimo** do cabeçalho UDP é de **8 bytes**.

* Segmento do UDP
	![[Pasted image 20250203083102.png]]

Como é observável, o UDP possui um checksum que tem a função de detectar erros nos bits de um segmento transmitido.
![[Pasted image 20250203083309.png]]
No caso, o emissor soma os bits do cabeçalho e do endereço IP com 1 e esse valor é colocado no cabeçalho do checksum.
No caso do receptor, ele faz o mesmo processo, pega os bits do cabeçalho com os bits de IP e soma 1, e depois compara eles com o Checksum no pacote, se tiver alguma diferença, significará que o segmento foi corrompido no caminho.
