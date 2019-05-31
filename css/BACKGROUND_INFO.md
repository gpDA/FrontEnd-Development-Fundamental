### Good UX Design
    - UX focuses on human-computer interaction
    


“Resetting” 과 “Normalizing” CSS의 차이점
resetting
Resetting은 요소의 모든 기본 브라우저 스타일을 제거하기 위한 것
Normalizing
normalizing은 모든 것은 “정리"하는 것이 아니라 유용한 기본 스타일을 보존


float이 어떻게 작동하는지 설명
float은 css 위치 지정 속성
Float된 요소는 페이지의 흐름의 일부로 남아 있으면 …
CSS clear 속성은 left / right / both float 엘리멘트 아래에 위치하도록 사용될 수 있다
.clearfix 핵은 영리한 CSS 의사 선택자 (:after)를 사용하여 실수를 제거
.clearfix:after {
  content: ' ';
  visibility: hidden;
  display: block;
  height: 0;
  clear: both;
}


inline block 과  block의 차이점


em 과 rem 의 차이점