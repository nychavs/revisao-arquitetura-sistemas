# Aquitetura de Sistemas
## Arquitetura Hexagonal
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

### Vantagens:

### Codigo:


## Arquitetura Clean
A arquitetura clena é bem parecida com a hexagonal na teoria, então tem como objetivo ser coesa, reuso de codigo e independencia de tecnologias externas e testabilidade. diferente do hexagonal que era dividido em portas e adaptadores (ou dominio e relacionadas), a arquitetura clena possui 5 camadas: entidade, caso de uso, adaptadores e frameworks externos. As classes centrais (entidade e casos) são mais estáveis - menos mudanças.
<br></br>
Entidades: são classes comuns a vários sistemas da empresa, além de dados podem implementar regras de negocio genericas
> Por exemplo, um sistema de uma universidade a todo momento tem que lidar com classes e dados de alunos e professores, esses ficariam nessa camada <br>
> Uma regra de negocio que poderia ser aplicada é que todos os professores devem pertencer à um departamento<br></br>

Casos de uso: a camada caso de uso implementa regras de negocio mais especificas.
> Por exemplo, um aluno só pode cursar matéria x se tiver passado em matéria y<br></br>

Adaptadores: tem a função de mediar as interações entre a camada mais externa (frameworks externos) e as do centro (caso e entidade), bem parecida com a do hexagonal, ela deve receber interações externas e encaminhar para o caso de uso correto e receber os resultados internos e encaminhar ao framework externo correto
> Por exemplo, converter determinada info do caso de em JSON e retornar ao endpoint correto<br></br>

frameworks externos: são bibliotecas, frameworks e qualquer sistema externo responsável pela persistência no banco de dados, construção de interfaces, provedores de pagamento envio de email etc<br></br>

### regra de dependencia
garante que as classe entidade e caso de uso são livres de qualquer dependencia com os frameworks externos, então pode ter nenhuma instancia ou conhecimento de classes externas, seja variavel, método ou classe.

### fluco de controle

### vantagens
