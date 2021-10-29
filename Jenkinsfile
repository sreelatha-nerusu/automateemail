pipeline {
    environment {
     BUILD_TRIGGER_BY = "${currentBuild.getBuildCauses()[0].shortDescription} / ${currentBuild.getBuildCauses()[0].userId}"
     BUILD_BRANCH_BY = "${currentBuild.getBuildCauses()[0].shortDescription} / ${currentBuild.getBuildCauses()[0].gitBrnach}"
     EMAIL_TO = "sh(GIT_BRANCH %GIT_BRANCH%)"
     BRANCH = "${env.BRANCH_NAME}"
     COMMIT = "${env.GIT_COMMIT}"
     AUTHOR = "${env.GIT_AUTHOR_NAME}"
     
   }
    agent any
    stages {
        stage('SCM') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/rajugcp/jenkins-maven.git']]])
                
                
            }
        }
        stage('Test') {
            steps {
                sh 'echo "$GIT_BRANCH"; exit 0'
                
            }
        }
        stage("Env Variables") {
            steps {
                sh 'echo $BRANCH'
                sh 'echo $COMMIT'
                sh 'echo $AUTHOR'
                echo "The build number is ${env.BUILD_NUMBER}"
                echo "You can also use \${BUILD_NUMBER} -> ${BUILD_NUMBER}"
                sh 'echo "I can access $BUILD_NUMBER in shell command as well."'
            }
        }
    
    }
    post {
        always {
            echo 'This will always run'
            
            
        }
        success {
            mail bcc: '', body: "<b>Build Success</b><br>\n<br>Project: ${env.JOB_NAME} <br>Branch: ${BRANCH}<br><br>CommitID: ${COMMIT}<br><br>Build Number: ${env.BUILD_NUMBER}<br> URL link: ${env.BUILD_URL}/console", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "JOB SUCCESS: Project name -> ${env.JOB_NAME}", to: "sreelathanerusu@gmail.com";
        }
        failure {
            
            mail bcc: '', body: "<b>Build Failed</b> <br>BUILD_TRIGGER_BY: ${BUILD_TRIGGER_BY}<br><br>\n<br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER}<br> URL link: ${env.BUILD_URL}/console", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "ERROR CI: Project name -> ${env.JOB_NAME}", to: "nagapatukuru@gmail.com";
            }
        unstable {
            echo 'This will run only if the run was marked as unstable'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }
    }
}
