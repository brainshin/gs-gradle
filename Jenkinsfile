node {
   // 젠킨스에 등록된 JFrog 인스턴스 ID 매핑
   def server = Artifactory.server('trialsfhhhn.jfrog.io')
   def rtGradle = Artifactory.newGradleBuild()
   def buildInfo = Artifactory.newBuildInfo()

   stage('Build') {
       // 최신 마스터 브랜치 코드를 완전히 새로 긁어옵니다.
       // git url: 'github.com'
      git url: 'https://github.com/brainshin-star/gs-gradle.git'
   }

   stage('Artifactory configuration') {
       rtGradle.tool = 'brainshin-gradle' // Gradle 8.14.5 가동
       
       // ⚠️ 배포(deployer) 창고만 지정하고, 라이브러리를 다운로드받는 리졸버(resolver) 단계는 완전히 지웁니다.
       rtGradle.deployer repo: 'brainshin-gradle-dev-local', server: server
   }

   stage('Config Build Info') {
       buildInfo.env.capture = true
       buildInfo.env.filter.addInclude("*")
   }

   stage('Extra gradle configurations') {
       // 소스 내부의 최신 com.jfrog.artifactory 플러그인 사양을 바인딩하기 위해 true 유지
       rtGradle.usesPlugin = true 
   }

   stage('Exec Gradle') {
       // ⚠️ [가장 중요] Deprecated 유발 인자인 buildFile 파라미터를 소멸시키고, 
       // 최신 규격 배포 명령어인 'artifactoryDeploy'로 강제 수정 주입합니다.
       rtGradle.run rootDir: "artifactory/", tasks: 'clean artifactoryDeploy', buildInfo: buildInfo
   }

   stage('Publish build info') {
       server.publishBuildInfo buildInfo
   }
}
