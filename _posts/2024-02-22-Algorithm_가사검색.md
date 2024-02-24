---
published: true
title: "-가사검색" 
categories: Algorithm 
tag: [algorithm, Programmers, PriorityQueue] 
toc: true
author_profile: false 
  
---



##### 문제(카카오)

가사에 사용된 모든 단어들이 담긴 배열 `words`와 찾고자 하는 키워드가 담긴 배열 `queries`가 주어질 때, 각 키워드 별로 매치된 단어가 몇 개인지 **순서대로** 배열에 담아 반환

검색 키워드는 와일드카드 문자인 `'?'`가 하나 이상 포함돼 있으며, `'?'`는 각 검색 키워드의 접두사 아니면 접미사 중 하나로만 주어집니다.

<br>



##### 핵심 아이디어 



<br>



##### 내 코드 : 처리하지 못한 경우 재검사 

```java 
import java.util.*;

class Job implements Comparable<Job> {
    int startTime;
    int duration;

    public Job(int startTime, int duration) {
        this.startTime = startTime;
        this.duration = duration;
    }

    @Override
    public int compareTo(Job other) {
        return this.duration - other.duration;
    }
}

class Solution {
    private int totalCompletionTime = 0;
    private int endTime = 0;
    private PriorityQueue<Job> jobQueue = new PriorityQueue<>();
    private boolean[] completed;

    public int solution(int[][] jobs) {
        Arrays.sort(jobs, Comparator.comparingInt((int[] job) -> job[0]).thenComparingInt(job -> job[1]));

        completed = new boolean[jobs.length];
        processJobs(jobs);

        // 처리 x 요청 있는 경우 재검사 
        for (int i = 0; i < completed.length; i++) {
            if (!completed[i]) {
                completed[i] = true;
                jobQueue.add(new Job(jobs[i][0], jobs[i][1]));
                processJobs(jobs);
            }
        }
        return totalCompletionTime / jobs.length;
    }

    private void processJobs(int[][] jobs) {
        while (!jobQueue.isEmpty()) {
            Job job = jobQueue.poll();

            if (job.startTime > endTime)
                endTime = job.startTime + job.duration;
            else
                endTime += job.duration;

            totalCompletionTime += endTime - job.startTime;

            for (int i = 0; i < jobs.length; i++) {
                if (completed[i]) continue;
                if (jobs[i][0] < endTime) {
                    completed[i] = true;
                    jobQueue.add(new Job(jobs[i][0], jobs[i][1]));
                }
            } // for 
        } // while 
    } // processJobs
}

```

<br> 



##### 처리할 요청이 없는 경우도 한번에 고려

```java
import java.util.* ; 

class Solution {
    public int solution(int[][] jobs) {
        int answer = 0;

        // 작업이 요청되는 시점 기준으로 오름차순 정렬
        Arrays.sort(jobs, (o1, o2) -> o1[0] - o2[0]);

        // 작업의 소요시간 기준으로 오름차순 우선순위 큐 
        PriorityQueue<int[]> pq = new PriorityQueue<>((o1, o2) -> o1[1] - o2[1]);

        int jobs_index = 0; // 작업 배열 인덱스
        int finish_job = 0; // 처리 완료된 작업 개수
        int end_time = 0; // 작업 처리 완료 시간

        while(true) {
            if(finish_job == jobs.length) break; // 모든 작업을 처리했다면 종료

            // 이전 작업 처리 중 요청된 작업 add
            while(jobs_index < jobs.length && jobs[jobs_index][0] <= end_time) {
                pq.add(jobs[jobs_index++]);
            }

            if(!pq.isEmpty()) { // 이전 작업 처리 중 요청된 작업이 있는 경우
                int[] job = pq.poll();
                answer += end_time - job[0] + job[1]; // 작업 요청부터 종료까지 걸린 시간 추가
                end_time += job[1]; // 작업 처리 완료 시간 갱신
                finish_job++; // 처리 완료된 작업 개수 1 증가
            } else { // 이전 작업 처리 중 요청된 작업이 없는 경우
                end_time = jobs[jobs_index][0]; // 다음 작업 요청 시점으로 갱신
            }
        }
        return answer / jobs.length;
    }
}
```

<br>



