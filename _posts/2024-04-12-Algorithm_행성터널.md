---
published: true
title: "Kruscal-행성 터널" 
categories: Algorithm 
tag: [algorithm,Shortest Kruscal, Union, Find] 
toc: true
author_profile: false 
  
---



##### 문제

[2887번: 행성 터널 (acmicpc.net)](https://www.acmicpc.net/problem/2887)

<br>



##### 핵심 아이디어

> 크루스칼 알고리즘 

모든 행성을 잇는 최단 경로를 구하는 크루스칼 알고리즘을 사용하라고 명시적으로 문제에서 언급 

<br>



##### Point

알고리즘을 떠올리는 것보다 알고리즘을 적용하는 준비가 더 어려운 문제. 

보통 간선의 정보가 주어지는 다른 문제들과 달리 이 문제는 각 행성의 좌표만 준다.

뿐만 아니라 해당 행성들은 모두 이어질 수 있는데 n = 1000000으로 매우 크기 때문에 

모든 행성을 각각 이어 간선을 만들면 시간 초과가 뜬다 (nC2) . 



따라서 문제의 조건 - 최단 거리 =  min(|xA-xB|, |yA-yB|, |zA-zB|) 임을 이용해

각 좌표들을 x, y, z 기준으로 각각 일렬 정렬하고  인접한 두 행성끼리만 간선을 긋는다. 

그 이유는 두 좌표 사이 행성이 하나 더있다면 그 두 간선은 어차피 최소 간선이 될 수 없기 때문이다. 

(일렬로 나열해서 그냥 그어도 n-1개를 만족한다) 



그렇게 x, y, z 별로 각각 그은 간선을 모두 ArrayList에 저장한 후 가중치 기준으로 정렬, 

하나씩 뽑으며 크루스칼 알고리즘 & Union-Find 연산을 실행한다. 

<br>  

##### ❌ 실패 코드 (시간 초과)

모든 행성에 간선을 그음 

```java
public class Main{
    public static void main(String[] args){
            :
            :
        
        // 그래프 만들기 
        ArrayList<Edge> edges = new ArrayList<>() ; 
        for(int i = 0 ; i < n ; i++){
            for(int j = i+1 ; j < n ; j++){
                int dx = Math.abs(xyz[j][0] - xyz[i][0]) ; 
                int dy = Math.abs(xyz[j][1] - xyz[i][1]) ; 
                int dz = Math.abs(xyz[j][2] - xyz[i][2]) ; 
                
                edges.add(new Edge(i, j, Math.min(Math.min(dx, dy) , dz ))) ; 
       }
    }
}
```

<br>



##### ✔ 코드

```java
package graph; 

import java.util.*; 
class Dist implements Comparable<Dist>{
    int start ;
    int end ; 
    int cost ;

    public Dist(int start, int end, int cost){
        this.start = start ; 
        this.end = end ; 
        this.cost = cost ; 
    }
    
    @Override
    public int compareTo(Dist other){
        return this.cost - other.cost ; 
    }
}

public class 행성터널{
    public static void main(String[] args){
        Scanner sc = new Scanner(System.in) ; 
        int n = sc.nextInt() ; 
        int[][] xyz = new int[n][4] ; 
        
        // 그래프 만들기 ** : 모든 행성은 연결 될 수 있다
        // 보통은 특정 출발 노드 -> 도착 노드 + 비용이 주어지는데 
        // 여기선 출발 노드의 위치, 도착 노드의 위치로 각 노드간의 비용을 내가 구해야한다 
        // 좌표 입력받기 
        for(int i = 0 ; i < n ; i++){
        	xyz[i][0] = i ; 
            xyz[i][1] = sc.nextInt() ; 
            xyz[i][2] = sc.nextInt() ; 
            xyz[i][3] = sc.nextInt() ; 
        }
        
        // 그래프 만들기 
        ArrayList<Dist> Dists = new ArrayList<Dist>(); 
        for(int i = 1 ; i <= 3 ; i++) {
        	int p = i ; 

        	Arrays.sort(xyz, (o1, o2) -> (o1[p] - o2[p]));
        	
        	for(int j = 0 ; j < n-1 ; j++) {
        		int p1 = xyz[j][0] ; 
        		int p2 = xyz[j+1][0] ; 
        		int cost = Math.abs( xyz[j][i] - xyz[j+1][i] ) ; 
        		
        		Dists.add(new Dist(p1,p2, cost)) ; 
        	}	
        }
        
        // 간선 정렬 
        Collections.sort(Dists) ; 
        
        // 테이블 만들기 
        int[] parent = new int[n] ; 
        for(int i = 0 ; i < n ; i++){
            parent[i] = i ; 
        }
        
        // 크루스칼 알고리즘 수행 
        int answer = 0 ; 
        for(int i = 0 ; i < Dists.size() ; i++){
            int start = Dists.get(i).start ; 
            int end = Dists.get(i).end ; 
            int cost = Dists.get(i).cost ; 
            
            if(find(parent, start) == find(parent, end))
                continue ; 
            
            union(parent , start, end); 
            answer += cost ; 
        }
        System.out.println(answer) ; 
     }
    
    public static int find(int[] parent, int x){
        if(parent[x] == x)
            return x ; 
        return parent[x] = find(parent, parent[x]) ; 
    }
    
    public static void union(int[] parent, int x, int y){
        int root_x = find(parent, x) ; 
        int root_y = find(parent, y) ; 
        
        if(root_x != root_y)
            parent[root_y] = root_x ; 
    }
    
}
```

<br>


