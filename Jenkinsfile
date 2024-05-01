pipeline {
    agent any
    stages {

        stage("Clone"){
            steps{
                checkout scm 
                echo 'GitOps Code clone in /var/lib/jenkins/workspace'
            }
        }
        stage("Update GIT"){
            steps{
                script {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    withCredentials([sshUserPrivateKey(credentialsId: "git", keyFileVariable: 'key')]) {
                        sh "pwd"
                        sh "ls -la"
                        sh "cat ./templates/front_end_dep_service.yaml"
                        sh "sed -i 's+{{ .Values.images.frontend }}.*+{{ .Values.images.frontend }}:${DOCKERTAG}+g' ./templates/front_end_dep_service.yaml"
                        sh "cat ./templates/front_end_dep_service.yaml"
                        sh "git add ."
                        sh "git commit -m 'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER}'"
                        sh "GIT_SSH_COMMAND='ssh -o StrictHostKeyChecking=no -i ${key}' git push git@github.com-adkharat:adkharat/eks-gitops.git HEAD:main"
                    }
                }
                }
            }
        }
        
    }
}