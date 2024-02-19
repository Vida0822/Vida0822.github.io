---
published: true
title: "Tree-자동완성" 
categories: Algorithm 
tag: [algorithm, Programmers, Trie] 
toc: true
author_profile: false 


---



##### 문제 : 프로그래머스 Level 4 (카카오 기출문제)

##### 구현(코드)

```java
import java.util.HashMap;
import java.util.Map;

class Solution {

    public class Node {
        Map<Character, Node> child = new HashMap<>(); // 특정 문자의 자식 노드들을 저장하는 HashMap 
        Node before = null; // 부모 계층 노드 
        Boolean isEnd = false; // leaf 노드인지 확인 
    }

    public class Trie { // Trie 구조를 갖는 클래스 
        public Node root = new Node(); // 루트 노드 

        /*
        insert : 주어진 문자열을 Trie에 삽입하는 메서드 
        --> 문자열의 각 글자를 Trie에 추가하면서 해당 노드의 자식 노드 맵에 추가
        */
        public void insert(String input) { 
            Node node = this.root; // 빈 기준 노드를 가져와서 첫번째 계층 노드로 설정 
            for (int i = 0; i < input.length(); i++) {
                Node before = node; // i-1번째에 노드로 만들어준 문자를 before에 저장
                node = node.child // i-1번째 노드의 자식들중에 
                    .computeIfAbsent(input.charAt(i) // i번째 글자가 없으면 
                                     , k -> new Node()); // 해당 문자로 새로운 노드를 만들어 child에 넣어준다
                /*
                Map.computeIfAbsent(lamda) : 특정 키에 해당하는 값이 존재하는지 확인한 후, 없으면 새로 만들어서 넣어주는 메서드 
                */
                node.before = before; // i-1번째 노드를 해당 글자 노드의 부모 노드로 설정 
            }
            node.isEnd = true; // 반복문이 끝나면 문자열 전체 검사 완료 --> leaf 노드 표시 
        }

        /*
        calInputCnt : 설계된 트라이를 바탕으로 입력받은 단어를 완성시키기 위해 필요한 최소 글자수를 구하는 메서드
        */
        public int calInputCnt(String input) {
            Node node = this.root;

            //마지막 노드로 이동 
            for (int i = 0; i < input.length(); i++) { // 한글자씩 검사하면서 
                node = node.child // 노드의 자식들중에 
                    .getOrDefault(input.charAt(i), null); // i 번째 글자가 있으면 해당 노드를 반환하고 없으면 null 반환 --> 자식으로 타고 들어감 
            }

            //1. 마지막 노드가 leaf 노드가 아닐 경우 <-> Trie에 반영된 단어가 아닐경우 
            if (node.child.size() != 0) 
                return input.length();  // 전체를 다 작성해야 검색 가능 

            //2. 마지막 노드가 leaf 노드일 경우 <-> Trie에 반영된 단어를 검색한 경우 
            int reduce = 0; // 생략 가능한 검색 횟수 
            
            while(true) { // 부모 노드들을 따라가면서 최소 입력 횟수를 계산
                node = node.before; // 해당 노드의 부모노드를 참조해 올라가면서 
                
                if (node == null) {  // 부모노드가 없으면
                    reduce--; // 다시 아래로 내려가주고 (해당 글자는 직접 검색 횟수에 포함시켜야함)
                    break; // 반복문 종료 
                }
                if (node.isEnd == false && node.child.size() == 1) 
                    // 부모노드가 리프노드가 아닌데 자식 노드가 하나, 즉 현재 검사하는 나밖에 없을 때  
                    reduce++; // 검색 횟수를 1회 빼준다 
                else
                    break;            
            }
            return input.length() - reduce; // 전체 문자 갯수에서 생략가능한 검색횟수를 빼준 값을 리턴 
        } // calInputCnt
    } // Trie

    public int solution(String[] words) {
        Trie trie = new Trie();
        //트라이 설계 (모든 단어들을 Trie 자료구조에 반영)
        for (int i = 0; i < words.length; i++)
            trie.insert(words[i]);

        //각 단어의 최소 입력 횟수 계산
        int result = 0;
        for (int i = 0; i < words.length; i++) {
            result += trie.calInputCnt(words[i]); // 누적 
        }
        return result;
    }
}
```

