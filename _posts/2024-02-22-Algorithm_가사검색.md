---
published: true
title: "-가사검색" 
categories: Algorithm 
tag: [algorithm, Programmers, PriorityQueue] 
toc: true
author_profile: false 
  
---



##### 문제(카카오)

가사에 사용된 모든 단어들이 담긴 배열 `words`와 찾고자 하는 키워드가 담긴 배열 `queries`가 주어질 때, 

각 키워드 별로 매치된 단어가 몇 개인지 **순서대로** 배열에 담아 반환

검색 키워드는 와일드카드 문자인 `'?'`가 하나 이상 포함돼 있으며, `'?'`는 각 검색 키워드의 접두사 아니면 접미사 중 하나로만 주어집니다.

<br>



##### Trie 

문자열 자체가 자료 구조 전체에 분산

특정 노드(A)의 모든 자손은 문자열의 공통 접두사(A)이다. 즉 자손들은 특정 접두사를 공유하는것이 전제된다.  



> Trie 클래스의 구조

필드 : childs, depth, (parent) 

메서드 : insert, find 

<br>



##### 핵심 아이디어 

양방향 Trie와 역방향 Trie를 만들어준다. 

와일드카드(?)는 쿼리의 앞과 뒤에 존재한다. 따라서 뒤집힌 문자열로 구성된 Trie도 생성해야한다. 



<br>



##### 실패 : 선형탐색 

> 시간 복잡도 : O(N^2*M)

* 시간 복잡도  O(queries.length * words.length * 단어길이)  
* 최악의 경우 O(100000 * 100000 * 10000) 

```java
import java.util.* ; 
class Solution {
    public int[] solution(String[] words, String[] queries) {
        
        // 쿼리의 단어만큼 반복
        int len = queries.length ; 
        int[] answer = new int[len] ; 
        for(int i = 0 ; i < len ; i++){
            
            int count = 0 ; 
            for(String word : words ){
                String query = queries[i] ; 
                if(isInBanList(query, word)) 
                    count++ ; 
            }
            answer[i] = count ; 
        }
        return answer ; 
    }
    
    public boolean isInBanList(String query, String word){
        if(query.length() != word.length())
            return false ; 
        
        for(int i = 0 ; i < query.length() ; i++){
            char c = query.charAt(i);
            if(c == '?')
                continue ; 
            
            if(c != word.charAt(i))
                return false; 
        }
        return true; 
    }
}
```

<br>



##### 풀이 1 : Trie 

```java 
import java.util.* ; 
class Solution {
    // 단어들이 '전부' 한글자씩 반영된 문자열 Tree
    class Trie{ 
        // 필드 
        Map<Integer,Integer> lenMap = new HashMap<>() ; // 특정 단어 길이에 해당하는 단어 갯수를 저장한 Map
        Trie[] child = new Trie[26] ; // 서브트리를 저장할 배열 (index는 각 문자 a~z)
        
        // 메서드
        void insert(String str){ 
            Trie node = this; // 루트 노드 
            
            // 단어길이 Map에 정보 저장
            int len = str.length() ; 
            this.lenMap.put(len, lenMap.getOrDefault(len,0)+1) ; 
            
            // 한글자씩 Trie에 삽입 
            for(char ch : str.toCharArray()){
                // 그 문자를 index로 치환해 해당 칸에 서브트리 생성  
                int idx = ch-'a' ;  
                if(node.child[idx] == null)
                    node.child[idx] = new Trie() ;  
                
                node = node.child[idx] ; // 부모갱신 (루트-->서브트리) -> 해당 서브트리 아래로 다음 문자 삽입
                node.lenMap.put(len, node.lenMap.getOrDefault(len,0)+1);  
                // 해당 단어와 관련있는(단어의 문자들) 서브트리에도 길이맵에 정보 반영
                // 탐색할 때 자식 노드의 서브트리에서 find를 호출해 다시 해당 단어를 검색하기 때문에
                // 특정 단어 길이에 해당하는 단어갯수 정보를 서브트리들에도 반영해줘야함 
                // 얘를 static으로 안빼는 이유 : 각 Trie의 lenMap은 '해당 문자들을 가지고 있는' 단어들의 길이를 반영하므로 단순히 단어 길이만 모아놓은 map을 만들면 실제로 query문자를 가지고 있는지는 상관없이 특정 길이의 단어갯수만 반환 
                // 저 조건을 반영하기 위해선 문자(index)에 해당하는 subtree의 lenMap만 길이 정보 반영해줘야함
            }
        } // insert 
        
		// 특정 단어가 Trie에 있는지 확인 
        int find(String str, int i){
             // ?가 나오면 탐색 종료 (그 뒤는 다 '?'임) --> 탐색 단어와 길이가 같은 단어 갯수 반환 
            if(str.charAt(i) == '?')
                return lenMap.getOrDefault(str.length(), 0) ;
            
            // 문자가 ?가 아니면 
            int idx = str.charAt(i) - 'a' ; // 해당 문자를 인덱스로 변환 
            return child[idx] == null? 0 : child[idx].find(str, i+1) ; // 자식 서브트리의 find 재귀 호출 
     		// 일반문자가 나왔다는건 무조건 뒤에 '?'가 한개 이상 등장(query 단어는 무조건 더 김), 
            // 근데 해당 문자를 부모로 갖는 자식문자들이 없다는 건 일치하는 후보 단어가 없다는 뜻 
            // (단어 길이 부족으로 일치하는 문자 없음)
        } // find 
    } // Trie
    
	// 문자열을 뒤집는 메서드 
    String reverse(String s){
        return new StringBuilder(s).reverse().toString() ; 
    }
    
    public int[] solution(String[] words, String[] queries) {
        /*
        역방향 Trie를 만드는 이유 : 만약 첫글자 ? 를 한번에 처리하려하면 Trie의 모든 1층,2층 노드들을 다 고려해줘야함 
        --> 재귀가 너무 많이 호출됨 
        --> 근데 단어를 뒤집음으로써 ?를 끝에만 오게 만든다면 ?가 오는 순간 단어의 길이만 query와 일치하는 단어들 반환하면 됨 
        */
        Trie front = new Trie() ; 
        Trie back = new Trie() ; 
        
        // 후보 단어들 Trie에 삽입 (insert)
        for(String word : words){
            front.insert(word);  
            back.insert(reverse(word)) ; 
        }
        
        // queries에 매치되는 단어들 찾기 
        return Arrays.stream(queries).mapToInt(
            query -> query.charAt(0) == '?' ? // 첫글자가 ?면 
                    back.find(reverse(query),0) :  // 역방향 Trie에서 탐색 
                    front.find(query,0)) //  // 아님 순방향 Trie에서 탐색 
            .toArray() ; // 각 쿼리단어가 적용되는 각각의 단어 갯수들 배열로 반환 
    }
}
```

<br> 

##### Map 함수 

* getOrDefault() : 주어진 키가 맵에 존재하지 않으면, 지정된 기본값(defaultValue)을 반환 

  => 실제로 Map의 value값에는 영향을 주지 않는다 

* computeIfAbsent() : 키를 입력으로 받아 이를 활용해 새 값을 생성하고, 이 값을 해당 키에 설정하는 데 사용된다

  =>  public V computeIfAbsent(K key,  Function<? super K, ? extends V> mappingFunction)

* putIfAbsent() : computeIfAbsent는 Function을 통한 계산된 value를 가지지만, putIfAbsent는 value를 바로 가진다

  => public V putIfAbsent(K key, V value)  

  => key값이 중복됐을 때, 값은 저장되지 않아도 메소드는 호출된다: 메소드 호출 비용이 비싸다면 성능이 저하된다.

<br>



##### 풀이 2 : 이분탐색

```java
import java.util.*;

class Solution {
    public List<Integer> solution(String[] words, String[] queries) {
        Map<Integer, List<String>> frontMap = new HashMap<>();
        Map<Integer, List<String>> backMap = new HashMap<>();

        for (String word : words) {
            int len = word.length();

            frontMap.computeIfAbsent(len, i -> new ArrayList<>()).add(word);
            backMap.computeIfAbsent(len, i -> new ArrayList<>()).add(reverse(word));
        }

        for (int key : frontMap.keySet()) {
            frontMap.get(key).sort(null);
            backMap.get(key).sort(null);
        }

        List<Integer> ans = new ArrayList<>();
        for (String query : queries) {
            List<String> list;
            if (query.charAt(0) == '?') {
                list = backMap.get(query.length());
                query = reverse(query);
            } else
                list = frontMap.get(query.length());

            if (list == null) ans.add(0);
            else ans.add(lowerBound(list, query.replace('?', Character.MAX_VALUE)) // frozz  ex) 5
                    - lowerBound(list, query.replace("?", ""))); // fro  ex) 2 
        }
        return ans;
    }

    int lowerBound(List<String> list, String str) {
        // 5글자인 단어들 list (20개) - abxds, asdfd, bdsdf ,ddfds, froab, frocc, frodd, frogt,froth,jsdfs...
        // str : fro.. & frozz 
        int s = 0, e = list.size();

        while (s < e) {
            int m = (s + e) / 2;

            if (str.compareTo(list.get(m)) <= 0) // 쿼리 단어가 후보 단어(mid)보다 알파벳상 앞서면  
                e = m; // 탐색 범위를 mid 이전으로 줄임 
            else s = m + 1;
        }
        return s; // 5 , 9
    }

    String reverse(String s) {
        return new StringBuilder(s).reverse().toString();
    }
}
```

<br>



##### 참고 사이트 

[[프로그래머스\] 가사 검색 / 2020 KAKAO BLIND RECRUITMENT - JAVA :: 그냥 그냥 블로그 (tistory.com)](https://girawhale.tistory.com/110)

