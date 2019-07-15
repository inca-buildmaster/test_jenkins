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
            when { anyOf { branch 'master'; branch 'change_docker_version' } }
            steps {
                echo 'Testing...'
                sh "./test_simple"
                sh "echo '${params.Debug_or_Release} !!!!'"
                script {
                   echo "========="
                   if (params.Debug_or_Release == 'release') {
                        echo '*******param is release.*********'
                        def workspace = pwd()
                        println(workspace)
                        def foo = sh(script: 'pwd', returnStdout: true)
                        println(foo)
                        println(WORKSPACE)
                        sh label: '', script: 'rm test_simple'
                        sh label: '', script: 'gcc test_simple.c -o test_simple'
                        sh label: '', script: './test_simple'
                        sh label: '', script: 'sudo cp test_simple /home'
                    } else {
                        echo '*******param is debug.*********'
                        def workspace = pwd()
                        println(workspace)
                        def foo = sh(script: 'pwd', returnStdout: true)
                        println(foo)
                        println(WORKSPACE)
                        sh label: '', script: 'gcc test_simple.c -o test_simple'
                        sh label: '', script: './test_simple'
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
