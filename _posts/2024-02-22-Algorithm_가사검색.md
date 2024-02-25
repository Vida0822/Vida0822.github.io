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
    class Trie{
        // 필드 
        Map<Integer,Integer> lenMap = new HashMap<>() ; 
        Trie[] child = new Trie[26] ; 
        
        // 메서드
        void insert(String str){ 
            Trie node = this; 
            int len = str.length() ; 
            lenMap.put(len, lenMap.getOrDefault(len,0)+1) ; 
            
            // 한글자씩 Trie에 삽입 
            for(char ch : str.toCharArray()){
                int idx = ch-'a' ; 
                if(node.child[idx] == null)
                    node.child[idx] = new Trie() ; 
                
                node = node.child[idx] ; 
                node.lenMap.put(len, node.lenMap.getOrDefault(len,0)+1); 
            }
        } // insert 
        
				// 특정 단어가 Trie에 있는지 확인 
        int find(String str, int i){
            if(str.charAt(i) == '?')
                return lenMap.getOrDefault(str.length(), 0) ; 
            int idx = str.charAt(i) - 'a' ; 
            return child[idx] == null? 0 : child[idx].find(str, i+1) ; 
        } // find 
    } // Trie
    
		// 문자열을 뒤집는 메서드 
    String reverse(String s){
        return new StringBuilder(s).reverse().toString() ; 
    }
    
    public int[] solution(String[] words, String[] queries) {
        Trie front = new Trie() ; 
        Trie back = new Trie() ; 
        /*
        역방향 Trie를 만드는 이유 : queries에서 첫글자가 '?'면 ?를 자식으로 갖는 Trie를 만들기 힘들어서?? 
        */
        // 후보 단어들 Trie에 삽입 (insert)
        for(String word : words){
            front.insert(word);  
            back.insert(reverse(word)) ; 
        }
        
        // queries에 매치되는 단어들 찾기 
        return Arrays.stream(queries).mapToInt(
            query -> query.charAt(0) == '?' ?
                    back.find(reverse(query),0) : 
                    front.find(query,0)).toArray() ; 
    }
}
```

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
            else ans.add(lowerBound(list, query.replace('?', Character.MAX_VALUE))
                    - lowerBound(list, query.replace("?", "")));
        }
        return ans;
    }

    int lowerBound(List<String> list, String str) {
        int s = 0, e = list.size();

        while (s < e) {
            int m = (s + e) / 2;

            if (str.compareTo(list.get(m)) <= 0)
                e = m;
            else s = m + 1;
        }
        return s;
    }

    String reverse(String s) {
        return new StringBuilder(s).reverse().toString();
    }
}
```

<br>



