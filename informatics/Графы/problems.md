# Графы

### Цикл
Вам дан ориентированный граф. Определите есть ли в нём цикл.
Входные данные

В первой строке заданы 2 числа количество вершин и рёбер. Далее заданы ребра.
Выходные данные

Вывдете YES если в графе цикл есть, иначе NO.

```python

def main():
    def dfs(v):
        #nonlocal begin, end
        color[v] = 1
        for to in graph[v]:
            if color[to] == 0:
                parent[to] = v
                dfs(to)
            elif color[to] == 1:
                indexcycle['begin'] = to
                indexcycle['end'] = v
        color[v] = 2
    n, m = map(int, input().split())
    graph = {i+1:[] for i in range(n)}
    for it in range(m):
        u, v = map(int, input().split())
        # u -= 1
        # v -= 1
        graph[u].append(v)
    indexcycle = {"begin":None, "end":None}
    color = [0] * (n+1)
    parent = [None] * (n+1)
    begin = end = None
    for start in range(1, n+1):
        if color[start] == 0:
            dfs(start)
    cycler = []
    if indexcycle['begin'] is None:
        print('NO')
        return
    else:
        print('YES')

main()
```
[Решить задачу](https://codeforces.com/gym/608550/submit/1)

### Обход в ширину
Дан (неориентированный) граф. Обойдите его методом обхода в ширину. В ответ выведете расстояния от вершины 1 до всех остальных вершин. Если какая-то вершина вдруг не посещена то вместо растояния напишите -1.
Входные данные

В первой строке даны 2 числа: числовершин и рёбер. В следующих m строках заданы рёбра графа.
Выходные данные

Выведите ответ на задачу.

```python
from collections import deque as Queue
def bfs(start):
    dist = [float('inf')] * (n+1)
    dist[start] = 0
    q = Queue()
    q.append(start)
    while len(q):
        v = q.popleft()
        for vertex in graph[v]:
            if dist[vertex] == float('inf'):
                dist[vertex] = dist[v] + 1
                q.append(vertex)
    return dist

n, m = map(int, input().split())
graph = {i+1:[] for i in range(n)}
for i in range(m):
    u, v = map(int, input().split())
    graph[u].append(v)
    graph[v].append(u)
print(*(-1 if i == float('inf') else i for i in bfs(1)[1:]))
```

[Решить задачу](https://codeforces.com/gym/608550/submit/2)

### Алгоритм Дейкстры

Вам дан неориентированный взвешаный граф. Примените на нём алгоритм Дейкстры. Выведте все кратчайшие расстояния от 1 вершины до другой, если вершину не удалось обойти то вместо кратчайшего расстояния там выведте inf.
Входные данные

В первой строке даны 3 числа n — количество вершин, m — количество рёбер, и статовая вершина — s. Далее в следующих m строках заданы рёбра графа в формате u, v, w.
Выходные данные

Выведете ответ на задачу

```python

import collections as coll
def dijkstra(graph, start):
    vertexes = {i+1:{"dist":float('inf'), "parent":-1} for i in range(len(graph))}
    vertexes[start]["dist"] = 0
    q = coll.deque()
    q.append(start)
    while len(q):
        vertex = q.popleft()
        for v, w in graph[vertex]:
            newd = vertexes[vertex]["dist"] + w
            if newd < vertexes[v]["dist"]:
                vertexes[v]["dist"] = newd
                vertexes[v]["parent"] = vertex
                q.append(v)
    return vertexes

def main():
    n, m, start = map(int, input().split())
    graph = {i+1:[] for i in range(n)}
    for i in range(m):
        u, v, w = map(int, input().split())
        graph[u].append((v, w))
        graph[v].append((u, w))
    result = dijkstra(graph, start)
    print(*(result[item]["dist"] for item in result))
    
main()
```

[Решить задачу](https://codeforces.com/gym/608550/submit/16)

### Компоненты-циклы

Дан граф. Выведете сколько в нём компонентов являющихся циклами.
Входные данные

В первой строке заданы 2 числа n и m — количество вершин и рёбер, в следующх m строках находятся по 2 числа:вершины соединёные ребром.
Выходные данные

Требуется вывести ответ на задачу.

```python
def dfs(vertex, color):
    colors[vertex] = color
    stack = [vertex]
    while len(stack):
        v = stack.pop()
        for u in graph[v]:
            if not colors[u]:
                stack.append(u)
                colors[u] = color
class Graph:
    def __init__(self):
        self.graph = {}
    def listengraph(self):
        self.n, self.m = map(int, input().split())
        self.graph = {i+1:[] for i in range(self.n)}
        for i in range(self.m):
            u, v = map(int, input().split())
            self.graph[u].append(v)
            self.graph[v].append(u)
    def creategraph(self, gr):
        pass

l = Graph()
l.listengraph()
n = l.n
graph = l.graph

colors = [0] * (n+1)
col = 0
for i in range(1, n+1):
    if not colors[i]:
        col += 1
        dfs(i, col)
componentes = {i + 1:[] for i in range(col)}
v = 1
for vertex in colors[1:]:
    componentes[vertex].append(v)
    v += 1
result = 0
graphes = {i+1:{} for i in range(col)}
num = 0
for i in componentes:
    x = {}
    for k in componentes[i]:
        x[k] = len(graph[k])
    graphes[num+1] = x
    num += 1
for componenta in componentes:
    if list(graphes[componenta].values()).count(2) == len(componentes[componenta]):
        result += 1
print(result)
```

[Решить задачу](https://codeforces.com/gym/608550/submit/3)
[Полное инфо задачи](https://yumsh24axol.contest.codeforces.com/group/D7AXT2cfnk/contest/586399/submit/E)


### Правила дорожного движения
В столице одной небольшой страны очень сложная ситуация. Многокилометровые пробки буквально парализовали движение в городе, и власти на многих улицах ввели одностороннее движение, не анализируя, можно ли будет теперь проехать из любого места в городе в любое другое, не нарушая правила. Транспортная система столицы представляет собой N площадей, соединенных M

полосами для движения, в том числе круговыми полосами, проходящими по площади. Каждая полоса предназначена для движения только в одну определенную сторону. При этом на магистралях есть полосы, направленные как в одну, так и в другую сторону. По круговой полосе можно двигаться только внутри площади и только против часовой стрелки.

Власти города на каждой полосе разместили видеокамеру, поэтому если Иннокентий едет по встречной полосе (при ее наличии) или, в случае одностороннего движения, в сторону противоположную предписанной знаками, то после поездки против правил по каждой из полос ему придется заплатить штраф в размере одной тысячи тугриков этой страны.

Иннокентий, который торопится купить кафельную плитку со скидкой, решился доехать до магазина в любом случае, даже если для этого придется нарушать правила. Но он хочет выбрать такой маршрут движения, суммарный штраф на котором минимален.

Иннокентий еще не решил, откуда именно и в какой магазин он собирается ехать, поэтому ему необходимо ответить на несколько вопросов вида «Какой минимальный штраф надо заплатить, чтобы добраться из пункта A
в пункт B

?». Отвечая на потребности жителей столицы, известная поисковая система Индекс разрабатывает соответствующий сервис.

Так как многие из вас рано или поздно будут проходить собеседование на работу в эту фирму, продемонстрируйте, что вы тоже умеете решать эту задачу.
Входные данные

В первой строке входных данных содержатся два числа N
и M — количество площадей и полос движения в городе соответственно (1≤N≤5000, 1≤M≤10000

). Далее содержатся описания полос, по которым движение разрешено. Каждая полоса описывается номерами двух площадей, которые она соединяет. Движение разрешено в направлении от первой из указанных площадей ко второй.

В следующей строке содержится одно число K
— количество вопросов у Иннокентия (1≤K≤10000, N⋅K≤2⋅107

). В следующих строках описываются вопросы, каждый вопрос описывается номерами двух площадей, между которыми требуется найти самый дешевый путь. Путь необходимо проложить от первой из указанных площадей ко второй.
Выходные данные

Для каждого вопроса выведите одно число — искомый минимальный размер штрафа в тысячах тугриков. В случае, если пути между выбранной парой площадей не существует, выведите −1
.

```python
import collections as coll

def dijkstra(start):
    q = coll.deque()
    q.append(start)
    dist = [float('inf')] * (n+1)
    dist[start] = 0
    while len(q):
        vertex = q.popleft()
        for v, w in graph[vertex]:
            newd = dist[vertex] + w
            if newd < dist[v]:
                dist[v] = newd
                q.append(v)
    return dist


n, m = map(int, input().split())
graph = {i+1:[] for i in range(n)}
for i in range(m):
    u, v = map(int, input().split())
    graph[u].append((v, 0))
    graph[v].append((u, 1))
dist = []
for i in range(1, n+1):
    dist.append(dijkstra(i))
q = int(input())
for queshion in range(q):
    u, v = map(int, input().split())
    print('-1' if dist[u-1][v] == float('inf') else dist[u-1][v])

```

### Два коня
На стд. шахматной доске живут два коня: белый и синий. Они бродя по доске пощипывая вкусную шахматную траву. Но сегодня у Белого коня День Рождения и поэтому Белый конь приглашает Синего в дом Белого коня. Для этого им надо оказаться на одной клетке. А потом они пойдут вместе. Эти кони — ходят точно также как шахматные но друг друга не едят и ходят одновременно.
Входные данные

В первой строке задана позиция того места где стоит Белый конь и через пробел позиция того места где стоит Синий конь.
Выходные данные

Выведете минимальное количество ходов за которое два коня окажутся на одной клетке. Если они никогда не встретятся то кони придут в грусть. И надо будет вывести Infinity

```python
import collections as coll
def bfs(start):
    q = coll.deque([start])
    dist = {(i // 8, i % 8):float('inf') for i in range(8*8)}
    dist[start] = 0
    marks = []
    while len(q):
        vertex = q.popleft()
        i, j = vertex
        pos = [(i-1, j-2), (i-1, j+2), (i-2, j-1), (i-2, j+1),
               (i+1, j-2), (i+1, j+2), (i+2, j-1), (i+2, j+1)]
        for v1, v2 in pos:
            if good(v1, v2):
                if dist[vertex] + 1 < dist[(v1, v2)]:
                    q.append((v1, v2))
                    dist[(v1, v2)] = dist[vertex] + 1
    return dist
def good(i, j):
    return -1 < i < 8 and -1 < j < 8
pos1, pos2 = map(str, input().split())
horse1 = (int(pos1[1])-1, ord(pos1[0]) - ord('a'))
horse2 = (int(pos2[1])-1, ord(pos2[0]) - ord('a'))
graph = {(i // 8, i % 8):[] for i in range(8*8)}

dsh1 = bfs(horse1)
dsh2 = bfs(horse2)
marks = []
for item in dsh1:
    if dsh2[item] == dsh1[item]:
        marks.append(item)

def giperke(value):
    return dsh1[value]

try:
    print(dsh1[min(marks, key=giperke)])
except ValueError:
    print("Infinity")
```

[Решить задачу](https://codeforces.com/gym/613316/submit/2)

### Удаление клеток
Есть прямоугольник размером n*m. Из него вырезали клетки. Вам дан получившийся прямоугольник. Символ # обозначает что клетка осталась цела. В противном случае если клетка была вырезана то там будет находится символ . .
Входные данные

Даны 2 числа n и m размеры таблицы. Далее в следующих n строках находится по m символов. Каждый из которых — '#' или '.'.
Выходные данные

Выведете на сколько частей распался прямоугольник.

```python
def dfs(start_vertex):
    if not visited[start_vertex]:
        visited[start_vertex] = True
        for neighbor in graph[start_vertex]:
            dfs(neighbor)

n, m = map(int, input().split())
graph = {i: [] for i in range(1, n * m + 1)}
ls = [['\0'] * (m + 2) for _ in range(n + 2)]
activyvertex = 1
visited = [0] * (n * m + 1)
for i in range(1, n + 1):
    s = input().strip()
    for j in range(1, m + 1):
        ls[i][j] = s[j - 1]

for ii in range(2, n + 2):
    for jj in range(2, m + 2):
        i = ii - 1
        j = jj - 1
        if ls[i][j] != '#':
            activyvertex += 1
            graph.pop(activyvertex - 1, None)
            continue
        if ls[i][j - 1] == '#':
            graph[activyvertex].append(activyvertex - 1)
        if ls[i][j + 1] == '#':
            graph[activyvertex].append(activyvertex + 1)
        if ls[i - 1][j] == '#':
            graph[activyvertex].append(activyvertex - m)
        if ls[i + 1][j] == '#':
            graph[activyvertex].append(activyvertex + m)
        activyvertex += 1

vert = [False] * (n * m + 1)
componentov = 0

for i in range(1, n * m + 1):
    if not vert[i] and i in graph:
        vert[i] = True
        visited = [False] * (n * m + 1)
        dfs(i)
        componentov += 1
        for k in range(1, n * m + 1):
            if visited[k]:
                vert[k] = True

print(componentov)
```

[Решить задачу](https://codeforces.com/gym/613316/submit/1)
