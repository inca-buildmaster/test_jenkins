pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                sh "gcc test_simple.c -o test_simple"
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
                sh "./test_simple"
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
