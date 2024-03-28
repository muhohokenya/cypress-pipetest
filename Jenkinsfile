pipeline {
    agent {
            node {
                label 'alpha'
            }
        }

    options {
        buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')
    }

    environment {
        jenkinsPod = ''
        cypressPod = ''
        logs = ''
        deploy = false
    }

    stages {
//         stage('Git Checkout') {
//             steps {
//                 script {
//                     git branch: 'master',
//                         url: 'https://github.com/muhohokenya/cypress-pipetest.git'
//
//                 }
//             }
//         }





        stage('Get pods') {
            steps {
                script {
                    sh '''
                    ls -la express-api
                     kubectl apply -f express-api/kubernetes/deployment.yml -n filetracker
                     kubectl get pods -n filetracker
                    '''
                }
            }
        }

    }
}

def waitForReport() {
    timeout(time: 5, unit: 'MINUTES') {
        script {
            def counter = 0
            while (!fileExists('/var/jenkins_home/html/index.html')) {
                counter++
                echo "Waiting for index.html file to exist... (Attempt ${counter})"
                sleep 10
            }
        }
    }
}


def fileExists(filePath) {
    return sh(script: "[ -f '$filePath' ]", returnStatus: true) == 0
}