node {
   // 정식 JFrog Server ID 매핑
   def server = Artifactory.server('trialsfhhhn.jfrog.io')
   def rtGradle = Artifactory.newGradleBuild()
   def buildInfo = Artifactory.newBuildInfo()

   stage('Build') {
       git url: 'https://github.com/brainshin-star/gs-gradle.git'
   }

   stage('Artifactory configuration') {
       rtGradle.tool = 'brainshin-gradle' // Gradle 8.14.5 지정
       
       // 젠킨스가 JFrog 시스템 설정에 등록된 계정 정보를 이 저장소에 대리 주입하도록 선언합니다.
       rtGradle.deployer repo: 'brainshin-gradle-dev-local', server: server
       rtGradle.resolver repo: 'brainshin-gradle-dev', server: server
   }

   stage('Config Build Info') {
       buildInfo.env.capture = true
       buildInfo.env.filter.addInclude("*")
   }

   stage('Extra gradle configurations') {
       // build.gradle 내부의 com.jfrog.artifactory 플러그인을 인지하도록 true 유지
       rtGradle.usesPlugin = true 
   }

   stage('Exec Gradle') {
       // ⚠️ [중요] Deprecated 경고를 지우기 위해 buildFile 인자를 지우고, 
       // 최신 규격인 'artifactoryDeploy' 태스크명으로 안전하게 빌드를 실행합니다.
       rtGradle.run rootDir: "artifactory/", tasks: 'clean artifactoryDeploy', buildInfo: buildInfo
   }

   stage('Publish build info') {
       server.publishBuildInfo buildInfo
   }
}
