# Algoritmo-Dijsktra
Trabalho da matéria de Teoria dos Grafos

1º passo: iniciam-se os valores:
para todo v ∈ V[G]
     d[v]← ∞ 
     π[v] ← -1
     d[s] ← 0
V[G] é o conjunto de vértices(v) que formam o Grafo G. d[v] é o vetor de distâncias de s até cada v. Admitindo-se a pior estimativa possível, o caminho infinito. π[v] identifica o vértice de onde se origina uma conexão até v de maneira a formar um caminho mínimo.

2º passo: temos que usar o conjunto Q, cujos vértices ainda não contém o custo do menor caminho d[v] determinado.
Q ← V[G]
3º passo: realizamos uma série de relaxamentos das arestas, de acordo com o código:
enquanto Q ≠ ø
         u ← extrair-mín(Q)                     //Q ← Q - {u}
         para cada v adjacente a u
              se d[v] > d[u] + w(u, v)          //relaxe (u, v)
                 então d[v] ← d[u] + w(u, v)
                       π[v] ← u
w(u, v) é o peso(weight) da aresta que vai de u a v.

u e v são vértices quaisquer e s é o vértice inicial. ffd extrair-mín(Q), pode usar um heap de mínimo ou uma lista de vértices onde se extrai o elemento u com menor valor d[u].

No final do algoritmo teremos o menor caminho entre s e qualquer outro vértice de G. O algoritmo leva tempo O(m + n log n) caso seja usado um heap de Fibonacci, O(m log n) caso seja usado um heap binário e O(n²) caso seja usado um vetor para armazenar Q.
