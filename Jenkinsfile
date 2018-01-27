pipeline {
    agent any
    stages{
        stage(' Build'){
            steps {
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifact artifact: '**/target/*.war'
                }
            }
        }
    }
}
