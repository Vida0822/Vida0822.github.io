---
published: true
title:  "# 에러 There isn’t anything to compare  해결 "
categories: coding
tag: [java, 코딩] 
toc: true
author_profile: false 
---
# 에러 There isn’t anything to compare  해결 

 

문제 : 내 github.io 블로그에 아무리 커밋을 해도 잔디심기(내 커밋기록에 반영되지 않음)

-> minimal.stake로 잡혀있는곳에서 full request?를 내 블로그로 해서 변경하고자 함 

-> 문제 발생 : full request 실패 

 ㄴ  에러 이름 : [Git] There isn’t anything to compare



필자는 STS에서 master로 push를 하였고 기본 브랜치가 master로 잡혀있어서 아직 main 브랜치에 미 적용 단계여서 위와 같은 문구가 나왔던 것이다.

[여담]

이전엔 default가 master였는데 마스터 슬레이브라는 용어가 흑인 문화에서 인종차별적 발언이 될 수 있다는 문제로 master에서 main으로 바뀌었다고 한다!

근데 eclipse나 sts 같은 곳에선 아직 default 이름을 master로 써서 기본값 이름이 다른거임 



=> 해결방법 : main branch 로 변경 ??



ㄴ 요거 정확한 원리 공부하는것도 도움이 될듯 



git bash 실행  : 아래와 같이 명령어 입력 

```git
git checkout master

git branch main master -f

git checkout main

git push origin main -f
```





* 실제 내 gitbash 화면창 

```git

SHIN HEEMIN@DESKTOP-DGFPD6H MINGW64 /d/Programming/githubio/Vida0822.github.io (master)
$ git branch main master -f

SHIN HEEMIN@DESKTOP-DGFPD6H MINGW64 /d/Programming/githubio/Vida0822.github.io (master)
$ git checkout main
Switched to branch 'main'

SHIN HEEMIN@DESKTOP-DGFPD6H MINGW64 /d/Programming/githubio/Vida0822.github.io (main)
$ git push origin main -f
Total 0 (delta 0), reused 0 (delta 0), pack-reused 0
remote:
remote: Create a pull request for 'main' on GitHub by visiting:
remote:      https://github.com/Vida0822/Vida0822.github.io/pull/new/main
remote:
To https://github.com/Vida0822/Vida0822.github.io.git
 * [new branch]        main -> main


```





full request 재시도 

![image-20230702190228251](D:\Programming\githubio\Vida0822.github.io\images\2023-07-02-mastermainbranch\image-20230702190228251.png)

또 ! 문제 발생 

: 애초에 내가 작업하던 브랜치가 default branch (master던, main이던)이라 그걸 똑같은 master, main branch에 pull request 할 수 없음 

-> 다른 branch에서 작업 후 매번 main branch (default) 에 pull request 해줘야 잔디 심기가 가능

![image-20230702190847587](D:\Programming\githubio\Vida0822.github.io\images\2023-07-02-mastermainbranch\image-20230702190847587.png)





근데! 그냥 브랜치만 만들어주면 

아직 해당 브랜치와 origin branch가 분리가 안되서 

github branch == master branch 오류 발생 

-> 새로 만든 branch에서 어떤 작업을 한 후 새롭게 커밋시켜 브랜치 구분해줌 



이제야 뜸 

![image-20230702190739556](D:\Programming\githubio\Vida0822.github.io\images\2023-07-02-mastermainbranch\image-20230702190739556.png)





pull request 성공 후 merge 까지 진행 

![image-20230702190821786](D:\Programming\githubio\Vida0822.github.io\images\2023-07-02-mastermainbranch\image-20230702190821786.png)





merge 성공 

![image-20230702190925657](D:\Programming\githubio\Vida0822.github.io\images\2023-07-02-mastermainbranch\image-20230702190925657.png)



근데 안됨 실화냐...?

해결책을 찾다보니 fork 된 프로젝트는 

한참을 더 끙끙거리며 검색해보니 fork된 프로젝트의 경우 잔디가 안심어진다는 해결방안을 찾게 되었다.

사전지식없이 깃허브 블로그를 처음 만들어보는 입장이라면, 마음에 드는 Jekyll테마를 fork한 뒤 Repository name을 바꿔서 제작하신 분들이 많을 거라 생각한다.

나의 경우, 기존의Github desktop을 이용해 새로운 *name.github.io* 폴더를 만든 후, 기존 블로그 파일을 옮겨 넣는 식으로 문제를 해결하였다.



![image-20230702191512344](D:\Programming\githubio\Vida0822.github.io\images\2023-07-02-mastermainbranch\image-20230702191512344.png)

fork 되어있는거 확인...



일단 포기... 이거 하려면 원격 저장소 지우고 다시 만들어서 거기에 로컬 저장소 내용 한번에 push해야하는데 

너무 무섭다 .... 

![image-20230702192442609](D:\Programming\githubio\Vida0822.github.io\images\2023-07-02-mastermainbranch\image-20230702192442609.png)

도전...



개망 블로그 삭제됨 ㅋㅋㅋㅋ github io 때려칩니다 ...

내 1주일 어디감 