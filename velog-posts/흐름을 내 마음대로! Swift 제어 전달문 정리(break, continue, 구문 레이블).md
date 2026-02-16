<p>프로그램은 기본적으로 <strong>위 → 아래로 순차 실행</strong>됩니다.
하지만 반복문이나 조건문을 실행하다 보면,<strong>여기서 그만 멈추고 싶을 때</strong>나 <strong>이번 차례만 건너뛰고 싶을 때</strong>가 생기자나요🥺</p>
<p>이때 실행 순서를 변경해 주는 도구가 바로 <strong>제어 전달문(Control Transfer Statements)</strong>입니다.</p>
<br />

<h2 id="1-break-이제-그만-당장-나가라"><strong>1. break: 이제 그만! 당장 나가라!</strong></h2>
<p><code>break</code>는 <strong>현재 실행 중인 반복문 또는 switch를 즉시 종료</strong>합니다.</p>
<img src="https://velog.velcdn.com/images/dudwntjs/post/7389a51c-9d2d-494d-8909-a1b5cc8104f5/image.png" width="500" />


<p>반복 횟수가 남았더라도 <code>break</code>를 만나는 순간 반복문을 빠져나와 다음 코드로 이동합니다.</p>
<h3 id="switch에서의-break"><code>switch</code>에서의 break</h3>
<p>Swift의 <code>switch</code>는 <strong>자동으로 break 되는 구조</strong>입니다.
(다른 언어처럼 break를 반드시 쓰지 않아도 됨)</p>
<pre><code class="language-swift">switch number {
case 1:
    print(&quot;One&quot;)
case 2:
    print(&quot;Two&quot;)
default:
    print(&quot;Other&quot;)
}</code></pre>
<ul>
<li>각 case 실행 후 자동으로 switch 종료</li>
</ul>
<pre><code class="language-swift">switch number {
case 1:
    print(&quot;One&quot;)
    fallthrough
case 2:
    print(&quot;Two&quot;)
default:
    print(&quot;Other&quot;)
}</code></pre>
<p>Swift에서는 기본적으로 다음 case로 넘어가지 않습니다.
다음 case까지 실행하고 싶다면 <strong>명시적으로 <code>fallthrough</code>를 작성</strong>해야 합니다.</p>
<br />

<hr />
<h2 id="2-continue-이번-판은-패스-다음-차례로"><strong>2. continue: 이번 판은 패스! 다음 차례로!</strong></h2>
<p><code>continue</code>는 <code>break</code>처럼 실행을 멈추지만, 아예 끝내는 게 아니라 <strong>&quot;현재 반복만 중단하고 다음 반복으로 건너뛰는&quot;</strong> 역할을 합니다.</p>
<img src="https://velog.velcdn.com/images/dudwntjs/post/2a7b6218-9396-45a3-9ba7-e68681fe5640/image.png" width="500" />

<p>🚨 <code>break</code>는 전체 반복 종료, <code>continue</code>는 이번 차례만 건너뛰고 다시 루프 조건 평가합니다</p>
<h3 id="실전-응용-문자열-필터링"><strong>실전 응용: 문자열 필터링</strong></h3>
<p><code>continue</code>를 응용하면 특정 문자만 변환하거나 필터링하는 기능을 쉽게 만들 수 있습니다🤗</p>
<img src="https://velog.velcdn.com/images/dudwntjs/post/e5145fce-25ba-4c6e-bc81-02bacad05878/image.png" width="500" />

<ul>
<li><code>continue</code>는 조건이 많아질수록 코드 가독성을 크게 개선</li>
<li>예외 케이스 먼저 처리하고 넘기길 때 자주 사용❗️</li>
</ul>
<br />

<hr />
<h2 id="3-구문-레이블statement-labels-타겟을-정확히-지정"><strong>3. 구문 레이블(Statement Labels): 타겟을 정확히 지정!</strong></h2>
<p>반복문이 여러 번 중첩되어 있을 때 <code>break</code>를 쓰면, <strong>가장 인접한(안쪽) 반복문만 종료</strong>됩니다.
만약 안쪽 루프에서 바깥쪽 루프까지 한꺼번에 종료하고 싶다면 어떻게 해야 할까요?</p>
<p>이때 사용하는 것이 바로 <strong>구문 레이블</strong>입니다🌟</p>
<h3 id="형식"><strong>형식</strong></h3>
<pre><code class="language-swift">레이블이름 : while 조건식 {
    break 레이블이름
}</code></pre>
<h3 id="비교해보기-레이블이-없을-때-vs-있을-때"><strong>비교해보기: 레이블이 없을 때 vs 있을 때</strong></h3>
<p><strong>❌ 레이블 없이 플래그 변수를 쓰는 복잡한 방식:</strong></p>
<pre><code class="language-swift">var loopFlag = true
for i in 1...5 {
    for j in 1...9 {
        if j == 3 {
            loopFlag = false
            break // 안쪽만 종료
        }
    }
    if loopFlag == false { break } // 바깥쪽도 종료시키기 위한 추가 코드 필요
}</code></pre>
<ul>
<li>불필요한 변수 생성</li>
<li>조건 체크가 두 번 들어감</li>
<li>가독성이 떨어짐</li>
</ul>
<p><strong>⭕️ 구문 레이블을 사용한 깔끔한 방식:</strong></p>
<img src="https://velog.velcdn.com/images/dudwntjs/post/78572e2f-b461-4aba-bd34-5fc9aef0a1a7/image.png" width="500" />

<ul>
<li><code>j == 3</code>이 되는 순간</li>
<li><code>break outer</code> 실행</li>
<li><code>outer</code>가 붙은 바깥 루프 전체 종료</li>
<li>반복문 완전히 끝남🤩</li>
</ul>
<h3 id="continue도-레이블과-함께-사용-가능">continue도 레이블과 함께 사용 가능!</h3>
<img src="https://velog.velcdn.com/images/dudwntjs/post/ffc120ff-a847-4804-ae7b-6fe37f7996aa/image.png" width="500" />

<ul>
<li>j가 1, 2일 때는 출력</li>
<li>j가 3이 되면<ul>
<li>안쪽 루프를 끝까지 도는 게 아니라</li>
<li><strong>바로 다음 i 값으로 넘어감</strong></li>
</ul>
</li>
</ul>
<br />

<hr />
<h2 id="📝-제어-전달문-최종-정리">📝 제어 전달문 최종 정리</h2>
<blockquote>
<ul>
<li><strong><code>break</code></strong>: 전체 반복을 즉시 종료하고 싶을 때</li>
</ul>
</blockquote>
<ul>
<li><strong><code>continue</code></strong>: 현재 반복만 건너뛰고 다음 반복을 계속하고 싶을 때</li>
<li><strong><code>구문 레이블</code></strong>: 중첩된 루프에서 원하는 위치의 반복문을 제어하고 싶을 때</li>
</ul>
<br />

<p><span style="color: gray;">사실 Swift에서는 <code>first(where:)</code>, <code>filter</code>, <code>map</code>, <code>contains</code>
같은 고차 함수가 많기 때문에 <code>break</code>와 <code>continue</code>를 직접 쓰지 않고 해결할 수 있는 경우도 많습니다.
그래서 중요한 건 단순히 문법을 아는 것이 아니라, <strong>어떤 방법이 가장 읽기 좋은 코드인지 고민하는 것</strong> 인 것 같아요ㅎㅅㅎ</span></p>
<p>제어 전달문을 적절히 활용해 복잡한 로직을 단순하게 정리하는 연습, 그리고 더 나은 실행 흐름을 설계하는 연습을 계속해봐야겠습니다🔥</p>