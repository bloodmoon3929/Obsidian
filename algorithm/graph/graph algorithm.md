# 그래프 알고리즘<br>

1. [벨만-포드 알고리즘](#1-벨만-포드-알고리즘)
2. [존슨 알고리즘](#2-존슨-알고리즘)
3. [코사라주 알고리즘](#3-코사라주-알고리즘)

## 1. 벨만-포드 알고리즘

### 다익스트라와의 차이점
차이점을 설명하기 위해 다익스트라 알고리즘을 간단히 설명하겠습니다.
1. 시작 정점에 인접한 정점을 조사후 더 작은 값을 가진 정점으로 이동합니다.
2. 이동한 정점의 이웃 정점의 최단기록을 기록합니다. 이미 최단거리가 기록된 정점에 대해 재조사 하지 않습니다.

이 경우 일반적인 상황에서는 원하는 값이 출력될 것이고, 프림 알고리즘은 모든 정점을 조사할 때까지 끝나지 않지만 다익스트라는 원하는 정점까지의 최단거리를 구할시 종료됩니다.
<br><br>
하지만 이는 음수 가중치를 가지는 엣지가 없을 때의 기준입니다. 다익스트라 알고리즘은 바로 마주하는 정점만을 조사하기에 그 이웃의 이웃에 음수 가중치가 있다는 사실을 알 수 없습니다.
<br><br>
이 경우에 벨만-포드 알고리즘을 사용하게 됩니다. 벨만-포드 알고리즘의 구동 방식은 다음과 같습니다.

1. 현재 정점을 0으로 초기화하고, 나머지 정점에 대해서는 무한대로 초기화합니다.
2. 시작 정점에 인접한 정점을 조사후 더 작은 값을 가진 정점으로 이동합니다.
3. 이동한 정점의 이웃 정점의 최단기록을 기록합니다. 이미 최단거리가 기록된 정점도 재조사하여 최단거리를 최신화합니다.

두 알고리즘은 크게 다르지 않지만 벨만-포드알고리즘은 음수 가중치 문제를 해결할 수 있습니다. 하지만 다익스트라 알고리즘에 비해 더 높은 시간 복잡도를 가지게 될 것입니다.


### 벨만-포드 알고리즘의 구현

```cpp
#include<iostream>
#include<vector>
#include<climits>

using namespace std;

struct Edge
{
    int src;
    int dst;
    int weight;
};

const int UNKNOWN=INT_MAX;

vector<int> BellmanFord(vector<Edge> edges, int V, int start)
{
    vector<int> distance(V, UNKNOWN);
    distance[start]=0;

    for(int i=0; i<V-1; i++)
    {
        for(auto& e : edges)
        {
            if(distance[e.src]==UNKNOWN)
                continue;
            if(distance[e.dst]>distance[e.src]+e.weight)
            {
                distance[e.dst]=distance[e.src]+e.weight;
            }
        }  
    }
    return distance;
}

int main()
{
    int V=5;
    vector<Edge> edges;
    vector<vector<int>> edge_map {{0,1,3},{1,2,5},{1,3,10},{3,2,-7},{2,4,2}};
    for(auto& e:edge_map)
        edges.emplace_back(Edge {e[0],e[1],e[2]});
    int start =0;
    vector<int> distance = BellmanFord(edges,V,start);

    cout<<"["<<start<<"번 정점으로 부터 최소거리"<<"]"<<endl;

    for(int i =0; i<distance.size(); i++)
    {
        if(distance[i]==UNKNOWN)
            cout<<i<<"번 정점 : 방문하지 않음!"<<endl;
        else
            cout<<i<<"번 정점 : "<<distance[i]<<endl;
    }
}
```

`` 실행 결과``<br> 
![image](https://github.com/bloodmoon3929/Algorithm/assets/144004857/659a1186-3495-464b-a44d-8175a9f0b375)

``그래프``<br>
![image](https://github.com/bloodmoon3929/Algorithm/assets/144004857/7a257365-b747-4422-bf4a-f99dda700fda)

``가중치``
||||||가중치|
|-|-|-|-|-|:-:|
|0|-|-|-|-|0|
|0|1|-|-|-|3|
|0|1|3|2|-|6|
|0|1|3|-|-|13|
|0|1|3|2|4|8|

### 음수 가중치 사이클
만일 한 사이클의 총 합이 음수일 경우를 가정해보겠습니다.<br>
![image](https://github.com/bloodmoon3929/Algorithm/assets/144004857/94207d19-6d50-483c-b716-35d380fd4a3e)<br>
이 경우 B, C, D 정점의 엣지의 총합이 음수가 나오게 됩니다. 이 경우 최단 경로를 찾지 않고, 해당 사이클을 도는 것이 가중치 값이 적게 나올것이라 생각할 것입니다.
<br>
해당 오류를 발견하는 것은 어렵지 않습니다. 정점의 최단 경로를 구하는데에는 $(정점의 수-1)$번의 단계만을 거치면 되는데, 그 보다 많이 반복하여 찾은 최단 경로는 음수 사이클을 거친 최단 경로 일것이기 때문입니다.
<br>
그러한 이유로 마지막에 다시 모든 엣지를 검사하여 기존에 구한 값보다 적은 가중치를 가지게 된다면, 이는 음수 사이클이 있는 경우일 것이고 이 경우에 벨만-포드 알고리즘으로는 정확한 값을 가질 수 없을 것입니다.


```cpp
#include<iostream>
#include<vector>
#include<climits>

using namespace std;

struct Edge
{
    int  src;
    int dst;
    int weight;
};

const int UNKNOWN=INT_MAX;

vector<int> BellmanFord(vector<Edge> edges, int V, int start)
{
    vector<int> distance(V, UNKNOWN);
    distance[start]=0;

    for(int i=0; i<V-1; i++)
    {
        for(auto& e : edges)
        {
            if(distance[e.src]==UNKNOWN)
                continue;
            if(distance[e.dst]>distance[e.src]+e.weight)
            {
                cout<<"음수 가중치 사이클 발견!"<<endl;
                return {};
            }
        }  
    }
    return distance;
}


int main()
{
    int V=6;
    vector<Edge> edges;
    vector<vector<int>> edge_map {{0,1,3},{1,3,-8},{2,1,3},{2,5,5},{3,2,3},{2,4,2},{4,5,-1},{5,1,8}};
    for(auto& e:edge_map)
        edges.emplace_back(Edge {e[0],e[1],e[2]});
    int start =0;
    vector<int> distance = BellmanFord(edges,V,start);

    if(!distance.empty())
    {
        cout<<"["<<start<<"번 정점으로 부터 최소거리"<<"]"<<endl;

        for(int i =0; i<distance.size(); i++)
        {
            if(distance[i]==UNKNOWN)
                cout<<i<<"번 정점 : 방문하지 않음!"<<endl;
            else
                cout<<i<<"번 정점 : "<<distance[i]<<endl;
        }
    }
}
```

``실행 결과``<br>
![image](https://github.com/bloodmoon3929/Algorithm/assets/144004857/4cc9e26f-46f1-49c2-8b56-492f0b65caca)

``그래프``<br>
![image](https://github.com/bloodmoon3929/Algorithm/assets/144004857/abad8dba-e67f-40da-b9fb-13f856a89733)

이 경우에는 1,2,3번 정점에 대하여 음수 사이클이 만들어 지기에 벨만-포드 알고리즘으로는 풀 수 없게 되고, 이를 사용자에게 "음수 가중치 사이클 발견!" 이라는 메세지와 함께 불가능 하다는 사실을 알려주게 됩니다.

## 2. 존슨 알고리즘

### 존슨 알고리즘이란
존슨 알고리즘은 다익스트라의 효율성을 활용하지만 음수 가중치에 대하여도 올바른 결과를 생성할 수 있는 알고리즘입니다.
<br>
이를 해결하기 위해서는 벨만-포드 알고리즘을 활용해야 합니다.

1. 우선 더미 정점을 만든 후 모든 정점과 연결하고, 가중치를 0으로 초기화 합니다.
2. 그 후 0번 정점을 기준으로 벨만-포드 알고리즘을 작동하여 최단 거리를 측정합니다.
3. 시작 정점과 끝 정점 사이의 거리에 더미 정점부터 시작 정점까지의 거리는 더하고, 더미 정점부터 끝 정점까지는 빼줍니다.

벨만-포드 알고리즘을 통하여 반환되는 값은 distance[dummy, start]+weight(dummy, end)>=distance[dummy, end]의 조건을 항상 충족하기에 weight(dummy, end)+distance[dummy, start]-distance[dummy, end]의 값은 항상 0보다 크거나 같습니다.<br>
즉 이 과정을 거치면 음수 가중치를 양수 가중치로 바꿀수 있습니다.<br>

<br>

``그래프``<br>
![image](https://github.com/bloodmoon3929/Algorithm/assets/144004857/1cf6442e-0160-4bf3-9bac-bdbc6c6fb7f5)
<br>
위의 그래프를 기준으로 양수 가중치를 구해보겠습니다.

1. 우선 각 정점의 최단 거리를 기록하겠습니다. 다음은 탐색순으로 정점을 나타내고 이의 최단거리를 기록한 것입니다.

|0|1|2|0|3|4|
|-|-|-|-|-|-|
|0|-7|-9|0|-5|-1|

2. 이제 기록된 정점간의 거리를 기록하겠습니다.

|0-1|1-2|2-0|0-3|3-4|
|-|-|-|-|-|
|-7|-2|10|-5|4|

3. 이제 주어진 식을 기준으로 계산해보겠습니다.()
   
(두 정점간의 가중치)=(두 정점간의 가중치)+(더미 정점부터 시작 정점간의 가중치)-(더미 정점부터 끝 정점간의 가중치)<br>
(0부터 1(-7))+(S부터 0(0))-(S부터 1(-7))=0<br>
``-7+0-(-7)=0``<br>
(1부터 2(-2))+(S부터 1(-7))-(S부터 2(-9))=0<br>
``-2+-7-(-9)=0``<br>
(2부터 0(10))+(S부터 2(-9))-(S부터 0(0))=1<br>
``10+(-9)-(0)=1``<br>
(0부터 3(-5))+(S부터 0(0))-(S부터 3(-5))=0<br>
``-5+0-(-5)=0``<br>
(3부터 4(4))+(S부터 3(-5))-(S부터 4(-1))=0<br>
``-4+(-5)-(-1)=0``<br>

추가로 0부터 4까지의 가중치부분은 책에 나와있지 않으나 구해보자면
(0부터 4(2))+(S부터 0(0))-(S부터 4(-1))=3<br>
``2+0-(-1)=3``<br>

이를 토대로 양수 가중치를 구하였고 이는 다음과 같습니다.

|0-1|1-2|2-0|0-3|3-4|0-4|
|-|-|-|-|-|-|
|0|0|1|0|0|3|


``양수 가중치``<br>
![image](https://github.com/bloodmoon3929/Algorithm/assets/144004857/180cf1c8-8bda-4bd0-9d86-ab6d369656d3)

양수 가중치를 다시 음수 가중치로 만들기 위해서는 위에서 빼주었던 더미 정점부터 도착 정점까지의 거리를 다시 더해주면 됩니다.

### 존슨 알고리즘의 구현

```cpp
#include<iostream>
#include<vector>
#include<climits>

using namespace std;

struct Edge
{
    int src;
    int dst;
    int weight;
};

const int UNKNOWN = INT_MAX;

bool HasNegativeCycle(const vector<Edge>& edges, vector<int> distance)
{
    for(auto& e : edges)
    {
        if(distance[e.src]==UNKNOWN)
            continue;
        if(distance[e.src]+e.weight<distance[e.dst])
            return true;
    }
    return false;
}

vector<int> BellmanFord(vector<Edge> edges, int V)
{
    vector<int> distance(V+1, UNKNOWN);

    int s=V;
    for(int i=0; i<V;i++)
    {
        edges.push_back(Edge{s,i,0});
    }
    distance[s]=0;

    for(int i=0; i<V; i++)
    {
        for(auto& e : edges)
        {
            if(distance[e.src]==UNKNOWN)
                continue;
            if(distance[e.dst]>distance[e.src]+e.weight)
            {
                distance[e.dst]=distance[e.src]+e.weight;
            }
        }
    }
    if(HasNegativeCycle(edges,distance))
    {
        cout<<"음수 가중치 사이클 발견!"<<endl;
        return {};
    }
    return distance;
}
    
int GetMinDistance(vector<int>& distance, vector<bool>& visited)
{
    int minDistance = UNKNOWN;
    int minIndex = -1;

    for(int i=0; i<distance.size(); i++)
    {
        if(!visited[i]&&distance[i]<=minDistance)
        {
            minDistance=distance[i];
            minIndex=i;
        }
    }
    return minIndex;
} 


vector<int> Dijkstra(vector<Edge> edges, int V, int start)
{
    vector<int> distance(V,UNKNOWN);
    vector<bool> visited(V, false);
     
    distance[start]=0;

    for(int i=0; i<V-1; i++)
    {
        int curr= GetMinDistance(distance, visited);
            
        visited[curr]=true;
        for(auto& e :edges)
        {
            if(e.src !=curr)
                continue;
            if(visited[e.dst])
                continue;
            if(distance[curr]!=UNKNOWN&&distance[e.dst]>distance[curr]+e.weight)
                distance[e.dst]=distance[curr]+e.weight;
        }
    }
    return distance;
}

void Johnson(vector<Edge> edges, int V)
{
    vector<int> h=BellmanFord(edges, V);
    if(h.empty())
        return;
    for(auto& e : edges)
    {
        e.weight+=(h[e.src]-h[e.dst]);
    }
    vector<vector<int>> shortest(V);
    for(int i=0; i<V; i++)
    {
        shortest[i]=Dijkstra(edges, V, i);
    }
    for(int i=0; i<V; i++)
    {
        cout<<i<<":\n";

        for(int j=0; j<V; j++)
        {
            if(shortest[i][j]!=UNKNOWN)
            {
                shortest[i][j] +=h[j]-h[i];
                cout<<"\t"<<j<<":"<<shortest[i][j]<<endl;
            }
        }
    }
}

int main()
{
    int V=5;
    vector<Edge> edges;

    vector<vector<int>> edge_map{{0,1,-7},{1,2,-2},{2,0,10},{0,3,-5},{0,4,2},{3,4,4}};

    for(auto& e : edge_map)
    {
        edges.emplace_back(Edge {e[0],e[1],e[2]});
    }
    Johnson(edges, V);
}
```
``그래프``<br>
![image](https://github.com/bloodmoon3929/Algorithm/assets/144004857/cf7a1ad9-c25d-4ecc-ad0e-4bf30ac32a5e)

``실행 결과``<br>
![image](https://github.com/bloodmoon3929/Algorithm/assets/144004857/eb7d5252-5c53-4cc4-b819-330307c8ccba)

## 3. 코사라주 알고리즘


### 강한 연결 요소

강한 연결요소는 부분 집합에 대하여 다른 집합으로 정보를 보낼수는 있지만, 받을수는 없는 구조를 말합니다. 아래의 그림을 토대로 이해하면 편할 것입니다.

``강한 연결 요소``<br>
![강한연결요소](https://github.com/bloodmoon3929/Algorithm/assets/144004857/32b3ebe0-fd61-4988-b4e8-3904c96312ad)

### 코사라주 알고리즘이란

코사라주 알고리즘은 강한 연결 요소들을 판별해내는 알고리즘입니다. 알고리즘의 메커니즘은 우선 DFS(깊이 우선 탐색)한 후 그래프의 방향을 기존에 가르치던 방향의 반대 바라보도록 만듭니다. 그 후 해당 그래프를 DFS를 하게 된다면, 강한 연결요소들이 구별될 것입니다.
<br>
책에 나온 그래프로 예시를 보여드리겠습니다. 시작 정점은 0번 정점이라 가정하고, 정점의 번호가 적은 것을 선택한다 가정하겠습니다. <br>

``그래프``<br>
![image](https://github.com/bloodmoon3929/Algorithm/assets/144004857/6f650f1e-3d85-4ce9-9c43-1a82e4531820)

``방문 정점``
||||||||||
|-|-|-|-|-|-|-|-|-|
|0|1|2|3|7|8|5|4|6|

``종료 순서 스택``
||||||||||
|-|-|-|-|-|-|-|-|-|
|6|4|5|8|7|3|2|1|0|


스택은 선입 후출 구조이므로 0번 정점부터 DFS를 실시 하겠습니다.

``전치 그래프``<br>
![image](https://github.com/bloodmoon3929/Algorithm/assets/144004857/c7768cc6-1d09-4068-b4c5-8181bbb7f901)
<br>

``강한 연결 요소``<br>
||||||||||
|-|-|-|-|-|-|-|-|-|
|0|||||||||
|1|||||||||
|2|4|5|6||||||
|3|7|8|||||||

### 코사라주 알고리즘의 구현

```cpp
#include <iostream>
#include <vector>
#include <stack>

using namespace std;

void FillStack(int node, vector<bool>& visited, vector<vector<int>>& adj, stack<int>& stack)
{
    visited[node]=true;

    for(auto next : adj[node])
    {
        if(!visited[next])
        {
            FillStack(next, visited, adj, stack);
        }
    }
    stack.push(node);
}

vector<vector<int>> Transpose(int V, vector<vector<int>> adj)
{
    vector<vector<int>> transpose(V);

    for(int i=0; i<V; i++)
    {
        for(auto next :adj[i])
        {
            transpose[next].push_back(i);
        }
    }
    return transpose;
}

void CollectConnectedComponents(int node, vector<bool>& visited, vector<vector<int>>& adj, vector<int>& component)
{
    visited[node]=true;
    component.push_back(node);

    for(auto next : adj[node])
    {
        if(!visited[next])
        {
            CollectConnectedComponents(next, visited, adj, component);
        }
    }
}

vector<vector<int>> Kosaraju(int V, vector<vector<int>> adj)
{
    vector<bool> visited(V, false);
    stack<int> stack;

    for(int i=0; i<V; i++)
    {
        if(!visited[i])
        {
            FillStack(i, visited, adj, stack);
        }
    }
    vector<vector<int>> transpose = Transpose(V, adj);
    fill(visited.begin(), visited.end(),false);
    vector<vector<int>> connectedComponents;

    while(!stack.empty())
    {
        int node= stack.top();
        stack.pop();

        if(!visited[node])
        {
            vector<int> component;

            CollectConnectedComponents(node, visited, transpose, component);
            connectedComponents.push_back(component);
        }
    }
    return connectedComponents;
}

int main()
{
    int V=9;
    vector<vector<int>> adj={{1,3},{2,4},{3,5},{7},{2},{4,6},{7,2},{8},{3}};

    vector<vector<int>> connectedComponents = Kosaraju(V, adj);
    cout<<"강한 연결 요소 개수 : "<<connectedComponents.size()<<endl;
    for(int i=0; i < connectedComponents.size(); i++)
    {
        cout<<"["<<i+1<<"]";
        for(auto node : connectedComponents[i])
            cout<<node<<" ";
        cout<<endl;
    }
}
```
``그래프``<br>
![image](https://github.com/bloodmoon3929/Algorithm/assets/144004857/6f650f1e-3d85-4ce9-9c43-1a82e4531820)

``방향 그래프가 지시하는 것``<br>
|0번 정점|1번 정점|2번 정점|3번 정점|4번 정점|5번 정점|6번 정점|7번 정점|8번 정점|
|:-----:|:------:|:-----:|:------:|:-----:|:------:|:-----:|:------:|:-----:|
|1|2|3|7|2|4|7|8|3|
|3|4|5|||6|2|||

``실행 결과``<br>
![image](https://github.com/bloodmoon3929/Algorithm/assets/144004857/3e36f4a8-5535-4f54-a4e9-2a0b7b929b41)