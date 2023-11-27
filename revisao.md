# Aquitetura de Sistemas
## Arquitetura Hexagonal
Criar sistemas que sejam desacoplados tanto do banco de dados quanto da interface do usuário, 
dessa forma o sistema conseue operar quando o banco de dados nao esta presente e consegue se conectar 
mais facilmente à outras aplicações. <br> Esse acoplamento reduzido facilita a construção de teste de regressão e os processos de integração.
toda vez que alguma informação chega na apicação/porta, essa porta traduz a interação do ambiente externo para 
o que a plicação reconhece, e quando ela precisa interagir com o ambiente externo ela faz isso atraves dos adaptadores
Por isso um apelido para essa arquitetura é portas e adaptadores. <br></br>

Resumindo: inputs via porta, interações via adpatadores. Portas -> dominio, relacionadas -> adaptadores<br>
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
desacoplamento, testabilidade, flexibilidade, manutentabilidade e independencia da tecnologia

### Desvantagens:
adicionar complexidade inicial ao porjeto e se o projeto for grande pode ficar lento

### Codigo:
dominio pizza:<br>
package dominio;<br>
public class Pizza{<br>
  private string nome<br>
  private string[] sabores<br>
}<br>
getters e setters<br></br>

porta:<br>
package ports;<br>
import dominio<br>
public interface Pizzaservice{<br>
  public void createPizza(Pizza pizza)<br>
  public Pizza getPizza(string nome)<br>
  public list<Pizza>listPizza();<br>
}<br></br>

porta repositorio<br>
package ports<br>
import dominio<br>
public interface PizzaRepo{<br>
  public void createPizza(Pizza pizza)<br>
  public Pizza getPizza(string nome)<br>
  public list<Pizza>listPizza();<br>
}<br></br>

adaptador<br>
package adpaters<br>
import dominio<br>
import ports <br>
public class PizzaController implements PizzaService{<br>
  private PizzaRepo pizzaRepo;<br>
  public PizzaController(PizzaRepo pizzaRepo){<br>
    this.pizzaRepo = pizzaRepo;
  }<br>
  @override<br>
  os metodos q colocamos na interface<br>
}<br></br>

implementação da repo pro bd<br>
package adapters<br>
import dominio;<br>
import ports;<br>
public class PizzaRepoImpl implements PizzaRepo {<br>
 private Map<String, Pizza> pizzaStore = new HashMap<String, Pizza>()<br>
dps os override, exemplo: <br>
@Override<br>
 public void createPizza(Pizza pizza) {<br>
 pizzaStore.put(pizza.getName(), pizza);<br>
 }<br></br>

 execução:<br>
 package ports<br>
 import adapters<br>
 public class PizzaServiceFactory {<br>
 public static PizzaService createPizzaService() {<br>
 PizzaRepo pr = new PizzaRepoImpl();<br>
 PizzaService ps = new PizzaController(pr);<br>
 return ps;<br>
 }<br>
}<br>


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

### fluxo de controle

### vantagens
desacoplamento e independencia de framework, testabilidade, escalabilidade e manutentabilidade, reuso de codigo e adaptabilidade a mudanças de tecnologia

### desvantagem
desafiador para equipes iniciantes, necessidade de seguir o padrão bem especificamente 

## Desevolvimento baseado em componentes
Surgiu pensando no reuso desses componentes, pois os softwres foram ficando cada vez maiores então a ideia veio pra poder usar esse modulo/componentes em diversas partes do software. Nao usamos classes porque eles sao muito ligadas ao dominio que eles pertencem, ja os componentes são mais independentes/individuais/genericos, podendo atuar como provedores de serviço (ex: métodos)
- Criar componentes reusaveis e independentes
- Componentes devem possuir interface
Segundo Chessman e Daniel, o ponto de partida são: criação do modelo de negócio (diagrama de classe - sem operações) e modelo de caso de uso (diagrama de caso de uso e identificação dos fluxos) <br>
Passo 1:
- derivação do diagrama de classe, só que com type
- identificação do tipo core (as classes principais)
Passo 2:
- modificação do modelo para identificar as interfaces
- para cada classe core será criado um componente gerenciador especificando os metodos dela, então i + o nome da classe + Mgt
Passo 3:
- adição da notação pirulito
Passo 4:
- elaborar o diagrama de componentes
![image](https://github.com/nychavs/revisao-arquitetura-sistemas/assets/101810029/a13f2a90-d280-48ed-b4d8-8f7a793e6b67)
Necessita-se uma interface entre  camada de sistema e de apresentação (web)
Passo 5:
- criação dos mgr. cada mgt possui um mgr que é um componente responsavel pela persistencia de dados
Passo 6:
- criação dos DAO que são adaptadores em cada um dos mgr.
passo 7:
- criação dos value objects
![image](https://github.com/nychavs/revisao-arquitetura-sistemas/assets/101810029/1607ebf6-a7ea-4280-933b-47a9137459fb)

### codigo

### vantagens
reutilização de componentes, manutentabilidade, separação de responsabilidade e escabilidade

### desvantagens
complexidade de gerenciamento, padronização para garantir a interoperabilidade e sobrecarga de comunicação

## Desenvolvimento Orientado a Dominio (ddd domain driven development)
abordagem para sistemas de softwares complexos onde o foco está no dominio do sistema, desenvolvedores e especialistas no negocio devem trabalhar de forma colaborativa usando linguagem ubiqua (vocabulario compartilhado entre os devs e especialistas).<br>

> o dominio de um sistema consiste na area do sistema que ele pretende resolver. deve ser norteado para atender o seu dominio e nao se moldar a uma determinada tecnologia.<br>

linguagem ubiqua
> são usados para melhor comunicação e para nomear entidades do codigo do sistema, como classes, metodos, atributos etc<br>

ddd e arquitetura<br>
> podem ser usados arquitetura em camdas, clean ou hexagonal. evans indica a separação em 4 camadas:
> interface, aplicação, dominio e infraestrutura, onde a aplicação faz ponte com infra/modelo de dominio e a interface.<br>

objetos importantes de dominio:
> entidades, objetos de valor, serviços, agregados e repositórios.<br>
entidade: valores unicos exemplo: ID<br>
objetos de valor: valores nao unicos exemplo: CEP e nao devem possuir set<br>
entidades sao mais importantes que objetos de valor.<br>
serviços: tb gerenciados/controladores são operações importantes (exemplo: servicoDeEmprestimo)<br>
agregados: tb frabricas são coleções de entidades e objetos de valor, as vezes eles nao podem ser individuais, ele possui uma raiz que é a entidade e a raiz referencia os objetos do agregado. ex: emprestimo e itemEmprestimo (que possui usuario e livro)<br>
![image](https://github.com/nychavs/revisao-arquitetura-sistemas/assets/101810029/f18d685c-beae-4c0c-b842-9f4ec8ed7301)

repositorio: usado para recuperar outros objetos de dominio de um bando de dados, é onde cadastramos as informações, controlamos objetos ja cadastrados e buscamos informações neles.uma bastração para o banco de dados. surge da necessidade do cliente de guardar e obter objetos do dominio. APENAS entidades e agregados podem possuir repositorios

### vantagens
alinhado com o negocio, melhor comunicação entre devs e especialistas

### desvantagens
nao recomendado pra projetos simples, aumentar a complexidade inicial do projeto 

## soa
as arquiteturas orientadas a serviço (soa) são uma forma de desenvolvimento de sistemas distribuidos em que os componentes de sistemas sao serviços autonomos, executando em computadores geograficamente distribuidos.
> é uma abordagem arquitetural corporativa que permite a criação de serviços de negocio interoperaveis que podem ser reutilizados e compartilhados. <br>
> visbilidade, interação e efeitos. <br>
> exemplo: api da amzon product advertising api. <br>

principios dos serviços:
> reutilizaveis, compartilham contrato formal, possuem baixo acoplamento, abstraem a logica, capazes de compor, autonomos, evitam alocação de recursos por longos periodos de tempo e são capazes de serem descobertos.
![image](https://github.com/nychavs/revisao-arquitetura-sistemas/assets/101810029/1cb0506e-0b0b-460f-b0fa-68543e38dfd0)

### vantagens 
reusabilidade, manutentabilidade, interoperabilidade, escalabilidade

### desvantagens
complexidade inicial, overhead de comunicação, dependencia de padroes especificos

## Webservices
representação padrão para algum recurso computacional ou de informações que podem ser usadas em outros programas, sendo acessados via HTTP e sendo xml puro podemos acessar via get 
![image](https://github.com/nychavs/revisao-arquitetura-sistemas/assets/101810029/5ef49eca-59ed-41b3-9bc9-d16ae48f0b62)
Podemos usar o soap como padrão <br></br>
SOAP
> padrão de troca de mensagens que oferece suporte à comunicação entre os serviços. Ele define os componentes essenciais e opcionais das mensagens passadas entre serviços

### diferença entre restful e webservices
webservice é qualquer serviço disponibilizado atraves da internet e usa tanto http quanto xml e soap, ja restful é uma arquitetura que define um conjunto de principios e retrições para o design de sistema distribuidos, usa http e json.
### vantagens
interoperabilidadem reusabilidade, integração facilitada e manutentabilidade

### desvantagens
overhead de comunicação, complexidade de implementação, dependencia da rede
