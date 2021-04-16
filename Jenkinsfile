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
                withSonarQubeEnv('SONAR_LOCAL') {
                    sh "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://192.168.0.43:9005 -Dsonar.login=ba692afc4ae0b8fc7b52f3cbd02016861aa43eae -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=**mvn**,**/src/test/**,**/model/**,**Application.java"
                }
            }
        }


    }
}

