

node("master") {
    String repo = "Leo18735/docker_flask"
    String imageName = "my-docker-app"
    String imageFile = "${imageName}.tar"

    stage('Get Latest Release') {
        withCredentials([string(credentialsId: 'GITHUB_TOKEN', variable: 'GITHUB_TOKEN')]) {
            String latestRelease = sh(
                script: "curl -s -H \"Authorization: token ${GITHUB_TOKEN}\" https://api.github.com/repos/$repo/releases/latest | jq -r .tag_name",
                returnStdout: true
            ).trim()
            sh(script: "curl -L -H \"Authorization: token ${GITHUB_TOKEN}\" -o ${imageFile} https://github.com/${repo}/releases/download/${latestRelease}/${imageFile}")
        }
    }

    stage('Load Docker Image') {
        sh "docker load < ${imageFile}"
    }

    stage('Run Docker Container') {
        sh "docker run -p 9000:8080 $imageName"
    }
}
