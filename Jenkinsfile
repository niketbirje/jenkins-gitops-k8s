node {

    properties([
        parameters([
            string(name: 'DOCKERTAG', defaultValue: '', description: 'Docker Image Tag')
        ])
    ])

    stage('Clone Repository') {
        git branch: 'main',
            credentialsId: 'github-token',
            url: 'https://github.com/niketbirje/jenkins-gitops-k8s.git'
    }

    stage('Update Manifest') {
        sh """
            echo "DOCKERTAG=${DOCKERTAG}"

            echo "===== BEFORE ====="
            cat deployment.yml

            sed -i "s|image: niket98/jenkins-flask:.*|image: niket98/jenkins-flask:${DOCKERTAG}|g" deployment.yml

            echo "===== AFTER ====="
            cat deployment.yml

            echo "===== GIT DIFF ====="
            git diff
        """
    }

    stage('Commit & Push') {
        withCredentials([
            usernamePassword(
                credentialsId: 'github-token',
                usernameVariable: 'GIT_USERNAME',
                passwordVariable: 'GIT_PASSWORD'
            )
        ]) {

            sh """
                git config user.email "birjeniket@gmail.com"
                git config user.name "niketbirje"

                git add deployment.yml

                git status

                git commit -m "Update image tag to ${DOCKERTAG}" || true

                git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/niketbirje/jenkins-gitops-k8s.git HEAD:main
            """
        }
    }
}
