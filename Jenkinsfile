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
                echo '===' + env.BUILD_TAG
                sh "gcc test_simple.c -o test_simple"
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
                sh "./test_simple"
                sh "echo '${params.Debug_or_Release} !!!!'"
                echo env.BUILD_NUMBER
                echo env.BUILD_ID
                script {
                   echo "=========$Binary_image_build_option=========="
                   echo "=========$Chassis_ip_address========="
                   def dateFormat = new SimpleDateFormat("yyyyMMddHHmm")
                   def date = new Date()
                   println(dateFormat.format(date))
                   if (Binary_image_build_option == 'release') {
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
                        echo 'build_number is' + env.BUILD_NUMBER
                        echo 'build_id is' + env.BUILD_ID
                    } else if (Binary_image_build_option == 'development') {
                        echo '*******param is debug.*********'
                        def workspace = pwd()
                        println(workspace)
                        def foo = sh(script: 'pwd', returnStdout: true)
                        println(foo)
                        println(WORKSPACE)
                        sh label: '', script: 'gcc test_simple.c -o test_simple'
                       if(Chassis_ip_address != "don't send cpio to chassis") {
                           sh label: '', script: 'scp -i ~/.ssh/id_rsa_build_master test_simple root@$Chassis_ip_address:/tmp'
                       } else {
                           echo "don't send cpio to chassis"
                       }
                        echo 'build_number is' + env.BUILD_NUMBER
                        echo 'build_id is' + env.BUILD_ID
                    }
                }
                echo 'deleting...'
                sh "sudo rm -rf *"
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
