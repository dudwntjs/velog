<p>iOS 프로젝트를 하다 보면 이런 값들이 필요합니다.</p>
<ul>
<li>BASE_URL</li>
<li>API_KEY</li>
<li>OAuth Secret</li>
<li>Firebase Key</li>
<li>Third-party SDK Key</li>
</ul>
<p>이 값들은 앱 동작에 꼭 필요하지만, 코드에 직접 <strong>하드코딩하는 방식은 좋은 방법이 아닙니다</strong>.
환경이 바뀔 때마다 코드를 수정해야 하고, 실수로 민감 정보가 노출될 위험도 있기 때문입니다🚨🚨</p>
<h4 id="왜-config-파일을-써야-할까요">왜 Config 파일을 써야 할까요?</h4>
<blockquote>
<ul>
<li>민감 정보 보호</li>
</ul>
</blockquote>
<ul>
<li>개발 / 스테이징 / 운영 서버 분리</li>
<li>빌드 환경별 설정 관리</li>
<li>팀원 간 충돌 방지</li>
</ul>
<p>실무에서는 거의 필수로 사용하는 방식입니다.</p>
<br />

<hr />
<h2 id="configuration-settings-file-만들기"><strong>Configuration Settings File 만들기</strong></h2>
<p>Xcode 프로젝트 네비게이터에서 우클릭하고, <code>New File from Template</code> 클릭합니다.</p>
<img src="https://velog.velcdn.com/images/dudwntjs/post/abf0d678-972f-4c82-aadb-849afa89ba61/image.png" width="500" />


<p><code>Configuration Settings File</code> 검색해서 <code>.xcconfig</code> 파일 생성합니다.</p>
<img src="https://velog.velcdn.com/images/dudwntjs/post/64c599a9-59c2-467f-86fd-dbdb3acdfaef/image.png" width="500" />

<br />

<hr />
<h2 id="project-→-info에서-연결하기">PROJECT → Info에서 연결하기</h2>
<p><code>PROJECT</code> → <code>Info</code>  들어가서 <code>Configurations</code> 영역 확인합니다.
<code>No Configuration File</code> 클릭하고, <code>Based on Configuration File</code>로 변경합니다.</p>
<p>방금 만든 <code>.xcconfig</code> 선택합니다.</p>
<img src="https://velog.velcdn.com/images/dudwntjs/post/c4879cab-2985-4175-b2fc-a22b51d1217d/image.png" width="600" />

<p>여기서<strong>Debug와 Release를 각각 다른 xcconfig 파일로 연결해두면</strong>, 빌드 타입에 따라 자동으로 서버가 분리됩니다✨</p>
<p>이거 설정안해두면 <strong>Config 파일 적용이 안됩니다</strong>⚠️</p>
<br />

<hr />
<h2 id="xcconfig-파일에-값-작성하기">xcconfig 파일에 값 작성하기</h2>
<img src="https://velog.velcdn.com/images/dudwntjs/post/f34ab3a0-899d-41f0-b5ef-a8dc10894cf0/image.png" width="600" />

<p>Xcode는 <code>//</code>를 주석으로 인식합니다.</p>
<p>그래서 이렇게 쓰면:</p>
<pre><code>BASE_URL = http://your-domain.com</code></pre><p><code>//</code> 뒤가 주석 처리되어 제대로 읽히지 않을 수 있습니다😵</p>
<p>그래서 안전하게:</p>
<pre><code>BASE_URL = http:/$()/your-domain.com</code></pre><p>이렇게 작성해줍니다‼️</p>
<br />

<hr />
<h2 id="swift에서-environment로-관리하기">Swift에서 Environment로 관리하기</h2>
<p>이제 Swift에서 이렇게 사용합니다.</p>
<pre><code class="language-swift">import Foundation

enum Environment {
  static let baseURL: String = Bundle.main.infoDictionary?[&quot;BASE_URL&quot;] as! String
}</code></pre>
<h4 id="강제-언래핑-써도-될까">강제 언래핑 써도 될까?</h4>
<p>BASE_URL이 필수 값이라서 없으면 앱이 터져도 된다고 생각했습니다!
그래도 조금 위험하니까 <code>as!</code> 대신 <code>fatalError</code>를 써도 좋을 것 같네요🤔</p>
<br />

<h3 id="🚫-github에는-올리면-안-됩니다">🚫 GitHub에는 올리면 안 됩니다</h3>
<p>Config 파일에는 API Key나 Base URL 같은 <strong>민감 정보</strong>가 들어가기 때문에
깃허브에 그대로 업로드하면 보안 문제가 발생할 수 있습니다.</p>
<p>그래서 보통 <strong>.gitignore에 추가해서 GitHub에 올라가지 않도록 관리</strong>합니다‼️</p>
<p><code>.gitignore</code> 파일에 아래 내용을 추가해 주세요:</p>
<pre><code>*.xcconfig</code></pre><p>이렇게 하면 모든 <code>.xcconfig</code> 파일이 Git 추적 대상에서 제외됩니다.</p>
<br />

<blockquote>
<p>참고한 공식 문서입니다.
<a href="https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project">Adding a build configuration file to your project | Apple Developer Documentation</a></p>
</blockquote>
<p><span style="color: gray;">처음엔 조금 낯설 수 있지만, 한 번 설정해두면 정말 편합니다.
이제 서버 주소를 코드에 하드코딩하지 말고,
깔끔하게 관리해보세요😊</span></p>