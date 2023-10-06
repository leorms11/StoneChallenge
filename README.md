# StoneChallenge
Desafio do processo seletivo da Stone
--------------------------------------------------

Fala pessoal? Tranquilos?
Fiz esse READ.md para contar um pouco como foi a minha experiência desde que recebi o desafio, explicar algumas tomadas de decisões, tecnologias utilizadas e pontuar algumas melhorias (débitos técnicos) que ficaram no projeto, bom vamos lá!

Durante as duas semanas que tive para realizar a entrega, tive que conciliar as responsabilidades do trabalho e o prazo do desafio, ao meu ver isso foi um ponto que no fim acabou agragando para a experiência como um todo, pois precisei me organizar muito bem para entregar algo que me deixasse satisfeito. (Sorte que sei trabalhar sob pressão hehe).

## Sobre o desafio: 

Consistia em desenvolver uma solução (Api REST) para cada um dos 3 contextos solicitados (Ao lado de cada API está o link para acessar o repositório específico de cada uma):
  - API de Cliente (https://github.com/leorms11/StoneChallengeCustomerApi).
  - API de Cobranças (https://github.com/leorms11/StoneChallengeBillingApi).
  - API de Calculo de consumo (https://github.com/leorms11/StoneChallengeConsumptionApi).

## Estrutura do projeto:

Para entregar uma melhor solução levando em conta a proposta do desafio, desenvolvi três microsserviços (um para cada API) onde a base de dados é comum entre elas. Escolhi manter uma database padrão entre os MS pois se eu precisasse lidar com a replicação dos dados, politicas de retry, observabilidade acabaria não tendo tempo para entregar as demais coisas.
Com relação a arquitetua, utilizei a Vertical Architecture que consiste na separação em 4 camadas onde cada requisição ira trafegar entre elas. Escolhi essa arquitetura porquê é a que mais tenho familiaridade hoje em dia então foi mais fácil aplicar conceitos do DDD. (Confeço que no começo quase me propus o desafio de desenvolver utilizando a onion architecture).
Exemplo da arquitetura:
![image](https://github.com/leorms11/StoneChallenge/assets/104157485/3e15eb15-70a1-44ec-afbc-ca6c6761ef7f)

Durante o desenvolvimento consegui aplicar alguns conseitos como, Factory Pattern, Repository Pattern, Arrange Act and Assert (AAA) Pattern, Clean Code (Princípios SOLID) entre outros.

## Técnologias utilizadas:

Desenvolvi utilzando a versão 7 do .Net com o C# 11. 
Para os testes utilizei a lib xUnit, onde implementei testes unitário e de integração.
Com relaçã a persistência dos dados utilizei um banco PostgreSql hospedado na AWS.
Para a documentação automática dos endpoints utilizei a integração com o Swagger.
Comunicação síncrona entre os MS utilizando o REFIT.

## Desafios encontrados durante o desenvolvimento.

Pra mim, o maior desafio foi a implementação dos testes de integração, gastei bastante tempo estudando estrutura de projetos de testes até por fim escolher uma abordagem. Tive dificuldade na hora de implementar o WebApplicationFactory pois não sabia se seria uma boa prática apontar para a Classe Program do projeto web api ou se deveria criar uma nova dentro do projeto de teste, por fim acabei decidindo criar uma nova, porém acabou gerando alguns erros pois no coemço do desenvolvimento das soluções escolhei fazer a implementação da classe Startup no projeto (isso é um gosto pessoal meu, acho que fica mais dividido as responsabilidades) e a forma que o meu extension method construia a classe Startup quebrava só pelo fato de eu estar utilizando uma classe program que não estava dentro do projeto web api. Essa foi a minha dor de cabeça:
```
  var startup = Activator.CreateInstance(typeof(TStartup), builder.Configuration, builder.Environment) as IStartup
            ?? throw new ArgumentException("Erro ao tentar instanciar a classe Startup.cs.");
```
O segundo maior desafio foi o tempo, a dificuldade em conciliar as demandas oriundas do período do ano na empresa onde trabalho atualemente com o esforço necessário para entregar um bom resultado.  

## Melhorias a serem feitas. 

- Separar o banco de dados, atualmente os testes de integração estão utilizando o mesmo banco que os clientes finais.
- Refatoração no projeto de Teste.
- Sepapar o banco principal em 3 (1 para cada MS) e implementar mensageria aassíncrona. 
- Revisar os conceitos do DDD pois acabei ferindo alguns durante o desenvolvimento.

Se você encontrar mais algum ponto de melhoria, por favor me conte, pretendo evoluir esse projeto para fim de arpendizado :D
