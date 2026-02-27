pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "Maven_3.9.12"
    }

    stages {
        stage('Checkout'){
            steps{
                checkout scm
            }
        }

        stage('Setup'){
            steps{
                script{
                    startZap(host: "localhost", port:9091, timeout:500, zapHome: "C:\\Program Files\\ZAP\\Zed Attack Proxy", allowedHosts:['github.com'], sessionPath:"C:\\Program Files\\ZAP\\Zed Attack Proxy\\zap-2.17.0.jar")
                }
            }
        }
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/jglick/simple-maven-project-with-tests.git'

                // Run Maven on a Windows agent.
                bat "mvn -Dmaven.test.failure.ignore=true clean package"

                // To run Maven on a Unix agent, use
                // sh "mvn -Dmaven.test.failure.ignore=true clean package"
            }

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
    }
}




