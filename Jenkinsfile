node {
    // Environment variables
    def sonarQubeServer = 'sonarqube'
    def projectKey = 'project1'
    def sonarHostUrl = 'http://localhost:9000'
    def sonarLoginToken = 'jenkins-sonar'

    try {
        stage('Checkout') {
            // Checkout source code from SCM
            checkout scm
        }

        stage('Build') {
            // Your build commands here
            echo 'Building project...'
        }

        stage('SonarQube Analysis') {
            // Running SonarQube analysis
            withSonarQubeEnv(sonarQubeServer) {
                sh "sonar-scanner -Dsonar.projectKey=${projectKey} -Dsonar.sources=. -Dsonar.host.url=${sonarHostUrl} -Dsonar.login=${sonarLoginToken}"
            }
        }

        stage('Quality Gate') {
            // Wait for SonarQube's Quality Gate result
            timeout(time: 1, unit: 'HOURS') {
                def qg = waitForQualityGate()
                if (qg.status != 'OK') {
                    error "Pipeline aborted due to quality gate failure: ${qg.status}"
                }
            }
        }
    } catch (Exception e) {
        echo e.toString()
        throw e
    } finally {
        // Post-build actions like cleanup, notifications, etc.
    }
}
