pipeline {
    agent any

    triggers {
        githubPush()  // Trigger otomatis saat push GitHub
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo 'Cloning repository...'
                checkout scm
            }
        }

        stage('Test Build') {
            steps {
                echo '🚀 Build Success! This is a simple CI from GitHub to Jenkins.'
            }
        }
    }
}
