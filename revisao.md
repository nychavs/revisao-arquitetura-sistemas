# Aquitetura de Sistemas
### Arquitetura Hexagonal
Criar sistemas que sejam desacoplados tanto do banco de dados quanto da interface do usuário, 
dessa forma o sistema conseue operar quando o banco de dados nao esta presente e consegue se conectar 
mais facilmente à outras aplicações. <br> Esse acoplamento reduzido facilita a construção de teste de regressão e os processos de integração.
toda vez que alguma informação chega na apicação/porta, essa porta traduz a interação do ambiente externo para 
o que a plicação reconhece, e quando ela precisa interagir com o ambiente externo ela faz isso atraves dos adaptadores
Por isso um apelido para essa arquitetura é portas e adaptadores. <br></br>

Resumindo: inputs via porta, interações via adpatadores. Portas -> dominio, relacionadas -> adaptadores
Essa arquitetura favorece o reuso de codigo, alta coesão e baixo acoplamento<br>
Uma arquitetura hexagonal divide as classes de um sistema em dois grupos: dominio e relacionadas. <br>
> Classes de Dominio: diretamente relacionada com os negócios do sistema<br>
> Classes Relacionadas com infraestrutura, banco de dados, integração com sistemas externos<br></br>

As classes de dominio nao dependem das relacionadas, nao as conhecem e se elas mudarem o dominio não é afetado. <br>
> Dominio sempre dentro no dentro do hexagono e relacionadas/adaptadores sempre por fora do hexagono. <br>
> Os adaptadores recebem chamadas de metodos externos (por ex: interface de usuario) e encaminham esses métodos
para as portas.<br></br>

Quando recebem chamdas de metodos internos encaminham para os serviço externo (ex: banco de dados)<br>
> Exemplo de classe dominio: classe livro com os atributos de nome, autora etc <br>
> Exemplo de classe adaptadora: classe livro controller que cria livros, atualiza etc...<br>

Vantagens:

Codigo:
