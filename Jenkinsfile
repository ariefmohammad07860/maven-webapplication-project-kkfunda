node {
    // Fetch the Maven installation path configured in Jenkins
    def mavenHome = tool name: "maven-3.9.6"

    // Stage 1: Git Checkout
    stage('git checkout') {
        git branch: 'development',
            credentialsId: 'e91a6a9b-2b82-4be6-89f0-555a8ad4956c',
            url: 'https://github.com/ariefmohammad07860/maven-webapplication-project-kkfunda.git'
    }

    // Stage 2: Build the application
    stage('Build') {
        sh "${mavenHome}/bin/mvn clean package"
    }

    // Stage 3: SonarQube Code Quality Report
    stage('SQ Report') {
        sh "${mavenHome}/bin/mvn sonar:sonar"
    }

    // Stage 4: Upload to Nexus Repository
    stage('Deploy into Nexus') {
        sh "${mavenHome}/bin/mvn clean deploy"
    }

    // Stage 5: Deploy WAR to Tomcat using curl
    stage('Deploy to Tomcat') {
        echo "Deploying WAR file using curl..."

        sh """
            curl -u eshan:eshan \
            --upload-file /var/lib/jenkins/workspace/jio-prod-scriptedway-PL/target/maven-web-application.war \
            "http://34.232.165.151:8080/manager/text/deploy?path=/maven-web-application&update=true"
        """
    }
} // node closing
