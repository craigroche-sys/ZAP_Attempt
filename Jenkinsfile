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
                    startZap(host: "localhost", port:9091, timeout:500, zapHome: "C:\\Program Files\\ZAP\\Zed Attack Proxy", allowedHosts:['github.com'])
                }
            }
        }
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/craigroche-sys/Maven_Test.git'

                // Run Maven on a Windows agent.
                bat "mvn -Dmaven.test.failure.ignore=true clean package"

                // To run Maven on a Unix agent, use
                // sh "mvn -Dmaven.test.failure.ignore=true clean package"
            }

            post {
                always{
                    script{
                    archiveZap()
                    }
                }
            }
        }
    }
}


















