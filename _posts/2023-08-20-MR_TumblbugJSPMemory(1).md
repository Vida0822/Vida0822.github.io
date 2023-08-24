---
published: true
title:  "그럼 연필을 쓰면 되잖아요?_ jsp 프로젝트 회고(1)"
categories: Memory
tag: [java, Project] 
toc: true
author_profile: false 
---



프로젝트 중 가장 크고 긴 세번째 프로젝트가 시작되었다. 팀 내 '테크 리더'를 맡은만큼 그 책임감과 부담이 굉장히 컸던것 같다.  <br>



### 요구분석 : 신중하고 꼼꼼하게

다양한 기능이 존재하지만 우리수준에서 구현가능한 사이트를 고르는건 어려웠고, 웹 프로젝트를 진행해본 사람이 없어 타당한 선택인지 판단하기 곤란했다. 다같이 사이트를 하나씩 뒤져가는 무분별한 주제 던지기식으론 제대로 된 선택을 할 수 없었고 정리가 되지 않았다. 따라서 강사님께 문의한 내용을 바탕으로 나와 팀장오빠는 주제를 정하는 '기준'을 명확히 정했고, 기준에 맞춰 조사해온 사이트를 살펴보며 회의를 진행하니 범위가 좁아져 판단하기 수월했다. 최종적으로 클라우딩 펀딩 사이트 '텀블벅'을 선정했는데, 겸사겸사 요구분석도 되었고, 어차피 수업때문에 다른팀도 우리팀도 이후 프로젝트를 따로 진행하긴 어려웠기에 꼼꼼하게 살피며 재미있고 독특한 사이트를 고른것 같아 뿌듯했다. 이후 반에서 가장 잘하는 현직자 오빠한테 물어가며 웹 개발 프로세스를 파악했고, 우리의 시간적, 기술적 한계를 고려해 생략해야할건 생략하고 필수 단계만 뽑아 개발일정을 수립한 후 팀원 및 강사님께 피드백을 받았다.   

전원이 사이트 전체를 꼼꼼히 보아 요구분석을 탄탄하게 할 수 있도록 ''각자 요구분석 명세서 채워오기'일정을 1주일로 넉넉하게 일정을 잡았다. 다들 한번씩 모델링을 해보았기에 어떤 방식으로, 어떤 관점에서 요구분석을 해야할지 저번보다 감을 잡고 있는 상태였다.  나는 큰 업무 프로세스를 파악하여 수많은 기능들의 '액터'를 먼저 도출한 후, 각 액터가 중심 객체인 '프로젝트'를 어떤 식으로 다루는지 염두하여 분석했다. 세세한 디테일은 내가 잘 이해하고 있는지 확인하는정도까지만 적고 꼭 깔끔하게 공들여 작성할 필요는 없었을 것 같다.





### DB 설계 : 그럼 연필을 쓰면 되잖아요?

이전에는 임의로 기능을 생략할 수 있었지만, 이번에는 사이트의 화면을 똑같이 구현해야 했기에 기능구현 다음으로 가장 공을 들인 단계다. 각자 작성해온 요구분석본을 바탕으로 개념적 모델링을 진행했는데, 역시 erd를 완료하지 못한 팀원들이 많았다. 작업시간이 부족하기도 했지만 "erd를 그려오세요" 라고 하니 모두 draw.io로 냅다 작업을 했고 불편한 툴로 설계와 작업을 동시에 하느냐 시간이 오래걸렸다고 한다. 초기 설계는 다 뒤집어질수 밖에 없기에 다루기 어려운 툴로 공들여 만드는건 오히려 제작, 수정만 더 어렵게 만든다. 그렇기에 나는 아이패드에 손으로 직접 그려가면서 빠르게 제작했고 덕분에 논의과정 중 수정이 용이했다. '구글독스'를 사용하며 느꼈듯이, 능력 밖의 고급 기능을 억지로 사용하는 건 좋지않다. 영화 '세얼간이'에서 총장이 우주에서 사용할 수 있는 펜을 최고의 발명품이라고 보여주지만, 란초의 그럼 연필을 쓰면 되잖아요? 라는 질문하고 총장은 말을 잃는다. 수단이 목적을 이겨선 안된다. 기술을 사용하는 목적과 이유가 뭔지 정확히 점검한 후, 사용할 수 있는 방법 중 가장 효율적인 방법을 택하는게 맞다. 너무 복잡하게, 어렵게 문제에 접근하는 건 개발자들이 범할 수 있는 흔한 문제인것 같다. 기술한테 '이용'당하는게 아닌 '이용'하는, 도구보다는 '본질'에 집중하는 진취적이고 주도적인 자세가 중요하다. 

부실한 상태라도 누구든 자료를 보여주고 피드백을 주고받으며 진행하는 것이 중요하기에 내 작업물을 설명하면서 회의를 이끌어나갔는데, 처음부터 다같이 작업하는것보다 대략적이라도 완성한 내용을 기반으로 진행하다 보니 빠르게 논의할 수 있었고, 구조 파악이 어려웠던 팀원들도 다시한번 자신의 이해를 점검할 수 있었다. 칼럼까지 정의하는 물리적 모델링을 병행했는데, db 설계가 처음일 땐 이 방식이 오히려 시간을 지체시켰지만 이번엔 어느정도 익숙해진 상태라 어렵지 않았다. 개체를 정리하다보니 작성한 ERD 구조를 수정해야 할 부분이 많았지만, 처음부터 줄글로 정의하기보다는 작성한 ERD를 기반으로 하나씩 점검하며 수정했던 것이 효율적이었던 것 같다. 관계가 어려운 부분은 직접 테이블을 그리고 데이터를 구상해보며 잘못된 부분을 빠르게 파악할 수 있었고, 팀장과 테크 리더만이 질문 가능한 상황이었기 때문에 의문이 생기거나 의견 차이가 있는 부분은 시간을 소모하지 않고 질문을 정리한 후 팀원들에게 공유했다.





### 화면설계 및 기능정의 : 내 코딩을 봐줘 !  

다시 한 번 긴 연휴가 시작되었는데 각자의 상황을 일일히 확인하는 것도 쉽지 않고, 할당된 작업이 명확히 안 정해진채로 개별적으로 작업하면 시간만 허비할 수 있었다. 따라서 구현할 기능 목록을 각자 도출하고, 이틀 동안 장소를 대관하여 오프라인 회의를 진행하기로했다. 화면 설계서를 활용하려 했으나 어설펐고, 결국 화면을 하나하나 타고가며 기능을 정리했다. 이 과정을 통해 특정 기능을 개발해 놓고 이를 필요한 곳에서 요청하여 활용하는 매커니즘을 깨달았고, 단순히 따라 따라쳤던 요청 URL과 MVC 구조, 한 테이블의 쿼리 작업을 묶어놓은 DAO의 존재 이유를 이해할 수 있었고 책을 읽고 코딩하는 방식보다 실제 제작에 사용해보면 기술을 더 잘 이해되고 빠르게 적응할 수 있다.

기능 목록을 먼저 완벽히 작성하려 했으나 구조 및 기능 파악이 원활이 되지않아 더이상 틀을 짜고 설계하는건 큰 의미가 없다고  판단했고, 함께 몇 가지 기능을 함께 개발하면서 개발 방식을 통일하고 전원이 이해할 수 있게끔 돕고자했다. 내 메모를 바탕으로 큰 문제 없이 프로젝트를 세팅한 후 DB 계정을 만들어 해당 IP 주소로 연결했고, 회원가입 및 로그인 기능 코딩을 시작했다. 그러나 진이 다 빠진 상태라 아무런 생각도 나지 않았고, 인혁오빠가 도착해 진행하며 구조만 간신히 파악했다. 집에 가면서 예상한 대로 팀원들이 MVC 구조와 설계한 DB에 대한 이해가 완벽하지 않으며 소통에 문제가 있다는 것을 실감하여 마음이 불편했다. 다음날 내가 코딩을 하면 팀원들은 화면을 보며 의견을 냈다. 다른 팀원들은 자신의 작업과정을 보여주기가 부끄러워하거나 창피해하는데, 나는 이런 협업 방식이 잘 맞았다. 코드 공유에 부담스러움을 느끼지 않으며, 내 코드에 대해 피드백을 받는 방식이 매우 재미있었고, 검토받는 과정에서 실력적으로도 큰 도움이 되었다.







### 기능 구현 : 예상 밖의 변수 

역할분담에 두 가지 의견을 제시했다. 첫 번째는 기능별로 역할을 나누는 방식인데, 나 혼자 작성한 목록이었기 때문에 애매한 부분이나 오류가 존재했다. 다른 방안은 액터를 기준으로 화면을 따라가며 개발하는 것인데, 같은 기능이 반복되는 경우가 많아 중복 작업을 할 수도 있고, 화면만 보고 기능을 파악하기 어렵다는 문제가 있었다. 그래서 화면기준 개발하되, 각자 개발 중인 기능을 내 기능 목록에 표시하고 공유 요청 URL 문서 작성을 제안했다. 중복된 기능이 있는지 목록에서 확인할 수 있어 상당히 효율적이었다. 이렇게 우리는 회원 액터 4명과 창작자 액터 3명으로 역할을 분담하기로 결정했다.

프로젝트 수행 과정에서 몇 가지 문제가 있었다. 첫 번째로, 액터에 대한 개념이 부족해 초기에 '후원자' 액터를 발견하지 못했다. 그 결과, 프로젝트 목록보기, 상세보기, 후원하기가 사이트의 핵심 기능이었음에도 이를 후순위로 미뤘다. 모든 기능을 구현하는건 애초에 불가능했기에 상황, 시간, 기술적 제약을 더 잘 고려한 뒤 중요도에 따라 개발을 시작했다면, 후반의 시간부족 문제를 피할 수 있었을 것이다. 두 번째는 창작자 액터 인원 중 나를 제외한 두 명이 MVC 구조에 대한 이해가 부족했다. 나 혼자 개발하면서 설명도 동시에 해줘야 했고, 이로 인해 작업 속도가 느려지고 비효율적이었다. 세 번째 문제는 프론트엔드 개발이다. 나중에 알게 된 사실이지만, 텀블벅 사이트는 프론트 코드를 공개하지 않고 자바 스크립트로 구현되어 있었고, 페이지 스타일링도 복잡했다. 다른 팀들처럼 이미 구현된 뷰 페이지를 활용하거나 프론트 개발자의 코드를 가져와 CSS 스타일을 맞추는 것이 아니라, 각 페이지를 일일이 구현하거나 스크립트로 구현된 코드를 수정해야 했다. 이 문제에 대해 강사님과 여러 번 회의하며 사이트를 변경할지 고려했으나 시간이 한정적이었기에 요구분석과 DB 설계를 다시 하기는 어려웠다. 따라서 우리는 주요 기능 중 메인 페이지의 주요 프로세스만 개발하는 대신 프론트 기능까지 완벽하게 구현하기로 의견을 모았다. 나는 작업 중이던 프로젝트 올리기를 백엔드 개발만 마치고 메인 페이지의 프로젝트 목록보기 및 상세보기 작업에 착수했고, 인혁오빠는 프론트 페이지 및 메인 페이지를 완성시키기로 했다. 이렇게 남은 시간이 한 주밖에 없는 상황에서 메인 프로세스의 기능 개발을 진행했다. 

시간이 너무 부족해 합숙을 하며 간신히 기능 개발을 완료했다. 그럼에도 불구하고 데이터 정리와 프로젝트 마무리 과정이 제대로 이루어지지 않아 기능 간의 연결이 원활하지 않은 부분이 있었다. 메인 프로세스 시연은 가까스로 성공적으로 마무리할 수 있었지만, 발표 역시 다른 팀에 비해 퀄리티가 떨어지는 부분이 있어 아쉬움이 남았다. 








