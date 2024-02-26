---
published: true
title: "Sort-디스크 컨트롤러" 
categories: Algorithm 
tag: [algorithm, Programmers, PriorityQueue] 
toc: true
author_profile: false 
  
---



##### 문제

각 작업에 대해 [작업이 요청되는 시점, 작업의 소요시간]을 담은 2차원 배열 jobs가 매개변수로 주어질 때, 

작업의 요청부터 종료까지 걸린 시간의 평균을 가장 줄이는 방법으로 처리하면 평균이 얼마가 되는지 return 

이때, 하드디스크가 작업을 수행하고 있지 않을 때에는 먼저 요청이 들어온 작업부터 처리한다.

<br>



##### 핵심 아이디어 

* '영향 문제' : 특정 task를 진행했을 때 문제 상황에 영향을 주기 때문에 특정 task를 진행할 때 마다 해당 변화를 적용해야 한다

* 두가지 정렬 기준 사용 : 작업 소요시간 , 작업 요청 시점 

<br>



##### 내 코드 : 처리하지 못한 경우 재검사 

```java 
import java.util.*;

class Job implements Comparable<Job> {
    int startTime; // 요청 시점 
    int duration; // 작업 소요 시간 

    public Job(int startTime, int duration) {
        this.startTime = startTime;
        this.duration = duration;
    }
    @Override
    public int compareTo(Job other) {return this.duration - other.duration;}
}

class Solution {
    private int totalCompletionTime = 0; // 총 작업 시간 
    private int endTime = 0; // 이전 작업 종료시간
    private PriorityQueue<Job> jobQueue = new PriorityQueue<>();
    private boolean[] completed; // 작업 완료 여부 배열

    public int solution(int[][] jobs) {
        Arrays.sort(jobs, Comparator.comparingInt((int[] job) -> job[0]) // 요청순 정렬
																	.thenComparingInt(job -> job[1])); // 소요시간 순 정렬

        completed = new boolean[jobs.length];
        processJobs(jobs);

        for (int i = 0; i < completed.length; i++) { // 작업들 하나씩 보면서 
            if (!completed[i]) { // 수행하지 않은 작업 있으면 
                completed[i] = true; // 검사 표시하고 
                jobQueue.add(new Job(jobs[i][0], jobs[i][1])); // 작업 큐에 넣어주기 
                processJobs(jobs);  // 작업 시작 
            }
        }
        return totalCompletionTime / jobs.length; // 평균 작업시간 = 총 작업시간 / 작업갯수
    }

    private void processJobs(int[][] jobs) {
        while (!jobQueue.isEmpty()) { // 수행할 작업 남아있으면 
            Job job = jobQueue.poll(); // 꺼내서 

            if (job.startTime > endTime) // 해당 작업 요청 시간이 이전 작업 종료시간보다 나중이면 
                endTime = job.startTime + job.duration; // 작업하고 이 작업 완료 시간은 요청시간 + 작업 소요시간
            else // 해당 작업 요청 시간이 이전 작업 종료시간보다 나중이면 
                endTime += job.duration; // 해당 작업 종료시간에 현재 작업 검사시간 추가 

            totalCompletionTime += endTime - job.startTime; // 총 작업 시간엔 검사 완료 시간 - 요청시간 누적

            for (int i = 0; i < jobs.length; i++) {  // 다른 작업들 검사하면서 
                if (completed[i]) continue; // 완료한 작업이면 pass 
                if (jobs[i][0] < endTime) { // 해당 작업이 현재 검사 작업종료시간보다 앞서면 
                    completed[i] = true; // 방문 처리
                    jobQueue.add(new Job(jobs[i][0], jobs[i][1]));  // 해당 작업 작업 큐에 넣어주기
                }
            }
        }
    }
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



