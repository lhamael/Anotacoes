O **Border Gateway Protocol** (BGP) é o protocolo mais utilizado na internet. Ele permite que as subredes sejam divulgadas na Internet, assim caso uma nova subrede surja na internet, ele quem desempenha esse papel de divulga-la e unifica-la com as demais subredes. O BGP possui duas funções principais.
* **eBGP**: obter as informações de subnets que podem ser alcançaveis a partir de sistemas autônomos vizinhos. Com ele os sistemas autônomos conseguem divulgar a lista de roteadores alcançáveis.
* **iBGP**: propaga a informação de alcançabilidade a todos os roteadores dentro de um mesmo AS.

![[Pasted image 20250211154636.png]]

O Funcionamento do BGP se dá por meio de uma sessão BGP, em que dois roteadores que conseguem falar BGP são capazes de se comunicar informando tanto o seu **iBGP** quanto o **eBGP**. Essa comunicação é feita por meio de [[TCP]]. Se por exemplo uma nova subrede se conecta a rede, o roteador de borda da rede divulga a existência dessa nova subrede informando que ele é alcançável, mas não a rota.
![[Pasted image 20250211155227.png]]
