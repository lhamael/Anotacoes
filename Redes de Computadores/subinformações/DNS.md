DNS ou Sistema de Nome de Domínios é um sistema que tem o intuito de atribuir a um domínio um nome, assim, ao invés do usuário ter que lembrar do IP, basta que ele lembre do Nome atribuído ao domínio e os servidores DNS farão o trabalho de identificar o IP associado.
Um dos serviços do DNS é o processo de tradução de nomes de domínio, apelidos do domínio como sites atrelados a outros domínios e apelidos de e-mail.

O processo de distribuição e hierarquização do DNS é dado pelo seguinte modelo:
![[Pasted image 20250107175917.png]]

Além disso, cada empresa pode ter o seu próprio servidor de DNS, como é o caso da Netflix ou até mesmo da própria UnB

Existem duas formas do servidor de DNS encontrar o IP associado ao domínio:
**Consulta iterativa:**
![[Pasted image 20250107181310.png]]
**Consulta recursiva:**
![[Pasted image 20250107181405.png]]
Normalmente é mais utilizado o modelo consultivo para não congestionar um único servidor DNS.
