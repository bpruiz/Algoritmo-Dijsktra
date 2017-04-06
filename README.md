Aluno: Bruno Piovesan Ruiz    /   RA: T14404-3   /    DATA: 06/04/2017      / 
UNIP - Universidade Paulista    /
Disciplina: Teoria dos Grafos   /
Professora Mirla


# Algoritmo-Dijsktra
Trabalho da matéria de Teoria dos Grafos - Profª Mirla

Explicação do Algoritmo:

O algoritmo de Dijkstra, cujo nome se origina de seu inventor, o cientista da computação Edsger Dijkstra, soluciona o problema do caminho mais curto para um grafo dirigido com arestas de peso não negativo. 

Este algoritmo gera uma árvore de caminho mais curto (Shortest Path Tree – SPT). Essa árvore é uma sub-árvore do grafo que representa o caminho mais curto de qualquer nó na STP até o nó origem.

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


// O algoritmo Dijkstra pode ser aplicado quando há necessidade de encontrar o caminho de custo mínimo, como por exemplo uma rota da cidade de São Paulo até a cidade do Rio de Janeiro.



// Implementação do algoritmo de Dijkstra em C++
//Algoritmo verificará o caminho de custo mínimo de São Paulo ao Rio de Janeiro

#include <iostream>
#include <list>
#include <queue>
#define INFINITO 10000000

using namespace std;

class Grafo
{
private:
	int V; // número de vértices

	// ponteiro para um array contendo as listas de adjacencias
	list<pair<int, int> > * adj;

public:

	// método construtor
	Grafo(int V)
	{
		this->V = V; // atribuição de número de vértices

		/*
			criar listas onde cada lista é uma lista de pairs
			cada pair formado por vértice destino e o custo
		*/
		adj = new list<pair<int, int> >[V];
	}

	// adicionar aresta ao grafo de v1 à v2
	void addAresta(int v1, int v2, int custo)
	{
		adj[v1].push_back(make_pair(v2, custo));
	}

	// algoritmo de caminhos minimos (Dijkstra)
	int dijkstra(int saopaulo, int riodejaneiro)
	{
		// vetor de distâncias
		int dist[V];

		/*
		   vetor visitados serve para caso o vértice já tenha sido
		   expandido (visitado) não expandir mais
		*/
		int visitados[V];

		// fila de prioridades de pair (distancia, vértice)
		priority_queue < pair<int, int>,
					   vector<pair<int, int> >, greater<pair<int, int> > > pq;

		// inicia o vetor de distâncias e visitados
		for(int i = 0; i < V; i++)
		{
			dist[i] = INFINITO;
			visitados[i] = false;
		}

		// a distância de saopaulo para saopaulo é 0
		dist[saopaulo] = 0;

		// insere na fila
		pq.push(make_pair(dist[saopaulo], saopaulo));

		// loop do algoritmo
		while(!pq.empty())
		{
			pair<int, int> p = pq.top(); // extrai o pair do topo
			int u = p.second; // obtém o vértice do pair
			pq.pop(); // remove da fila

			// verifica se o vértice não foi expandido
			if(visitados[u] == false)
			{
				// marca como visitado
				visitados[u] = true;

				list<pair<int, int> >::iterator it;

				// percorre os vértices "v" adjacentes de "u"
				for(it = adj[u].begin(); it != adj[u].end(); it++)
				{
					// obtém o vértice adjacente e o custo da aresta
					int v = it->first;
					int custo_aresta = it->second;

					// relaxamento (u, v)
					if(dist[v] > (dist[u] + custo_aresta))
					{
						// atualiza a distância de "v" e insere na fila
						dist[v] = dist[u] + custo_aresta;
						pq.push(make_pair(dist[v], v));
					}
				}
			}
		}

		// retorna a distância mínima da saopaulo até riodejaneiro
		return dist[riodejaneiro];
	}
};

int main(int argc, char *argv[])
{
	Grafo g(5);

	g.addAresta(0, 1, 4);
	g.addAresta(0, 2, 2);
	g.addAresta(0, 3, 5);
	g.addAresta(1, 4, 1);
	g.addAresta(2, 1, 1);
	g.addAresta(2, 3, 2);
	g.addAresta(2, 4, 1);
	g.addAresta(3, 4, 1);

	cout << g.dijkstra(0, 4) << endl;

	return 0;
}



//Prova que o algoritmo funciona corretamente

Para efetuar a prova, vamos primeiro definir o conceito de caminho especial mais curto. Seja v1 a origem. Para qualquer vértice vi que não faz parte do conjunto S, o caminho especial mais curto é o caminho mais curto para alcançar vi a partir de v1 e que passa somente por vértices do conjunto S.

Para provar que o algoritmo funciona corretamente, é so provar o seguinte:

Se um vértice i pertence ao conjunto S, então D[i] contém o comprimento do caminho mais curto entre a origem e i.
Se um vértice i não faz parte do conjunto S, então D[i] contém o comprimento do caminho especial mais curto entre a origem e i.
