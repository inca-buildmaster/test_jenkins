pipeline {
    agent any
    parameters {
        choice(choices: ['debug', 'release'], description: 'build for debug or release?', name: 'Debug_or_Release')
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
                sh "echo '${params.Debug_or_Release} !!!!'"
                script {
                   echo "========="
                   if (params.Debug_or_Release == 'release') {
                        echo '*******param is release.*********'
                        def workspace = pwd()
                        echo $workspace
                    } else {
                        echo '*******param is debug.*********'
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
