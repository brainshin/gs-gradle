node {
  def myGradleContainer = docker.image('gradle:jdk8-alpine')
  myGradleContainer.pull()
  
  stage('prep') {
    checkout scm
  }
  
  stage('test') {
     myGradleContainer.inside {
       sh 'cd complete && gradle test'
     }
  }
  
  stage('run') {
     myGradleContainer.inside {
       sh 'cd complete && gradle run'
     }
  }
}
