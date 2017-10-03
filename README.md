# Machine Learning CPS863
Resoluções de desafios propostos na matéria propostos na matéria "Machine Learning" (CPS 863) 2017.3, UFRJ - PESC, pelos professores Edmundo de Souza e Silva e Rosa Leão.

# Markov Reward

Desafio:

I - Uma loja mantém um estoque de um determinado produto A para venda. Suponha (para manter o
problema com pequeno tamanho para ser fácil a solução manual) que a loja aloca espaço para que, no
máximo, 2 itens possam ser mantidos no estoque por semana. A loja não abre no sábado e domingo,
e esses dias são feitos para repor o estoque de acordo com o pedido feito no início da semana. Em
outras palavras, antes da loja abrir na segunda feira, o gerente deve solicitar produtos para repor o
estoque. Durante a semana, clientes podem ou não comprar produtos, mas se não houver nenhum
produto na prateleira, obviamente nada é vendido, e a loja perde a potencial venda. O gerente, no
início da semana, tem que decidir quantos itens solicitar para a fábrica, mas baseia sua decisão apenas
no número de itens na prateleira na segunda feira (antes da loja abrir) o que inclui o estoque anterior
e mais os itens recebidos de fábrica no fim de semana. O gerente não mantém nenhum registro de
clientes que procuraram o produto mas nao o encontraram na prateleira.

  A cada semana, com probabilidade p_0 , nenhum cliente chega semana para comprar o produto A, e
com probabilidade p_1 (p_2 ) 1 (ou, respectivamente, 2) cliente(s) chega(m) para comprar A. O gerente
usa uma política simples de reposiçao: ele faz uma requisição de 2 produtos caso, no inicio da semana,
o estoque esteja vazio. (Note que os 2 itens só são entregues no fim de semana, e não poderão ser
usados para atender possíveis clientes na semana.)


a) Desenhe a cadeia de Markov com o objetivo de determinar o número médio de produtos na
prateleira no início de dada semana.

b) Suponha que p_0 = 0.25, p_1 = 0.6 (logo p_2 = 0.15). Calcule o número médio de clientes que n ̃
ao
encontram A na prateleira.

c) Suponha que o custo de A na fábrica seja de R$ 40.00, a loja ganha R$ 100.00 por item vendido
e ainda que o custo de entrega seja de R$ 20.00 pelos 2 itens. Calcule o ganho esperado da loja
por semana, se A é vendido a R$ 100.00,




II - Seja "a_i" a ação que indica solicitaçao de i produtos do tipo A. O gerente quer melhorar o seu ganho
e para isso ele pensa na possibilidade de realizar as seguintes ações:

• Caso o número de produtos na prateleira seja de igual a 1 no início da semana, ele pode tomar
as ações a_0 ou a_1 .

• Caso o número de produtos na prateleira seja de igual a 0, ele pode tomar as ações a_1 ou a_2 .
Suponha ainda que o custo de entrega de 1 item apenas seja de R$ 15.00, e de 2 itens continue sendo
de R$ 20.00.


d) Escolha as ações que maximizam o ganho médio da loja por semana, a longo prazo.

e) Suponha que, na primeira semana de funcionamento da loja após um período de recesso, a loja
esteja com apenas 1 item na prateleira no início da semana. Pense em como escolher a sequˆencia
de ações a tomar a cada semana para maximizar o lucro médio total nas 3 primeiras semanas
de funcionamento. Tente formalizar o problema e explique o que pode ser feito para chegar a
solução. Tente calcular o lucro médio.

# Resolução:

Primeiramente, para facilitar a comprienção, acho que vale a pena realizar um exemplo. Por isso contruir uma trabela que apresenta três semanas (geradas arbitráriamente) e que apresentam as principais mecânicas que regem esse exercício. 



DIA          | ESTOQUE | CLIENTES | PEDIDOS  
---------    | ------- | -------- | --------
SEGUNDA      |    2    |    0     |    0
TERÇA        |    2    |    0     |    0
QUARTA       |    2    |    0     |    0
QUINTA       |    1    |    1     |    0
SEXTA        |    1    |    0     |    0
SÁBADO       |    1    |    0     |    0
DOMIGO       |    1    |    0     |    0
SEGUNDA      |    1    |    0     |    0
TERÇA        |    1    |    0     |    0
QUARTA       |    0    |    1     |    0
QUINTA       |    0    |    0     |    0
SEXTA        |    0    |    0     |    0
SÁBADO       |    0    |    0     |    0
DOMIGO       |    0    |    0     |    0
SEGUNDA      |    0    |    0     |    2
TERÇA        |    0    |    0     |    0
QUARTA       |    0    |    0     |    0
QUINTA       |    0    |    0     |    0
SEXTA        |    0    |    2     |    0
SÁBADO       |    0    |    0     |    0
DOMIGO       |    0    |    0     |    0
SEGUNDA      |    2    |    0     |    0

A pesar do enuciado não deixar claro como funciona a dinamica da loja dia-a-dia (por adotar como unidade de templo ciclos semanais), nesse exemplo todos os dias de três semanas (a partir da data inicial) foram representados para facilitar a comprienção do leitor. Dessa meneira é possivel ver a cada dia o nível do estoque, o número de clientes e número de pedidos realizados. No exemplo acima, o nivel de estoque inicial era de duas unidades do produto "A". Durante a primeira semana uma unidade foi comprada, durante a segunda semana outra unidade foi comprada, de maneira que no início da terceira semana o estoque chegou a zero e o gerente fez o pedido de mais duas unidades. Na terceira semana dois novos clientes vieram a loja, mas não puderem comprar nada por que não havia estoque. Por fim, na ultima segunda feira represetada chegaram as duas unidades encomendadas uma semana antes. A tabela a seguir resume todo o exemplo mostrando o resultado semana a semana:


SEMANA       | ESTOQUE | CLIENTES | PEDIDOS  
---------    | ------- | -------- | --------
S1           |    2    |    1     |    0
S2           |    1    |    1     |    0
S3           |    0    |    2     |    2
S4 (SEGUNDA) |    2    |    0     |    0


Essa representação, mesmo sendo mais compacta, pode gerar confunções por aglutinar numa mesma linha resultados que não ocorrem simultaneamente. De toda maneira, esse exemplo apresenta as principais caracteristicas do problema.


Uma vez familirizados com o problema podemos propor um modelo de rede de markov que capture a natureza da situação, a imagem a seguir apresenta o modelo proposto:

