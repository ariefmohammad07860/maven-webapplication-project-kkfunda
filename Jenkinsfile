node {
    // Fetch the Maven installation path configured in Jenkins
    def mavenHome = tool name: "maven-3.9.6"

    // Stage 1: Git Checkout
    stage('git checkout') {
        git branch: 'development',
            credentialsId: 'e91a6a9b-2b82-4be6-89f0-555a8ad4956c',
            url: 'https://github.com/ariefmohammad07860/maven-webapplication-project-kkfunda.git'
    }

    
} // node closing
