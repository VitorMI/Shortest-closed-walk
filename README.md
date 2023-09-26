# Shortest-closed-walk

Codigo desenvolvido durante a aula 18 da cadeira Teoria dos Grafos - Ufcg

## Chinese Postman Problem 

Alguns problemas como o Chinese Postman Problem demandam a definição de um passeio fechado de menor custo que inicia e termina em um mesmo vértice e percorre todas as arestas de um grafo. Não se trata da definição de um circuito de Euler, visto que arestas podem ser repetidas. No entanto, algoritmos para encontrar um circuito de Euler são comumente utilizados para auxiliar na definição deste passeio.

Implemente a função shortest_closed_walk que recebe como entrada um grafo ponderado não-direcionado G e um vértice init deste grafo e retorna um passeio fechado de menor custo que passa por todas as arestas, iniciando e terminando em init. Assuma que G é uma instância da classe Graph.

Para implementar esta função, sugerimos utilizar a seguinte heurística:

Crie uma cópia de G como um multigrafo:

> **M = nx.Multigraph(G)**

Se (x,y) é uma aresta de G, então M terá aresta (x,y,0), onde 0 é o identificador (key) da aresta.

Seja odd_nodes uma lista com os vértices de grau ímpar de M. Lembrando que em todo grafo, a quantidade de vértices de grau ímpar é um número par;

Enquanto odd_nodes não for vazia:

Encontre dois vértices x e y em odd_nodes cuja distância entre eles seja a menor possível (Dica: use a função shortest_path_length para calcular a distância entre dois os vértices);

Para cada aresta (u,v,0) no caminho entre x e y, adicione uma nova aresta (u,v,key) em M com o mesmo peso. Desta forma x e y passarão a ter grau par. O seguinte trecho pode ser usado para adicionar a aresta, com o mesmo peso da original.

  **M.add_edge(u, v, key)**
  **M[u][v][key]['weight'] = g.get_edge_data(u, v, 0)['weight']**
  **key += 1**

A variável key pode ser inicializada com 1 e incrementada sempre que cada aresta for adicionada;

**Remova x e y de odd_nodes.**

No grafo M resultante, que será um grafo par, execute a função eulerian_circuit passando init como parâmetro e retorne o circuito calculado.

Considere o grafo "s-u-w-cy-sc-p-05.graphml" no trecho de código abaixo. Este grafo não é Euleriano, pois possui dois vértices de grau ímpar, n0 e n4. O menor caminho entre estes dois vértices é (n0,n2,n4). Portanto, segundo a heurística proposta, as arestas (n0,n2) e (n2,n4) serão duplicadas no grafo cópia, para torná-lo par. A função deve retornar então o seguinte passeio, caso o vértice de origem seja n0:

**[('n0', 'n3'), ('n3', 'n4'), ('n4', 'n2'), ('n2', 'n0'), ('n0', 'n2'), ('n2', 'n4'), ('n4', 'n1'), ('n1', 'n0')]**
