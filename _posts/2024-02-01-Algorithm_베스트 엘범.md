---
published: true
title: "Hash - 베스트 엘범" 
categories: Algorithm 
tag: [algorithm, 프로그래머스, 해시] 
toc: true
author_profile: false 

---



##### 문제 

매개변수로 각 고유번호 i 번째 노래 각각의 장르 정보가 주어지는 배열 genres와 노래별 재생 횟수를 나타내는 정수 배열 plays가 주어진다. 

 하나의 엘범에, 장르별로 가장 많이 재생된 노래를 최대 2곡씩 뽑아 엘범을 구성하려한다. 

1. 속한 노래가 많이 재생된 장르를 먼저 수록합니다.
2. 장르 내에서 많이 재생된 노래를 먼저 수록합니다.
3. 장르 내에서 재생 횟수가 같은 노래 중에서는 고유 번호가 낮은 노래를 먼저 수록합니다.



##### Step 1. 문제 접근 

전체 노래 재생 횟수가 가장 많은 장르 순서대로 가장 재생횟수가 많은 곡 두곡씩을 뽑는다. 



##### Step 2. 선택한 알고리즘 

해시맵과 정렬  



##### Step 3. 선택 이유 

* 곡의 장르를 하나씩 검증하면서 

* 각 장르를 key로 하는 HashMap<장르, 곡 HashMap>를 생성 (만약 key로 조회되는 장르가 없으면)

* Hash에서 해당 장르의 list을 가져와서 Entry형태로 삽입하고 

* 재생 횟수 누적 합을 구해 내림차순으로 배열에 장르를 넣는다. 

  

##### Step 4. 구현 (코드)

```java
import java.util.* ; 

class Solution {
    public int[] solution(String[] genres, int[] plays) {
        List<Integer> answer = new ArrayList<>();

        Map<String, Map<Integer, Integer>> maps = new HashMap<>(); 
        // <장르, <곡번호, 재생횟수>>로 해시맵 생성 

        for (String genre : genres) { // 각 장르마다 
            maps.put(genre, new HashMap<>()); // 곡정보(곡 리스트)를 저장할 HashMap 생성 후 넣어줌  
        }

        for (int i = 0; i < genres.length; i++) { // 곡을 하나씩 검사하면서 
            maps.get(genres[i]).put(i, plays[i]); // 해당 장르의 곡 리스트를 가져와서 곡 정보, 즉 <고유번호, 재생횟수> 를 추가해줌 => 장르별 곡 리스트 완성 
        
        }

        Map<String, Integer> maps2 = new HashMap<>(); // <장르, 총재생횟수>를 저장할 두번째 HashMap 생성 

        for (String s : maps.keySet()) { // 장르별 곡 리스트 map에서 key값들, 즉 장르들만 가져옴 
            int count = maps.get(s) // 각 장르별 곡리스트, <곡번호, 재생횟수>를 가져와서
                .values() // 재생횟수만 뽑아옴 
                .stream().mapToInt(n -> n) // 곡들의 재생횟수들을 스트림으로 변환해서
                .sum() ;  // 합친값을 반환 => 총 재생횟수 
            
            maps2.put(s, count); // <장르, 총 재생횟수> 를 map에 삽입
        }

        List<String> keySetList = new ArrayList<>(maps2.keySet()); 
        // key값들 즉 곡의 장르들 가져와서 List생성 (for 정렬)

        keySetList.sort((o1, o2) -> (maps2.get(o2).compareTo(maps2.get(o1))));
        // 그 장르들을 총 재생횟수 순으로 정렬 (o1, o2 는 장르 즉 key값, 정렬 기준은 그 키값을 기준으로 maps2에서 가져온 value값 즉 총재생횟수!)

        
        keySetList.forEach(s -> { // 총재생횟수 순으로 정렬된 장르들을 순서대로 가져오면서, 즉 장르들을 하나씩 검사하면서 
            Map<Integer, Integer> songList = maps.get(s); // <장르, 곡리스트>가 저장된 HashMap에서 해당 장르의 곡리스트(<곡번호, 재생횟수>) 를 가져오고 

            List<Integer> integers = new ArrayList<>(songList.keySet());
            // 곡번호들로 새로운 list 생성 (곡 리스트를 재생횟수(value 값) 순으로 정렬하기 위해 Map을 List로 변환 )
            
            integers.sort((o1, o2) -> (songList.get(o2).compareTo(songList.get(o1))));
             // 곡리스트의 곡번호들을 재생횟수 순으로 정렬 (o1, o2 는 곡번호 즉 key값, 정렬 기준은 그 키값을 기준으로 가져온 재생횟수값 - 내림차순) 

            if (integers.size() <= 1) { // 해당 장르의 곡 갯수가 1개 이하면 
                answer.add(integers.get(0)); // 하나만 추가 (곡 번호)
            } else { // 두개 이상이면
                answer.add(integers.get(0)); // 가장 많은 두개 추가 (곡 번호)
                answer.add(integers.get(1));
            }
        }); // x 각 장르별로 반복 
        return answer.stream().mapToInt(i -> i).toArray(); 
        // 곡번호들로 구성된 ArrayList를 Array로 변환해서 반환
    }
}
```











