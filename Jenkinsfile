node {

  // 🔹 Add Poll SCM Trigger
  properties([
    pipelineTriggers([
     // pollSCM('* * * * *')  // Polls SCM every 1 minute
     //cron('* * * * *'), 
      githubPush()  
    ])
  ])

  echo "git branch name: ${env.BRANCH_NAME}"
  echo "build number is: ${env.BUILD_NUMBER}"
  echo "node name is: ${env.NODE_NAME}"

  def mavenHome = tool name: "maven-3.9.6"

  try {

    stage('Git Checkout') {
      notifyBuild('STARTED')
      git branch: 'development', url: 'https://github.com/ariefmohammad07860/maven-webapplication-project-kkfunda.git'
    }

    stage('Compile') {
      sh "${mavenHome}/bin/mvn clean compile"
    }

    stage('Build') {
      sh "${mavenHome}/bin/mvn clean package"
    }

    stage('SonarQube Report') {
      sh "${mavenHome}/bin/mvn sonar:sonar"
    }

    stage('Upload Artifact') {
      sh "${mavenHome}/bin/mvn clean deploy"
    }

    stage('Deploy to Tomcat') {
      sh """
        curl -u arief:arief \
        --upload-file /var/lib/jenkins/workspace/jio-dev-scriptedway-PL/target/maven-web-application.war \
        "http://54.145.20.3:8080/manager/text/deploy?path=/maven-web-application&update=true"
      """
    }

  } catch (e) {
    currentBuild.result = "FAILED"
  } finally {
    notifyBuild(currentBuild.result)
  }

} // node ending


// 🔹 Slack Notification Function
def notifyBuild(String buildStatus = 'STARTED') {
  buildStatus = buildStatus ?: 'SUCCESS'

  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  if (buildStatus == 'STARTED') {
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESS') {
    colorCode = '#00FF00'
  }

  slackSend(color: colorCode, message: summary, channel: '#jio-devteam')
  //slackSend(color: colorCode, message: summary, channel: '#jio-devops')
}
