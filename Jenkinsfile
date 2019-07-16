pipeline {
    agent any
    parameters {
        choice(choices: ['debug', 'release'], description: 'build for debug or release?', name: 'Debug_or_Release')
    }
    stages {
       stage('clean workspace') {
            steps {
                sh "ls -al ${env.WORKSPACE}"
                /*deleteDir()*/
                sh "ls -al ${env.WORKSPACE}"
            }
        }
        stage('Build') {
            steps {
                echo 'Building..'
                echo 'Pulling... ' + env.GIT_BRANCH
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
                        println(workspace)
                        def foo = sh(script: 'pwd', returnStdout: true)
                        println(foo)
                        println(WORKSPACE)
                        sh label: '', script: 'rm test_simple'
                        sh label: '', script: 'gcc test_simple.c -o test_simple'
                        sh label: '', script: './test_simple'
                        sh label: '', script: 'sudo cp test_simple /home'
                        sh label: '', script: 'scp -i ~/.ssh/id_rsa_build_master test_simple root@192.168.129.196:/tmp'
                    } else {
                        echo '*******param is debug.*********'
                        def workspace = pwd()
                        println(workspace)
                        def foo = sh(script: 'pwd', returnStdout: true)
                        println(foo)
                        println(WORKSPACE)
                        sh label: '', script: 'gcc test_simple.c -o test_simple'
                        sh label: '', script: 'scp -i ~/.ssh/id_rsa_build_master test_simple root@192.168.129.196:/tmp'
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
    post {
        always {
            echo "${env.WORKSPACE}"
            sh 'sudo rm -rf ${env.WORKSPACE}'
        }
        success {
            echo 'Build is succeeeded!'
        }
        unstable {
            echo 'Build is unstable'
        }
        failure {
            echo 'Build is failed'
        }
    }
}
