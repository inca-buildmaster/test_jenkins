pipeline {
    agent any
    parameters {
        string(name: 'Debug_or_Release', defaultValue: 'release', description: 'build for debug or release?')
    }
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
                sh "echo "${params.Debug_or_Release} !!!!""
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
