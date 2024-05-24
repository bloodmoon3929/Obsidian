# 고급 알고리즘<br>

[[14번 문제]]
[[27번 문제]]

1. 도형 문제
2. 계차와 누적합
3. 미분 (뉴턴법)
4. 에라토스네스의 체
5. 그래프 알고리즘
6. 나머지 계산
7. 행렬의 거듭제곱

## 도형 문제

### 계산기하학이란?
기하학적인 문제를 컴퓨터로 풀기 위해 효율적인 알고리즘을 연구하는 학문

### 벡터란?
크기와 방향을 가진 힘. 크기와 방향이 같다면 시작점이 달라도 같은 벡터<br>
계산기하학을 풀기위해 알아야 하는 벡터의 기본개념은 다음과 같다.

벡터의 덧셈과 뻴셈
<br>
$\overrightarrow{a}+\overrightarrow{b}=(a_x+b_x,a_y+b_y)$<br>
$\overrightarrow{a}-\overrightarrow{b}=(a_x-b_x,a_y-b_y)$
<br>
벡터의 크기
<br>
$|\overrightarrow{a}|=\sqrt{(a_x)^2+(a_y)^2}$
<br>
벡터의 내적
<br>
$\overrightarrow{a}\cdot\overrightarrow{b}=a_xb_x+a_yb_y$<br>
내적의 세 가지 성질
```
두 벡터가 이루는 각이 90도 미만 - 내적은 양수
두 벡터가 이루는 각이 90도     - 내적은 0
두 벡터가 이루는 각이 90도 초과 - 내적은 음수
```
<br>
벡터의 외적
<br>

$|\overrightarrow{a} \times \overrightarrow{b}|=|a_xb_y-a_yb_x|$<br>
외적의 두가지 성질
1. 외적의 크기는 두 벡터가 만드는 평행사변형의 면젹과 같다.
2. 외적의 식에서 절댓값을 제외한다고 생각하면
   <br>ex) 
   $\overrightarrow{BA}\times\overrightarrow{BC}$<br>
   점 A,B,C가 시계방향으로 배치되어 있다면   - 양수<br>
   점 A,B,C가 반시계방향으로 배치되어 있다면 - 음수<br>
   점 A,B,C가 일직선으로 배치되어 있다면     - 0
<br>

### 예제 : 점과 선분의 거리
이 문제를 풀기 위해서는 3가지 패턴이 나온다는 것을 알아야 합니다.
1. 각 ABC가 90도 초과 
2. 각 ACB가 90도 초과
3. 각 ABC와 ACB가 90도 이하 

1번과 2번의 경우는 고민할것도 없이 A와 B사이의 거리, A와 C사이의 거리가 최단 경로일 것입니다.<br>
하지만 3번의 경우는 선분 $\overline{BC}$와 수직을 이루는 지점이 최단경로일 것입니다.<br><br>
이를 구하기 위한 공식은 다음과 같습니다.
1. 벡터 $\overrightarrow{BA}$와 $\overrightarrow{BC}$의 외적을 구합니다.(평행 사변형의 면적을 구하기 위해)
2. 평행사변형 면적을 구하는 공식은 $밑변\times높이$임으로 구한 외적에 밑변인 $\overline{BC}$를 나누어 높이를 구합니다.

이를 구현한 코드는 다음과 같습니다.
```cpp
#include<iostream>
#include<cmath>
#include<iomanip>

using namespace std;

int main()
{
    int arr[2][3];
    int pattern=2;
    int vecter[2][4];
    for(int i=0; i<3; i++)
    {
        for(int j=0; j<2; j++)
        {
            cin>>arr[j][i];
        }
    }
   
    vecter[0][0]=arr[0][0]-arr[0][1];
    vecter[1][0]=arr[1][0]-arr[1][1];

    vecter[0][1]=arr[0][2]-arr[0][1];
    vecter[1][1]=arr[1][2]-arr[1][1];

    vecter[0][2]=arr[0][0]-arr[0][2];
    vecter[1][2]=arr[1][0]-arr[1][2];
    
    vecter[0][3]=arr[0][1]-arr[0][2];
    vecter[1][3]=arr[1][1]-arr[1][2];

    if(vecter[0][0]*vecter[0][1]+vecter[1][0]*vecter[1][1]<0)
        pattern=1;
    else if(vecter[0][2]*vecter[0][3]+vecter[1][2]*vecter[1][3]<0)
        pattern=3;

    if(pattern==1)
        cout<<setprecision(13)<<sqrt(pow(vecter[0][0],2)+pow(vecter[1][0],2));
    else if(pattern==2)
    {
       cout<<setprecision(13)<<abs(vecter[0][0]*vecter[1][1]-vecter[1][0]*vecter[0][1])/sqrt(pow(vecter[0][1],2)+pow(vecter[1][1],2));
    }
    else if(pattern==3)
        cout<<setprecision(13)<<sqrt(pow(vecter[0][2],2)+pow(vecter[1][2],2));
    return 0;
}
```
arr 배열에 입력된 값은 책 기준 다음과 같습니다.

| |0(x)|1(y)|
|-|-|-|
|a|0|5|
|b|1|1|
|c|3|0|

<br><br>
vecter 배열에 입력된 값은 책 기준 다음과 같습니다.

| |0(x)|1(y)|
|-|-|-|
|BA|-1|4|
|BC|2|-1|
|CA|-3|5|
|CB|-2|1|

<br><br>
pattern은 다음과 같음을 나타냅니다

1. ABC의 각이 90가 초과했을시($\overrightarrow{BA}\cdot\overrightarrow{BC}<0$)
2. 패턴 1, 3 둘다 아닐시
3. ACB의 각이 90가 초과했을시($\overrightarrow{CA}\cdot\overrightarrow{CB}<0$)
<br><br>

### #include<iomanip>
입출력 조작과 관련된 도구를 제공하는 헤더파일<br>
setprecision()을 사용하기 위해 사용하였고, 해당 메서드는 출력할 소수점의 몇번째 자리까지 출력할지 결정하는 메서드


이 외의 계산기하학문제는
1. 가장 가까운 점 찾기
2. 볼록 다각형 만들기
3. 보로노이 다이어그램
4. 미술관 문제등이 있습니다.


## 계차와 누적합
### 계차란?
n+1번 항과 n번 항의 차
### 누적합
1번항 부터 n번 항까지의 합

ex) 
|1|2|4|8|16|
|-|-|-|-|-|
|1|2|3|4|5|

의 계차는

|1|1|3|4|8|
|-|-|-|-|-|
|0-1|2-1|3-2|4-3|5-4|

### 예제1 : 입장 인원 계산하기
이 문제는 누접합을 이용하여 푸는 문제입니다.<br>
2번 줄에 기록된 배열의 누접합을 구하고, n부터 m까지 구한다고 가정했을시<br>
누접합 배열의 $m의 값-(n-1)의 값$을 구한다면 O(NQ)가 아닌 O(N+Q)의 시간복잡도로 이 문제를 풀 수 있습니다.

```cpp
#include<iostream>
#include<vector>

using namespace std;
int main()
{
    int N,Q,X,Y;
    cin>>N>>Q;
    vector<int> sum;
    cin>>X;
    sum.push_back(X);
    for(int i=1; i<N; i++)
    {
        cin>>X;
        sum.push_back(X+sum[i-1]);
    }
    vector<pair<int,int>> input;
    for(int i=0; i<Q; i++)
    {
        cin>>X>>Y;
        input.push_back(make_pair(X-1,Y-1));
    }
    for(int i=0; i<Q; i++)
    {
        if(input[i].first==0)
        {
            cout<<sum[input[i].second]<<endl;
        }
        else
        {
            cout<<sum[input[i].second]-sum[input[i].first-1]<<endl;
        }
    }
    return 0;
}
```

### 예제2 : 눈 시뮬레이션
이 문제는 계차를 이용하여 푸는 문제입니다.<br>
n일 부터 m일까지 k만큼의 눈이 내렸다면 계차 배열에는 n번 항에 k를 더하고 m-1항에 k를 빼주면 됩니다. 이렇게 모두 기록하고 난 후 1번부터 m번까지가(0번 제외) 구하고자한 값이고 비교 연산자를 사용해 출력만 해주면 됩니다.<br>
이 역시 시간 복잡도는 O(N+Q)입니다. 

```cpp
#include<iostream>
#include<vector>

using namespace std;
int main()
{
    int N,Q,X,Y,Z;
    cin>>N>>Q;
    vector<int> sum(N+2);
    for(int i=0; i<Q; i++)
    {
        cin>>X>>Y>>Z;
        sum[X]+=Z;
        sum[Y+1]-=Z;
    }
    for(int i=2; i<N+1; i++)
    {
        if(sum[i]>0)
        {
            cout<<"<";
        }
        else if(sum[i]==0)
        {
            cout<<"=";
        }
        else if(sum[i]<0)
        {
            cout<<">";
        }
    }
    return 0;
}
```


## 미분
### 미분이란?
특정 점에서 함수의 기울기 같은것을 구하는 것<br><br>
$기울기=\frac{y 증가량}{x 증가량}$<br><br>

다항식에서의 미분 : 각 항마다 차수만큼을 곱하여 주고 차수를 1줄임

### 뉴턴법이란?
뉴턴-랩슨법(Newton-Raphson method)이라고도 불리며, 폐구간 [a,b]에서 실수R 에 대하여 정의된 f:[a,b]->R이 미분가능할 때 방정식 f(x) = 0 의 해를 근사적으로 찾을 때 유용하게 사용되는 방법이다.

1. 구하고자 하는 수와 가까운 초깃값과 함수와 그 함수에서 구하고자 하는 수가 나오는 y좌표 기준으로 그래프를 그립니다
2. 처음 구했던 초기값을 x에 대입하여 y값을 구합니다(이 숫자를 a라고 하겠음). 그 후 미분한 함수에도 초기값을 대입하여 숫자 하나를 구합니다(이 숫자를 b라고 하겠음).
3. $y=(b)x-a$ 의 직선과 맨 처음 구하고자 하는 숫자가 있는 y좌표의 그래프와 교차하는 교점의 x좌표를 초기값에 대입함
4. 2~3번 과정을 반복함, 반복할수록 일치하는 자릿수가 배수로 증가하게됨

구현
```cpp
#include<iostream>
#include<iomanip>

using namespace std;

int main()
{
    double a=2;//초기값임 교제와 동일
    int repeat=5;//반복 횟수 교제와 동일
    int f_x, f_y;
    for(int i=0; i<=repeat; i++)
    {
        cout <<setw(2)<<i<<"번 반복시 :"<< setprecision(27) << a << endl;
        
        double f_x = a * a + 2;
        double f_prime_x = 2 * a;
        
        a = f_x / f_prime_x;
    }
    return 0;
}
```
결과
```
 0번 반복시 :2
 1번 반복시 :1.5
 2번 반복시 :1.41666666666666674068153498
 3번 반복시 :1.4142156862745098866440685
 4번 반복시 :1.41421356237468986982719343
 5번 반복시 :1.41421356237309492343001693
```
$y=(2\times a)x-a^2$<br>
이 식에서 y가 2일때를 구함으로<br>
$2=(2\times a)x-a^2$<br>
$2+a^2=(2\times a)x$<br>
$(2+a^2)\div(2\times a)=x$<br>


이 외의 수치계산 문제는
1. 수치 미분과 수치 적분
2. 아주 큰 정수 계산 문제가 있음
   
## 에라토스네스의 체
### 에라토스네스의 체란?
1. 2부터 N까지의 정수를 기록한다
2. 표시가 없는 가장 작은수에 O표시를 하고 그 수의 배수에 X표시를 한다(소수는 1과 본인빼고는 약수가 없어야 하기 때문)
3. 2번 과정을 반복하여 N번까지 반복한다
4. O표시가 있는 것이 소수이다.

구현
```cpp
#include<iostream>
#include<vector>
#include<cmath>

using namespace std;

int main()
{
    int N,limit;
    cin>>N;
    vector<bool> arr(N+1);
    fill(arr.begin(),arr.end(),true);
    
    limit=sqrt(N);
    for(int i=2; i<limit+1; i++)
    {
        if(arr[i]==true)
        {
            for(int j=i*i; j<N+1; j+=i)
            {
                arr[j]=false;
            }
        }
    }
    for(int i=2; i<N+1; i++)
    {
        if(arr[i]==true)
        {
            cout<<i<<", ";
        }
    }

    return 0;
}
```
limit가 $\sqrt{N}$인 이유<br>
N이 100이라고 가정했을시, 100의 루트인 10의 배수에 100이 있기에 그 이상의 값은 확인하지 않아도 되기 때문

### 적분이란>
어떤 함수로 만들어진 영역의 면적을 구하는 조작. y가 0이하의 값에 대해서는 양수가 아닌 음수임을 기억해야한다.

다항식의 적분의 경우 : 차수를 1 증가시키고 차수만큼 나누어 줌

$\frac{1}{x}$의 적분 : $\int\limits_a^b\frac{1}{x}dx=(\log_eb-\log_ea)$ (에라토스네스의 체의 시간복잡도를 구하는 데 사용됨)

역수의 합이 $\log$임을 증명하기 : 
1. 우선 정수들을 x축, 그 정수의 역수를 y축으로 두었다고 가정하고 막대그래프를 그립니다.
2. 그래프 기준 그래프의 크기를 넘지 않는 $y=\frac{1}{x+1}$를 A라고 가정했을 시 면적은 $\log_e(N+1)$
3. 그래프 기준 그래프의 크기를 넘는 $y=\frac{1}{x}$를 C라고 가정했을시 면적은 $\log_e(N)+1$
4. 역수의 합(조화급수)는 두 면적 A,C사이의 값이며, log값에 근사함
<br>

## 그래프 알고리즘
### 그래프란?
알고리즘에서 그래프는 네트워크 구조를 의미함, 정점(vertex)와 변(edge)로 이루워짐

### 그래프의 종류
1. 무방향 그래프, 방향 그래프
2. 가중치 그래프, 무가중 그래프
3. 이분 그래프 : 연결된 정점끼리 같은 색이 아닌 그래프
4. 평면 그래프 : edge가 서로 교차하지 않는 그래프
5. 오일러 그래프 : 한 정점에서 시작하여 모든 edge를 지나 다시 출발했던 정점으로 돌아올수 있는 그래프
6. 트리 구조
7. 완전 그래프 : 무방향 그래프에서 모든 정점 사이에 간선이 연결된 것
8. 정규 그래프 : 무방향 그래프에서 모든 정점의 차수(간선의 수)가 같은것
9. 완전 이분 그래프 : 모든 정점 사이에 간선이 하나뿐인 이분 그래프
10. 방향 비순환 그래프 : 방향 그래프중 같은 정점을 거치지 않고 원래 정점으로 돌아올 수 있는 경로가 없는 그래프

### 그래프의 구현 

인접 리스트 문제
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
int main()
{
    int N,M,X,Y;
    vector<vector<Edge>> G(N + 1);
    cin>>N>>M;
    for(int i=0; i<M; i++)
    {
        cin>>X>>Y;
        G[X].push_back({X,Y,0});
        G[Y].push_back({Y,X,0});
    }

    for (int i = 1; i <= N; i++) 
    {
        cout << i << ": "<<endl;
        for (auto& node : G[i]) 
        {
            cout <<"{" <<node.src<<":" <<node.dst<< "}"<<endl;
        }
        cout << endl;
    }
    return 0;
}
```
const int UNKNOWN=INT_MAX;는 추후 사용할때 쓰려고 미리 만듬<br>
```cpp
struct Edge
{
    int src;
    int dst;
    int weight;
};
```
src는 시작점, dst는 도착점, weight는 가중치이나 무가중 그래프이기에 0의 가중치를 주었음

### 깊이 우선 탐색
Depth First Search(DFS)라고 부르는 깊이 우선 탐색은 다음과 같은 절차를 수행합니다.
1. 모든 정점을 false로 설정
2. 선택한 정점에 방문하고, 정점의 값을 true로 수정
3. 두 가지 절차중 하나 실행(처음 실행한 정점에거 a절차 실행시 반복 종료)
   a. 인접 정점이 모두 true라면 이전 정점으로 복귀
   b. 그렇지 않다면, 현재 정점을 true로 수정후 인접 정점에서 가장 작은 정점 방문
4. 모든 정점이 true 값을 가진다면 그래프가 연결된 것임
   

### 너비 우선 탐색
Breadth First Search(BFS)라고 부르는 너비 우선 탐색은 다음과 같은 절차를 수행합니다.
1. 모든 정점을 false로 설정
2. queue에 선택한 정점 추가후 정점을 true로 설정, dist[선택한 정점]=0으로 설정(정점까지의 거리)
3. queue가 빌때까지 다음의 절차를 반복
   queue에서 요소를 확인(비어있다면 종료)
   queue의 가장 앞의 요소를 추출
   추출한 요소를 기준으로 인접정점에 dist를 해당 정점의 (dist값 +1)로 설정하고 queue에 넣음

queue - 선입 선출의 자료구조

깊이 우선 탐색과 너비 우선 탐색의 구현
```cpp
#include<iostream>
#include<vector>
#include<string>
#include<set>
#include<map>
#include<queue>
#include<stack>

using namespace std;

template<typename T>
struct Edge
{
    unsigned int src;
    unsigned int dst;
    T weight;
};

template<typename T>
class Graph
{
public:
    Graph(unsigned int N) : V(N){}
    auto vertices() const {return V;}
    auto &edges() const {return edge_list;}
    auto edges(unsigned int v) const
    {
        vector<Edge<T>> edges_from_v;
        for(auto &e: edge_list)
        {
            if(e.src==v)
                edges_from_v.emplace_back(e);
        }
        return edges_from_v;
    }
    void add_edge(Edge<T>&& e)
    {
        if(e.src>=1&&e.src<=V&&e.dst>=1&&e.dst<=V)
            edge_list.emplace_back(e);
        else
            cerr<<"에러: 유효 범위를 벗어난 정점!"<<endl;
    }
    template<typename U>
    friend ostream& operator<<(ostream& os, const Graph<U>& G);
private:
    unsigned int V;
    vector<Edge<T>> edge_list;
};

template<typename U>
ostream& operator<< (ostream& os, const Graph<U>& G)
{
    for(unsigned int i=1; i<G.vertices(); i++)
    {
        os<<i<<":\t";
        auto edges = G.edges(i);
        for(auto& e : edges)
            os<<"{"<<e.dst<<": "<<e.weight<<"}, ";
        os<<endl;
    }
    return os;
}

template<typename T>
auto create_reference_graph()
{
    Graph<T> G(9);

    map<unsigned int, vector<pair<unsigned, T>>> edge_map;
    edge_map[1]={{2,0},{5,0}};
    edge_map[2]={{1,0},{5,0},{4,0}};
    edge_map[3]={{4,0},{7,0}};
    edge_map[4]={{2,0},{3,0},{5,0},{6,0},{8,0}};
    edge_map[5]={{1,0},{2,0},{4,0},{8,0}};
    edge_map[6]={{4,0},{7,0},{8,0}};
    edge_map[7]={{3,0},{6,0}};
    edge_map[8]={{4,0},{5,0},{6,0}};

    for(auto& i: edge_map)
        for(auto& j : i.second)
            G.add_edge(Edge<T>{i.first, j.first, j.second});
    return G;
}

template<typename T>
auto DFS(const Graph<T>& G, unsigned int start)
{
    stack<unsigned int> stack;
    set<unsigned int> visited;
    vector<unsigned int> visit_order;
    stack.push(start);

    while (!stack.empty())
    {
        auto current_vertex = stack.top();
        stack.pop();

        if(visited.find(current_vertex)==visited.end())
        {
            visited.insert(current_vertex);
            visit_order.push_back(current_vertex);

            for(auto& e : G.edges(current_vertex))
            {
                if(visited.find(e.dst)==visited.end())
                {
                    stack.push(e.dst);
                }
            }
        }

    }
    return visit_order;
}


template<typename T>
auto BFS(const Graph<T>& G, unsigned int start)
{
    queue<unsigned int> queue;
    set<unsigned int> visited;
    vector<unsigned int> visit_order;
    queue.push(start);

    while (!queue.empty())
    {
        auto current_vertex = queue.front();
        queue.pop();

        if(visited.find(current_vertex)==visited.end())
        {
            visited.insert(current_vertex);
            visit_order.push_back(current_vertex);

            for(auto& e : G.edges(current_vertex))
            {
                if(visited.find(e.dst)==visited.end())
                {
                    queue.push(e.dst);
                }
            }
        }

    }
    return visit_order;
}

int main()
{
    using T=unsigned int;

    auto G= create_reference_graph<T>();
    cout<<"[입력 그래프]"<<endl;
    cout<<G<<endl;

    cout<<"[DFS 방문 순서]"<<endl;
    auto dfs_visit_order = DFS(G,1);
    for(auto v : dfs_visit_order)
        cout<<v<<endl;

    cout<<"[BFS 방문 순서]"<<endl;
    auto bfs_visit_order = BFS(G,1);
    for(auto v : bfs_visit_order)
        cout<<v<<endl;
    return 0;
}
```

이 외의 그래프 문제는 
1. 단일 시작점 최단거리 문제
2. 모든 정점까지의 최단거리 문제
3. 최소 전역 트리 문제
4. 최대 플로 문제
5. 이분 매칭 문제

## 나머지 계산
### 모듈러 역수란?
$A (mod M)$을 A를 M으로 나눈 나머지라고 할 때, A을 $B (mod M)$한 결과 값을 C, B을 $B (mod M)$한 결과 값을 D라고 했을시, 다음을 만족합니다.<br><br>

$A+B\equiv C+D(mod M)$<br>
$A-B\equiv C-D(mod M)$<br>
$A\times B\equiv C\times D(mod M)$<br>

$\equiv$ 란?<br>
예를 들어, $A\equiv B(mod M)$이라고 할 때, A를 M으로 나눈 나머지와 B를 M으로 나눈 나머지가 같음을 의미합니다.<br>
$A=M\times Q_a+r$<br>
$B=M\times Q_b+r$<br>
여기서 $Q_{a,b}$는 미지수 이고, r이 나머지입니다.

이러한 이유로 식을 모두 계산 후 나머지를 구하는 것이 아닌, 계산 이전부터 나머지를 구하고 계산후 나머지를 구하게 된다면 컴퓨터가 계산하지 못하는 단위의 계산도 가능하게 됩니다.

하지만 나누기에 대해서는 이가 성립하지 않습니다.

그러하기에 나눗셈은 다음의 예시와 같은 절차로 풀게됩니다.
<br>ex)$8\div 2=4(mod M)$  ->    $2\times 4=8(modM)$<br>
다음의 과정을 거친다면, 나눗셈이 아닌 곱셈을 계산하는 것이 되기에 모듈러 역수를 구할수 있게 됩니다. 이를 식으로 나타낸다면,<br>
$A\div B=C(mod M)$<br>
$A\times B^{-1}=C(mod M)$<br>
$A\times B\times B^{-2}=C(mod M)$<br>
$A\times B=C\times B^{2}(mod M)$<br>

### 예제1 : 피보나치 수열의 나머지
```cpp
#include<iostream>
#define MOD 1000000007

using namespace std;

int main()
{
    int N,pre=1,cur=1,temp;
    cin>>N;
    for(int i=2; i<N; i++)
    {
        temp=(pre+cur)%MOD;
        pre=cur;
        cur=temp;
    }
    if(N<=2)
        cout<<"1";    
    else
        cout<<temp%MOD;
    return 0;
}
```
책에서 요구한 8691200번째의 피보나치 수열은 자리수만 209자리임으로 int 자료형으로는 절대 풀 수 없으나, 모듈러 역수를 알고있다면 구할 수 있습니다.<br>

### 예제2 : a의 b 제곱과 나머지
```cpp
#include<iostream>
#include<queue>

#define MOD 1000000007

using namespace std;

int main()
{
    long long int N,M,temp=1;
    cin>>N>>M;
    queue<int> binary;
    while(M>0)
    {
        binary.push(M%2);
        M/=2;
    }
    while(!binary.empty())
    {
        int bit =binary.front();
        binary.pop();
        if(bit==1)
        {
            temp*=(N%MOD);
        }
        N*=N;
    }
    cout<<temp%MOD;
    return 0;
}
```
승수의 경우 $A^{13}$을 구하는 방식을 $A^8 \times A^4 \times A^1$로 푸는 것이 메모리도 덜 차지하기에 승수를 2진수화 한 뒤 그 값에 따라 곱하여 주었습니다. 

### 예제3 : 경로의 경우의 수에 나머지 적용하기 


### RSA 암호
1. 송신자가 수신자의 공개키를 얻는다.
2. 보내고자 하는 메일을 숫자로 변환한후, e의 제곱을 한 후 n으로 나눈 나머지를 수신자에게 보냅니다.
3. $x^d$을 n으로 나눈 나머지를 구하면 $x^d=m^{ed} \equiv m(modn)$이기에 원래 문장으로 복원 가능합니다.


## 행렬의 거듭제곱
### 행렬이란?
수를 가로와 세로로 배열한 것. N행 M열의 크기를 가지는 행렬을 $N\times M$ 행렬이라고 함

### 행렬의 덧셈
$\begin{pmatrix}
    3,1,4,1\\
    5,9,2,6\\
    5,3,5,8
\end{pmatrix}+
\begin{pmatrix}
    1,4,1,4\\
    2,1,3,5\\
    6,2,3,7
\end{pmatrix}=
\begin{pmatrix}
    4,5,5,5\\
    7,10,5,11\\
    11,5,8,15
\end{pmatrix}
$
<br>
이 처럼 각각의 행과 열의 인덱스가 같은 것끼리 더한값을 가지게 됩니다. 뺄셈 역시 동일합니다.<br>

### 행렬의 곱셈
행의 곱의 경우는 행렬 A의 열과 행렬 B의 행의 수가 같을 때만 계산 할 수 있으며, 행렬끼리의 곱의 행렬은 A행렬의 행인 N과 B행렬의 열인 M의 크기를 가지게 됩니다. 이해를 돕기위해 다음의 예시를 보겠습니다.
<br>
$\begin{pmatrix}
    3,1,4,1,5\\
    9,2,6,5,3\\
    5,8,9,7,9\\
    3,2,3,8,4
\end{pmatrix}+
\begin{pmatrix}
    1,4,1\\
    4,2,1\\
    3,5,6\\
    2,3,7\\
    3,0,9
\end{pmatrix}=
\begin{pmatrix}
    36,37,80\\
    54,85,109\\
    105,102,197\\
    48,55,115
\end{pmatrix}
$<br>
A 행렬은 4행 5열, B 행렬은 5행 3열입니다. B 행열의 행과 A 행열의 열의 크기가 같기에 행렬의 곱을 할 수 있습니다.<br>
또한 결과 행렬의 값은 어떤 식으로 정해지는 가에 대해서는 예시로 결과 행렬의 2행 2열의 값을 구하는 방법은 다음과 같습니다.<br>

1. A 행렬의 2행인 [9, 2, 6, 5, 3] 과 B 행렬의 2열인 [4, 2, 5, 3, 0]을 찾습니다.
2. 각 배열의 같은 인덱스끼리 곱한 배열을 구합니다.<br>
   [ 9,  2,  6,  5,  3]<br>
   [ 4,  2,  5,  3,  0]<br>
   [36,  4, 30, 15,  0]
3. 결과 배열의 합이 결과 행렬의 2행 2열의 값입니다.<br>
   36+4+30+15+0=85

행렬의 곱에서 주의할 점은 교환법칙($AB=BA$)가 성립하지 않습니다<br>
하지만 결합법칙($A(BC)=(AB)C$)은 성립합니다.

### 행렬의 거듭제곱
이것이 성립되기 위해서는 피 연산자는 행과 열이 같아야 합니다. 또한 A 행렬의 제곱을 $A^2$이라고 할 때,<br>
$A^2\times A^2=A^4$<br>
임을 알 수 있는데, 이는 결합법칙이 성립하기에 가능한 것입니다.

### 행렬로 피보나치 수열의 아래 9자리 숫자 구하기
$\begin{pmatrix}
    1,1\\
    1,0
\end{pmatrix}
$
<br>
행렬의 거듭제곱을 하고 $A^n$의 2번째 행의 합을 구하면 n+1번째 피보나치를 구할수 있습니다.

### 빠르게 계산하는 법
1. 구하고자하는 인덱스를 고르고, 그 값에서 1을 뺀 값을 이진법으로 변환합니다.
    ex) 12= 1100(8+4)
2. 행렬의 거듭제곱을 통하여 이진법으로 분류된 행렬을 구합니다<br>
    $A^8\times A^4=A^{12}$
    <br>
    $\begin{pmatrix}
    34,21\\
    21,13
    \end{pmatrix}
    \times
    \begin{pmatrix}
    5,3\\
    3,2
    \end{pmatrix} =
    \begin{pmatrix}
    233,144\\
    144,89
    \end{pmatrix}$
    <br>
3. 행렬의 2번째 항의 합을 구하면 정한 인덱스에 맞는 피보나치의 값을 계산할 수 있다.
   <br>$144+89=233$<br>

### 구현
```cpp
#include <iostream>
#include <vector>
#include<queue>

#define MOD 1000000007

using namespace std;

struct Matrix 
{
    vector<vector<long long int>> data;
};

Matrix multiply(const Matrix& A, const Matrix& B) 
{
    Matrix result;
    result.data.resize((long long int)2, vector<long long int>(2, 0));

    for (int i = 0; i < 2; ++i) {
        for (int j = 0; j < 2; ++j) {
            for (int k = 0; k < 2; ++k) 
            {
                result.data[i][j] += (A.data[i][k] * B.data[k][j])%MOD;
            }
        }
    }

    return result;
}

int main() 
{
    int N;
    cin >> N;

    Matrix begin;
    begin.data = {{1, 1}, {1, 0}};

    queue<int> binary;
    N--;
    while(N>0)
    {
        binary.push(N%2);
        N/=2;
    }
    Matrix result = begin;
    Matrix temp;
    temp.data={{1,0},{0,1}};
    while(!binary.empty())
    {
        int bit =binary.front();
        binary.pop();
        if(bit==1)
        {
            temp = multiply(temp,result);
        }
        result = multiply(result,result);

    }

    cout << (temp.data[1][0] + temp.data[1][1])%MOD;

    return 0;
}

```

1. struct Matrix - 이차원 배열을 저장할 구조체
2. Matrix multiply(const Matrix& A, const Matrix& B) - 입력받은 행렬 두개를 곱하여 주는 함수
3. 입력한 숫자를 2진수의 단위로 쪼개는 작업
```cpp
while(N>0)
    {
        binary.push(N%2);
        N/=2;
    }
```
4. $2\times 2$ 행렬에서 곱했을시 그대로인 행렬
```cpp
temp.data={{1,0},{0,1}};
```
5. 
```cpp
while(!binary.empty())
    {
        int bit =binary.front();
        binary.pop();
        if(bit==1)
        {
            temp = multiply(temp,result);
        }
        result = multiply(result,result);

    }
```

6. cout << result.data[1][0] + result.data[1][1]; - 결과 출력
7. N--; - --인 이유는 begin함수에 하나가 이미 존재하기 때문