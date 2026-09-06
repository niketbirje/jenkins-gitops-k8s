node {
    def app

    stage('Clone repository') {
      

        checkout scm
    }

    stage('Update GIT') {
            script {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    withCredentials([usernamePassword(credentialsId: 'github-token', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                        //def encodedPassword = URLEncoder.encode("$GIT_PASSWORD",'UTF-8')
                        sh "git config user.email birjeniket@gmail.com"
                        sh "git config user.name niketbirje"
                        //sh "git switch master"
                        sh "cat deployment.yml"
                        
                        
                        sh 'sed -i "s|image: niket98/jenkins-flask:.*|image: niket98/jenkins-flask:' + DOCKERTAG + '|g" deployment.yml'                    
                        sh "cat deployment.yml"
                        sh "git add ."
                        sh "git commit -m 'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER}'"
                        sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/jenkins-gitops-k8s.git HEAD:main"
      }
    }
  }
}
}
