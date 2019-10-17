// pipeline {
//     agent none
//     // parameters {
//     //     string(name: 'IMAGE', defaultValue: 'isbee/spinnaker-test:2.1.6', description: 'Spinnaker with Jenkins')
//     // }
//     stages {
//         stage('Build docker image') {
//             agent any
//             steps {
//                 sh "docker build -t ${IMAGE}:${GIT_TAG_NAME} ."
//                 // sh "docker build -t ${params.IMAGE} ."
//             }
//         }
//         stage('Push docker image') {
//             agent any
//             steps {
// 				sh "docker login -u \"isbee\" -p \"dltmdgus2!\" docker.io"
//                 sh "docker push ${IMAGE}:${GIT_TAG_NAME}"
//                 // sh "docker push ${params.IMAGE}"
//             }
//         }
//     }
// }
//
// Test
node {
    git url: 'https://github.com/isbee/spin-kub-v2-demo'
    env.IMAGE = "isbee/spinnaker-test"
    env.GIT_TAG_NAME = gitTagName()
    print(env.GIT_TAG_NAME)

    stage('Build docker image') {
        agent any
        steps {
            sh "docker build -t ${IMAGE}:${GIT_TAG_NAME} ."
            // sh "docker build -t ${params.IMAGE} ."
        }
    }
    stage('Push docker image') {
        agent any
        steps {
			sh "docker login -u \"isbee\" -p \"dltmdgus2!\" docker.io"
            sh "docker push ${IMAGE}:${GIT_TAG_NAME}"
            // sh "docker push ${params.IMAGE}"
        }
    }
}

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
