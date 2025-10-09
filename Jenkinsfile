node {
    // Fetch the Maven installation path configured in Jenkins
    def mavenHome = tool name: "maven-3.9.6"

    // Stage 1: Git Checkout
    stage('git checkout') {
        git branch: 'development',
            credentialsId: 'e91a6a9b-2b82-4be6-89f0-555a8ad4956c',
            url: 'https://github.com/ariefmohammad07860/maven-webapplication-project-kkfunda.git'
    }

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

  slackSend(color: colorCode, message: summary, channel: '#bsnl-main')
  //slackSend(color: colorCode, message: summary, channel: '#jio-devops')
}
} // node closing
