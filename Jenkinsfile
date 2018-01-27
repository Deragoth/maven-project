pipeline {
    agent any
    stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
            }
            post {
                echo 'Now Archiving...'
                archiveArtifact artifact: '**/*.war'
            }
        }
    }
}
