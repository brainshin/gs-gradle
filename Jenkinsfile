node {
   def server = Artifactory.server('trialsfhhhn.jfrog.io')
   def rtGradle = Artifactory.newGradleBuild()
   def buildInfo = Artifactory.newBuildInfo()

   stage('Build') {
       git url: 'github.com'
   }

   stage('Artifactory configuration') {
       rtGradle.tool = 'brainshin-gradle' 
       rtGradle.deployer repo: 'brainshin-gradle-dev-local', server: server
       rtGradle.resolver repo: 'brainshin-gradle-dev', server: server
   }

   stage('Config Build Info') {
       buildInfo.env.capture = true
       buildInfo.env.filter.addInclude("*")
   }

   stage('Extra gradle configurations') {
       // ⚠️ [중요] 소스 내부의 플러그인 설정을 무시하고 젠킨스가 직접 배포하도록 false로 변경합니다.
       rtGradle.usesPlugin = false 
   }

   stage('Exec Gradle') {
       // 젠킨스 툴이 직접 빌드 후 아티팩토리를 추출하여 JFrog로 강제 전송합니다.
       rtGradle.run rootDir: "artifactory/", buildFile: 'build.gradle', tasks: 'clean artifactoryPublish', buildInfo: buildInfo
   }

   stage('Publish build info') {
       server.publishBuildInfo buildInfo
   }
}
