pipeline{
    agent any
    stages{
        stage("Inicio") {
            steps {
                sh "echo Inicio"
            }
        }
        stage("Meio") {
            steps {
                sh "echo meio"
                sh "echo meio de novo"
            }
        }
        stage("Fim") {
            steps {
                sleep(5)
                sh "echo fim"
            }
        }        
    }
}
