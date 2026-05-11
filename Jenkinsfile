node {
   def server = Artifactory.server('trialsfhhhn.jfrog.io')
   def rtGradle = Artifactory.newGradleBuild()
   def buildInfo = Artifactory.newBuildInfo()

   stage('Build') {
       git url: 'https://github.com/brainshin-star/gs-gradle.git'
   }

   stage('Artifactory configuration') {
       rtGradle.tool = 'brainshin-gradle' 
       
       // 결과물을 올리는 배포 창고만 지정합니다.
       rtGradle.deployer repo: 'brainshin-gradle-dev-local', server: server
       
       // ⚠️ [중요] 라이브러리를 내 JFrog 안에서만 찾게 강제하던 충돌 유발 코드를 제거(주석처리)합니다.
       // rtGradle.resolver repo: 'brainshin-gradle-dev', server: server
   }

   stage('Config Build Info') {
       buildInfo.env.capture = true
       buildInfo.env.filter.addInclude("*")
   }

   stage('Extra gradle configurations') {
       rtGradle.usesPlugin = true 
   }

   stage('Exec Gradle') {
       // ⚠️ [중요] Deprecated 경고를 완벽히 지우기 위해 오직 rootDir만 남기고,
       // 최신 규격인 'artifactoryDeploy' 태스크명으로 안전하게 빌드를 가동합니다.
       rtGradle.run rootDir: "artifactory/", tasks: 'clean artifactoryDeploy', buildInfo: buildInfo
   }

   stage('Publish build info') {
       server.publishBuildInfo buildInfo
   }
}
