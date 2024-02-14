---
published: true
title: "DFS-불량 사용자" 
categories: Algorithm 
tag: [algorithm, Programmers, DFS] 
toc: true
author_profile: false 


---

##### 1. Trie 

—> ❌ : 동일한 형태의 banid가 별개의 노드로 저장이 안됨

```java
import java.util.* ; 
class Solution {
    public class Node{
        Map<Character, Node> child = new HashMap<>() ; 
        Node before = null ; // 부모 노드 
        Boolean isEnd = false ; // leaf 노드인지 확인 
    } // Node
    
    public class Trie{ // Trie 구조를 갖는 클래스
        public Node root = new Node() ;   // 루트 노드 --> 빈 노드로 모든 첫글자 노드의 공통적인 부모 
        
        // 특정 문자열을 Trie 클래스에 반영하는 메서드 
        public void insert(String input){
            Node node = this.root ; 
            for(int i = 0 ; i < input.length() ;i++){
                Node before = node ; // 해당 글자 before 변수에 넣기 
                node = node.child.computeIfAbsent(input.charAt(i) ,key ->new Node()); // <key, new Node() > 
                
                node.before = before ; 
            }
            node.isEnd = true ; // 반복문 끝나면 문자열 전체 검사 완료 --> leaf노드임을 표시 
        } // insert
        
        // 특정 문자열이 Trie(ban ids)에 있는지 
        public int search(String input){
            Node node = this.root ; // 루트노드 가져와서 
            for(int i = 0 ; i < input.length() ; i++){
                if(node != null){
                   node = (node.child.get('*') !=null)? 
                        node.child.get('*') : node.child.getOrDefault(input.charAt(i), null) ; 
                }else
                    break; 
            }
            return (node != null && node.isEnd) ? 1 : 0; 
        } // search
    }
     
    public int solution(String[] user_id, String[] banned_id) {
        // Trie 설계-전체 ID 로 ban 가능한 후보군 목록 생성  
        Trie trie = new Trie() ; 
        for(int i = 0 ; i < banned_id.length ; i++){
            trie.insert(banned_id[i]) ; 
        }
        // Trie 탐색 - ban id로 해당하는 후보군 고르기(이전) -> 변형 : 갯수 구하기 
        int answer = 0 ; 
        for(int i = 0 ; i < user_id.length ; i++){
           answer += trie.search(user_id[i]) ;  
        }
        return answer ; 
    }// solution
}
```

* LinkedHashSet : 해시 테이블에 요소를 저장하는 컬렉션을 생성하지만 해당 HashSet 대응 요소와 달리 요소의 삽입 순서를 유지







##### 2. dfs

```java
import java.util.* ; 
class Solution {
    static HashSet<HashSet<String>> answer ; 
    static String[] user_id ; 
    static String[] banned_id ; 
    
    public int solution(String[] user_id, String[] banned_id) {        
        this.user_id = user_id ; 
        this.banned_id = banned_id ; 
        
        answer = new HashSet<>() ; 
        dfs(new LinkedHashSet<>()) ; 
        return answer.size() ; 
    }// solution
    
    private static void dfs(HashSet<String> hs){
        // 종료 조건 
        if(hs.size() == banned_id.length){
            if(isBanList(hs, banned_id))
                answer.add(new HashSet<>(hs)) ; 
            return ; 
        }
        
        // 인접 노드 방문 
        for(String userId : user_id){
            if(hs.add(userId)){
                dfs(hs) ; 
                hs.remove(userId) ; 
            }
        }
    }
      private static boolean isBanList(HashSet<String> hs, String[] banned_id) {
        int idx = 0;
        for (String userID : hs) {
            String banID = banned_id[idx++];
            if (userID.length() != banID.length()) {
                return false;
            }
            for (int i = 0; i < banID.length(); i++) {
                if (banID.charAt(i) == '*') {
                    continue;
                }
                if (userID.charAt(i) != banID.charAt(i)) {
                    return false;
                }
            }
        }
        return true;
    }
}
```

