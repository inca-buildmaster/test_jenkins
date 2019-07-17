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
                sh "echo Testing...."
		        sh "echo 'build_number is' + env.BUILD_NUMBER"
                echo 'build_id is: ' + env.BUILD_ID
                echo 'build tag is: ' + env.BUILD_TAG
                echo 'git branch is: ' + env.GIT_BRANCH
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
                sh "./test_simple"
                sh "echo '${params.Debug_or_Release} !!!!'"
                script {
                   echo "=========$Binary_image_build_option=========="
                   echo "=========$Chassis_ip_address========="
                   /*echo "${currentBuild.buildCauses}"*/
		   /*def isStartedByUser = currentBuild.rawBuild.getCause(hudson.model.Cause$UserIdCause) != null*/
			// started by commit
			currentBuild.getBuildCauses('jenkins.branch.BranchEventCause')
			// started by timer
			currentBuild.getBuildCauses('hudson.triggers.TimerTrigger$TimerTriggerCause')
			// started by user
			currentBuild.getBuildCauses('hudson.model.Cause$UserIdCause')
				
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
                        echo 'build tag is:' + env.BUILD_TAG
                        echo 'git branch is:' + env.GIT_BRANCH
                        def now = new Date()
                        println now.format("yyyyMMdd.HHmm")
                        def time_stamp = now.format("yyyyMMdd.HHmm")
                        def test11 = time_stamp.toString()
                        echo test11
                        sh label: '', script: 'cp test_simple test_simple."$BUILD_TAG"'
			if((Chassis_ip_address != "don't send cpio to chassis")||(Chassis_ip_address == null)){
			  echo '==11==empty chassis ip====='
			}
                    } else if (Binary_image_build_option == 'development') {
                        echo '*******param is debug.*********'
                        def workspace = pwd()
                        println(workspace)
                        def foo = sh(script: 'pwd', returnStdout: true)
                        println(foo)
                        println(WORKSPACE)
                        sh label: '', script: 'gcc test_simple.c -o test_simple'
                       if((Chassis_ip_address != "don't send cpio to chassis") && (!"".equals(Chassis_ip_address))){
                           /*sh label: '', script: 'scp -i ~/.ssh/id_rsa_build_master test_simple root@$Chassis_ip_address:/tmp'*/
                       } else {
                           echo "don't send cpio to chassis"
                       }
			   
		       if ("".equals(Chassis_ip_address)) {
			  echo '==euqal empty==empty chassis ip====='
		       }
			if (!"".equals(Chassis_ip_address)) {
			  echo '== no equal empty ==chassis ip====='
		       }
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
