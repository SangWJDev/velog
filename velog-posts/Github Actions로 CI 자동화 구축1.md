<p>팀 프로젝트를 진행하며 GitHub Actions를 활용해 CI 자동화를 구축하는 과정에서 생각보다 많은 시행착오를 겪었다.
이 글은 그 과정에서 겪었던 트러블슈팅과 설정 이슈들을 정리한 기록이다.
앞으로 나 자신은 물론, GitHub Actions를 처음 접하는 다른 팀원들도 같은 문제로 헤매지 않기를 바라는 마음에서 정리하게 되었고,
이 과정을 명확히 이해하고 넘어가는 것이 나중에 배포 환경까지 이어지는 데에도 훨씬 수월할 것이라 생각한다.</p>
<h2 id="1-env-값이-github-actions에서-어떻게-주입되는가">1. .env 값이 GitHub Actions에서 어떻게 주입되는가?</h2>
<h3 id="📁-env-파일-예시">📁 .env 파일 예시</h3>
<pre><code class="language-env">DB_DRIVER=com.mysql.cj.jdbc.Driver
DB_HOST=localhost
DB_PORT=3306
DB_NAME=test_db
DB_USERNAME=test_user
DB_PASSWORD=test_password</code></pre>
<p>이런 값들은 일반적으로 application.yml이나 docker-compose.yml에서 ${} 형태로 사용된다.</p>
<hr />
<h3 id="🧩-github-actions에서의-환경변수-주입-방식">🧩 GitHub Actions에서의 환경변수 주입 방식</h3>
<pre><code class="language-yml"># workflows.yml
- name: Build and Test
  env:
    DB_DRIVER: ${{ secrets.DB_DRIVER }}
    DB_HOST: ${{ secrets.DB_HOST }}
    DB_PORT: ${{ secrets.DB_PORT }}
    DB_NAME: ${{ secrets.DB_NAME }}
    DB_USERNAME: ${{ secrets.DB_USERNAME }}
    DB_PASSWORD: ${{ secrets.DB_PASSWORD }}
  run: ./gradlew clean build</code></pre>
<p>👉 GitHub Actions에서는 .env 파일을 직접 불러오지 않고,
GitHub Settings &gt; Secrets에 등록된 값들을 ${{ secrets.XXX }} 형태로 불러와 env:에 직접 주입한다.</p>
<hr />
<p>🔄 반면 docker-compose.yml이나 application.yml에서는 .env를 자동으로 읽거나 ${}를 통해 참조한다.</p>
<hr />
<h2 id="2-servicesmysql은-github-actions-안에서-어떻게-동작하는가">2. services.mysql은 GitHub Actions 안에서 어떻게 동작하는가?</h2>
<p><img alt="" src="https://velog.velcdn.com/images/tkdalsrb123/post/453bcf5a-fa53-4266-99a3-d8ddf0d24f9a/image.png" /></p>
<pre><code class="language-yml"># workflows.yml
services:
  mysql:
    image: mysql:latest
    env:
      MYSQL_DATABASE: ${{ secrets.DB_NAME }}
      MYSQL_ROOT_PASSWORD: root
      MYSQL_USER: test_user
      MYSQL_PASSWORD: ${{ secrets.DB_PASSWORD }}
    ports:
      - 3306:3306</code></pre>
<p>🔍 설명:
    •    GitHub Actions가 띄우는 임시 VM 안에서 MySQL 컨테이너를 구동한다.
    •    이 컨테이너는 CI 과정 동안만 살아 있고, 워크플로우 종료 시 자동으로 종료된다.
    •    위 설정은 MySQL 공식 Docker 이미지에서 인식 가능한 환경변수 이름을 따라야 하며,
문법은 MySQL Docker 이미지 문서 기준으로 작성해야 한다.</p>
<hr />
<h2 id="3-mysql-설정-문법을-따라야-하는-이유">3. MySQL 설정 문법을 따라야 하는 이유</h2>
<p>MySQL 공식 이미지가 docker-entrypoint.sh에서 인식하는 환경변수는 다음과 같다.</p>
<p>환경 변수 설명
MYSQL_ROOT_PASSWORD    루트 계정 비밀번호 (필수)
MYSQL_DATABASE    초기 생성할 DB 이름
MYSQL_USER    초기 생성할 일반 사용자
MYSQL_PASSWORD    해당 유저의 비밀번호</p>
<p>❗ 여기에 존재하지 않는 변수명(ex. DB_HOST)을 넣어도 MySQL 컨테이너는 무시하거나 에러를 낼 수 있음</p>
<hr />
<h2 id="4-applicationyml과-docker-composeyml-workflowyml의-차이점-정리">4. application.yml과 docker-compose.yml, workflow.yml의 차이점 정리</h2>
<p>항목    위치    주요 역할    .env 사용 방식
application.yml    Spring Boot 내부 설정    애플리케이션 실행 설정 (DB, JPA 등)    ${}로 참조 (Spring이 해석)
docker-compose.yml    로컬 개발 환경 구성    DB 등 서비스 컨테이너 정의    env_file: 또는 ${} 사용 가능
workflow.yml    GitHub Actions 워크플로우    CI 자동화 프로세스 정의    env:로 명시적으로 선언
.env    루트 또는 도커 내부    변수 통합 저장    Spring과 Compose에서 자동 인식 or GitHub Secrets로 분리 등록</p>
<hr />
<h3 id="✅-정리-요약">✅ 정리 요약</h3>
<p>📌 GitHub Actions에서 .env처럼 사용하려면?
    •    GitHub Secrets에 환경변수 등록 → env: 블록으로 직접 주입</p>
<p>📌 services.mysql이 하는 역할은?
    •    테스트용 MySQL 컨테이너를 GitHub VM 내부에서 임시 실행 (DB 따로 설치 X)</p>
<p>📌 MySQL 컨테이너 설정에서 지켜야 할 문법은?
    •    MYSQL_ROOT_PASSWORD, MYSQL_USER, MYSQL_PASSWORD, MYSQL_DATABASE 등의 공식 Docker 환경변수만 사용해야 함</p>
<p>📌 application.yml이나 docker-compose.yml은 어떻게 다름?
    •    Spring Boot 실행용 설정 vs 개발환경 전체를 구성하는 설정 (Compose)
    •    .env는 둘 다 참조 가능하지만 GitHub Actions는 직접 주입해야 함</p>
<h3 id="참고자료">참고자료</h3>
<p>[github docs] (<a href="https://docs.github.com/en/enterprise-cloud@latest/admin/configuring-settings/configuring-private-networking-for-hosted-compute-products/about-azure-private-networking-for-github-hosted-runners-in-your-enterprise">https://docs.github.com/en/enterprise-cloud@latest/admin/configuring-settings/configuring-private-networking-for-hosted-compute-products/about-azure-private-networking-for-github-hosted-runners-in-your-enterprise</a>)</p>