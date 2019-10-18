pipeline {
    agent none
    environment {
        IMAGE = "isbee/spinnaker-test"
    }
    stages {
        // stage('Build docker image') {
        //     agent any
        //     when { expression { gitTagName() && !gitTagName().equalsIgnoreCase("null") } }
        //     steps {
        //         sh "docker build -t ${IMAGE}:${gitTagName()} ."
        //         sh "docker images"
        //     }
        // }
        stage('Push docker container') {
            agent any
            when { expression { gitTagName() && !gitTagName().equalsIgnoreCase("null") } }
            steps {
                sh "docker images | grep ${IMAGE}:${gitTagName()} | awk '{system(\"docker rmi \" \$1 \":\" \$2)}'"
                sh "docker images"
                sh "docker rmi ${IMAGE}:0.0.5"
                sh "docker images"
                sh "docker rmi $(docker images -f \"dangling=true\" -q)"
                sh "docker images"
                // sh "docker login -u \"isbee\" -p \"dltmdgus2!\" docker.io"
                // sh "docker push ${IMAGE}:${gitTagName()}"
                // sh "docker images | grep ${IMAGE}:${gitTagName()} | awk '{system(\"docker rmi \" \$1 \":\" \$2)}'"
            }
        }
    }
}

// node {
//     git url: 'https://github.com/isbee/spin-kub-v2-demo'
//     env.IMAGE = "isbee/spinnaker-test"
//     env.GIT_TAG_NAME = gitTagName()

//     stage('Build docker image') {
//         if (GIT_TAG_NAME && !GIT_TAG_NAME.equalsIgnoreCase("null")) {
//             sh "docker build -t ${IMAGE}:${GIT_TAG_NAME} ."
//         }
//     }
//     stage('Push docker image') {
//         if (GIT_TAG_NAME && !GIT_TAG_NAME.equalsIgnoreCase("null")) {
//             sh "docker login -u \"isbee\" -p \"dltmdgus2!\" docker.io"
//             sh "docker push ${IMAGE}:${GIT_TAG_NAME}"
//         }
//     }
// }
// // Test47

/** @return The tag name, or `null` if the current commit isn't a tag. */
String gitTagName() {
    commit = getCommit()
    if (commit) {
        desc = sh(script: "git describe --tags ${commit}", returnStdout: true)?.trim()
        if (isTag(desc)) {
            return desc
        }
    }
    return null
}

/** @return The tag message, or `null` if the current commit isn't a tag. */
String gitTagMessage() {
    name = gitTagName()
    msg = sh(script: "git tag -n10000 -l ${name}", returnStdout: true)?.trim()
    if (msg) {
        return msg.substring(name.size()+1, msg.size())
    }
    return null
}

String getCommit() {
    return sh(script: 'git rev-parse HEAD', returnStdout: true)?.trim()
}

@NonCPS
boolean isTag(String desc) {
    match = desc =~ /.+-[0-9]+-g[0-9A-Fa-f]{6,}$/
    result = !match
    match = null // prevent serialisation
    return result
}
