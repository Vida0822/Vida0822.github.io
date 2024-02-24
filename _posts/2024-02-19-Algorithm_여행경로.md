---
published: true
title: "DFS-여행경로" 
categories: Algorithm 
tag: [algorithm, Programmers, Graph, BackTracking, DFS] 
toc: true
author_profile: false 
  
---



##### 문제

주어진 항공권을 모두 이용하여 여행경로를 짜려고 합니다. 항상 "ICN" 공항에서 출발합니다.

항공권 정보가 담긴 2차원 배열 tickets가 매개변수로 주어질 때, 방문하는 공항 경로를 배열에 담아 return

- 주어진 항공권은 모두 사용해야 합니다.
- 만일 가능한 경로가 2개 이상일 경우 알파벳 순서가 앞서는 경로를 return

<br>



##### BFS 풀이 --> ❌

```java 
import java.util.*; 
class Solution {
    public static HashMap<String, ArrayList<String>> graph ; 
    public static ArrayList<String> path = new ArrayList<>() ; 
    
    public String[] solution(String[][] tickets) {
        // 최소 비용 신장 트리 문제 
        // 프림 알고리즘 
        // But, key 값이 문자열인게 주의할 점 
        // 또한, 한번 방문한 곳을 재방문 할 수 있음
        // 항공권을 다 사용해야함 
        
        // part 1. 그래프 만들기 
        graph = new HashMap<>() ; 
        for(int i = 0 ; i < tickets.length ; i++){
            if(!graph.containsKey(tickets[i][0]))
                graph.put(tickets[i][0], new ArrayList<>()) ; 
            graph.get(tickets[i][0]).add(tickets[i][1]) ; 
        }
        
        // part 2. bfs 수행 + 이동 경로 기록 
        /*
        bfs 안되는 이유 : '경로'기 때문에 타고 들어오는 이어지는 흐름을 가져가야함. 이렇게 하면 실제로 이어지지 않은 공항끼리 이동하는게 됨 
        */
        PriorityQueue<String> pq = new PriorityQueue<>() ; 
        pq.add("ICN") ; 
        while(!pq.isEmpty()){
            String airport = pq.poll() ;  // ICN 
            path.add(airport) ; 
            
            ArrayList<String> destinations = graph.get(airport) ; 
            if(destinations == null)
                continue ; 
            for(int i = 0; i < destinations.size(); i++){
                String nextAirport = destinations.get(i);
                pq.add(nextAirport);
                destinations.remove(i);  // 요소를 직접 제거
                i--;  // 제거 후에 인덱스 조정
            }
        } // while
        
        // part 3. 경로 반환 
        String[] answer = path.toArray(new String[0]) ;
        return answer ; 
    }
    
}
```

<br> 



##### DFS + Backtracking 

```java
import java.util.*; 

class Solution {
    public static HashMap<String, ArrayList<String>> graph ; 
    public static ArrayList<String> path = new ArrayList<>() ; 
    
    public String[] solution(String[][] tickets) {
        // part 1. 그래프 만들기 
        graph = new HashMap<>() ; 
        for(int i = 0 ; i < tickets.length ; i++) {
            String from = tickets[i][0];
            String to = tickets[i][1];
            if(!graph.containsKey(from))
                graph.put(from, new ArrayList<>()) ; 
            graph.get(from).add(to);
        }
        
        // 도착지를 알파벳 순서로 정렬
        for(ArrayList<String> destinations : graph.values()) {
            Collections.sort(destinations, new Comparator<String>(){
                @Override
                public int compare(String s1, String s2){
                    return s1.compareTo(s2); 
                }
            });
        }
        
        // part 2. dfs 수행 + 이동 경로 기록 
        dfs("ICN", tickets.length) ; 
        
        // part 3. 경로 반환 
        String[] answer = path.toArray(new String[0]) ;
        return answer ; 
    }
    
    public static boolean dfs(String airport, int ticketsLeft){
        path.add(airport) ; 
         
        if(ticketsLeft == 0) // 모든 티켓을 사용했을 때
            return true;
        
        ArrayList<String> destinations = graph.get(airport) ; 
        if(destinations == null)
            return false; // 아직 써야하는 티켓 수가 남았는데 현재 노드에서 갈 수 있는 곳이 없으면 티켓 반환
        
        for(int i = 0; i < destinations.size(); i++){
            String nextAirport = destinations.get(i);
            destinations.remove(i); // 사용한 티켓 제거 (즉, 도착지 후보에서 제거)
            
            if(dfs(nextAirport, ticketsLeft - 1)) // 재귀 호출 : 해당 도착지로 이동해 dfs를 수행해 최종적으로 모든 타켓을 사용했으면
                return true; // true 반환 (연쇄적으로 타고 올라감)
            
            destinations.add(i, nextAirport); //  해당 도착지로 이동해 dfs를 수행했는데 모든 타켓을 사용하지 못했으면, 해당 도착지로 이동 x (티켓 반환, 다시 도착지 후보에 추가 )
        }
        
        // 그 어떤 인접노드들도 최종적인 경로가 되지 못할 경우 현재 노드도 반환하고 backtracking해서 새로운 경로를 찾음 
        path.remove(path.size() - 1);
        return false;
    }
}

```

<br>



##### 그래프 X + 완전 탐색

* 매 dfs 탐색마다 사용하지 않은 '티켓'을 검사 : 그래프를 만들 필요 없이 검사 티켓의 출발지가 현재 노드인지만 check
* 방문 배열(특정 티켓 사용 여부) 배열 사용 
* '가능 경로'들을 담는 리스트 사용 : 완전 탐색으로 모든 가능한 경로들을 담는다 
* 최종적으로 해당 리스트 알파벳순 정렬해서 맨 앞 경로 반환 

```java
import java.util.*;
class Solution {

    static ArrayList<String> list = new ArrayList<>();
    static boolean useTickets[];

    public String[] solution(String[][] tickets) {
        useTickets = new boolean[tickets.length];
        
        dfs(0, "ICN", "ICN", tickets);

        Collections.sort(list); // 가능한 경로들을 '알파벳' 순으로 정렬 
        return list.get(0).split(" "); // 가장 앞에있는 경로 반환
    }

    static void dfs(int depth, String now, String path, String[][] tickets){
        if (depth == tickets.length) { // dfs 깊이가 사용 티켓 개수와 같으면 (만약 갈 수 없으면 depth가 깊어지지 않음)
            list.add(path); // 해당 경로를 '경로 리스트'에 추가
            return; // backtracking 
        }

        // 사용하지 않은 티켓 검사 ** (아이디어 중요)
        for (int i = 0; i < useTickets.length; i++) {
            if (!useTickets[i] && now.equals(tickets[i][0])) { // 인접한 공항이고, 해당 티켓을 사용하지 않았으면
                useTickets[i] = true; // 해당 티켓 사용 표시 (자식 노드 dfs 탐색위해)
                String newPath = path + " " +tickets[i][1] ; // 도착지만 문자열로 이어서 '경로' 만듬
                dfs(depth+1, tickets[i][1], newPath , tickets);  // 해당 도착지로 이동해 dfs 수행 
                useTickets[i] = false; // 형제 노드쪽도 탐색해야하니 다시 복구  
            }
        } // for 
    }
}
```

<br>





##### PriorityQueue로 정렬 생략

```java
import java.util.*;

class Solution {
    public Queue<String> pq = new PriorityQueue<>(); // 자동으로 문자열 알파벳순 정렬 기준 적용 (default)
    public boolean[] visited;
    
    public String[] solution(String[][] tickets) {
        visited = new boolean[tickets.length];
        dfs(tickets, "ICN", 0, "ICN");
        String[] answer = pq.peek().split(",");
        return answer;
    }
    
    public void dfs(String[][] tickets, String currentCity, int cnt, String path){
        if(cnt == tickets.length){
            pq.add(path);
            return;
        }
        for(int i=0;i<tickets.length;i++){
            if(!visited[i] && currentCity.equals(tickets[i][0])){
                visited[i] = true;
                dfs(tickets, tickets[i][1], cnt+1, path +","+ tickets[i][1]);
                visited[i] = false;
            }
        }
    }
}
```

<br>









