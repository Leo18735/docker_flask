

node("master") {
    String githubToken = credentials('GITHUB_TOKEN')
    String repo = "Leo18735/docker_flask"
    String imageName = "my-docker-app"
    String imageFile = "$imageName.tar"

    stage('Get Latest Release') {
        sh """
        latest_release=\$(curl -s -H "Authorization: token ${githubToken}" https://api.github.com/repos/${repo}/releases/latest | jq -r .tag_name)
        curl -L -H "Authorization: token ${githubToken}" -o ${imageFile} https://github.com/${repo}/releases/download/\${latest_release}/${imageFile}
        """
    }

    stage('Load Docker Image') {
        sh "docker load < ${imageFile}"
    }

    stage('Run Docker Container') {
        sh "docker run -d $imageName"
    }
}
