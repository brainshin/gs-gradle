node {
   // ⚠️ [중요] 젠킨스 Credentials ID에 등록된 JFrog 계정 정보의 ID를 매핑합니다.
   // 만약 기존에 만들어두신 ID 이름이 'jfrog-login'이 아니라면 실제 등록된 이름으로 변경하세요.
   withCredentials([usernamePassword(credentialsId: 'jfrog-login', usernameVariable: 'JFROG_USER', passwordVariable: 'JFROG_PASS')]) {
       
       stage('Build & Checkout') {
           // 소스 코드를 원격 저장소에서 다운로드합니다.
           git url: 'github.com'
       }

       stage('Exec Gradle & Deploy') {
           // 아티팩토리 하위 폴더 내부로 작업 디렉토리를 변경합니다.
           dir('artifactory') {
               // 1. 젠킨스 내장 툴 설정을 무시하고, 프로젝트 자체 래퍼를 Java 21 규격(Gradle 8.14.5)으로 강제 이식합니다.
               sh "gradle wrapper --gradle-version 8.14.5"
               sh "chmod +x gradlew"
               
               // 2. 오류를 유발하던 플러그인 객체 대신, 순수 쉘 명령어로 배포 태스크를 직접 트리거합니다.
               // 젠킨스 자격 증명 변수를 자바 인자(-P)로 강제 주입하여 인증을 통과시킵니다.
               sh "./gradlew clean artifactoryDeploy -Partifactory_user=${JFROG_USER} -Partifactory_password=${JFROG_PASS}"
           }
       }
   }
}
