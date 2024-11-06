pipeline {
    agent any

    environment {
        REGISTRY_CREDENTIAL = "dockerhub_creds"
    }

    stages {

        stage('Git') {
            steps {
                git branch: 'main',
                url: 'https://github.com/itgenius-devops/application-code.git'
            }
        }

        stage('Test'){
            steps{
                echo 'test'
            }
        }

        stage('Docker build and push'){
            steps{
                script {
                    docker.withRegistry('', REGISTRY_CREDENTIAL) {
                sh """
                chmod +x ./mvnw
                ./mvnw clean install
                docker rmi -f itgeniusdevops/itgenius-app-project-repo &>/dev/null && echo 'Removed old images'
                docker build -t itgeniusdevops/itgenius-app-project-repo:v001 .
                docker push itgeniusdevops/itgenius-app-project-repo:v001
                """
                
                }
            }
        }
        }


/*
        stage('Deploy to server'){
            steps{
                sshPublisher(publishers: [sshPublisherDesc(configName: 'test-server',
                transfers: [sshTransfer(cleanRemote: false, excludes: '',
                execCommand: '''cd /home/ec2-user/
                sh deployment.sh''',
                execTimeout: 120000,
                flatten: false,
                makeEmptyDirs: false,
                noDefaultExcludes: false,
                patternSeparator: '[, ]+',
                remoteDirectory: '',
                remoteDirectorySDF: false,
                removePrefix: '',
                sourceFiles: '')],
                usePromotionTimestamp: false,
                useWorkspaceInPromotion: false,
                verbose: false)])
            }
        } */
    }
}
