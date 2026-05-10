node {
  def myGradleContainer = docker.image('gradle:jdk8-alpine')
  def GRADLE_HOME = "/var/jenkins_home/.gradle"
  
  myGradleContainer.pull()
  
  stage('prep') {
    checkout scm
  }
  stage('test') {
     myGradleContainer.inside("-v ${GRADLE_HOME}:/home/gradle/.gradle") {
       sh 'cd complete && gradle test'
     }
  }
  stage('run') {
     myGradleContainer.inside("-v ${GRADLE_HOME}:/home/gradle/.gradle") {
       sh 'cd complete && gradle run'
     }
  }
}
