node {
   // 젠킨스 시스템 설정에 등록된 정식 JFrog Server ID 매핑
   def server = Artifactory.server('trialsfhhhn.jfrog.io')
   def rtGradle = Artifactory.newGradleBuild()
   def buildInfo = Artifactory.newBuildInfo()

   stage('Build') {
       git url: 'github.com'
   }

   stage('Artifactory configuration') {
       // 툴 이름은 웹툴에 등록해 둔 brainshin-gradle을 가리킵니다.
       rtGradle.tool = 'brainshin-gradle' 
       rtGradle.deployer repo: 'brainshin-gradle-dev-local', server: server
       rtGradle.resolver repo: 'brainshin-gradle-dev', server: server
   }

   stage('Config Build Info') {
       buildInfo.env.capture = true
       buildInfo.env.filter.addInclude("*")
   }

   stage('Extra gradle configurations') {
       // build.gradle 내부의 최신 plugins {} 구문을 인지하도록 true 유지
       rtGradle.usesPlugin = true 
   }

   stage('Exec Gradle') {
       // ⚠️ [중요] 구형 명령어(artifactoryPublish)는 에러를 뿜으므로 최신 규격인 'artifactoryDeploy'를 실행합니다.
       rtGradle.run rootDir: "artifactory/", buildFile: 'build.gradle', tasks: 'clean artifactoryDeploy', buildInfo: buildInfo
   }

   stage('Publish build info') {
       // 빌드 결과 정보를 JFrog 플랫폼 대시보드에 최종 전송합니다.
       server.publishBuildInfo buildInfo
   }
}
