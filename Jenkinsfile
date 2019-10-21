pipeline {
    agent none
    environment {
        IMAGE = "isbee/spinnaker-test"
        HOST = "gcr.io"
        PROJECT = "deeply-listen"
    }
    stages {
        stage('Build & Push docker image') {
            agent any
            when { expression { gitTagName() && !gitTagName().equalsIgnoreCase("null") } }
            steps {
                sh "docker build -t ${HOST}/${PROJECT}/${IMAGE}:${gitTagName()} ."
                // sh "docker build -t ${IMAGE}:${gitTagName()} ."

                // sh "docker login -u \"isbee\" -p \"dltmdgus2!\" docker.io"
                // sh "docker image inspect ${IMAGE}:${gitTagName()} >/dev/null 2>&1 && echo yes || echo no"
                sh "docker push ${HOST}/${PROJECT}/${IMAGE}:${gitTagName()}"
                sh "docker images | grep ${HOST}/${PROJECT}/${IMAGE}:${gitTagName()} | awk '{system(\"docker rmi -f \" \$1 \":\" \$2)}'"
                // sh "docker images | grep ${IMAGE}:${gitTagName()} | awk '{system(\"docker rmi -f \" \$1 \":\" \$2)}'"
            }
        }
    }
}
// Test57

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
