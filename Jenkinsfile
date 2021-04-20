pipeline{
    agent any
    stages{
        stage("Build Backend") {
            steps {
                sh "mvn clean package -DskipTests=true"
            }
        }
        stage("Backend Unit Tests") {
            steps {
                sh "mvn test"
            }
        }
        stage("Backend Sonar Analysis") {
            environment {
                scannerHome = tool "SONAR_SCANNER"
            }
            steps {
                withSonarQubeEnv('sonar_local') {
                    sh "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://192.168.0.43:9005 -Dsonar.login=ba692afc4ae0b8fc7b52f3cbd02016861aa43eae -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=**mvn**,**/src/test/**,**/model/**,**Application.java"
                }
            }
        }
        stage("Quality gate") {
            steps {
                sleep(10)
                timeout(time: 1, unit: "MINUTES") {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
        stage("Deploy Backend") {
            steps {
                deploy adapters: [tomcat8(credentialsId: 'TomcatAdmin', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'
            }
        }

        stage("API Test") {
            steps {
                dir("api-test") {
                    git branch: 'main', url: 'https://github.com/juliano-secondo/tasks-api-test'
                    sh 'mvn test'
                }
            }
        }

    }
}

