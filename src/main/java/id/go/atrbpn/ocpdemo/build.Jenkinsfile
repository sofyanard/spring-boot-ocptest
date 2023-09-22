node('base') {
	def appName="springbootdemo"
	def projectName="sofyan-test2"
	def gitBranch="main"

    stage('Clone') {
        sh "git config --global http.sslVerify false"
        sh "git clone -b ${gitBranch} https://github.com/sofyanard/spring-boot-ocptest source "
    }
    stage('Build and Deploy') {
        dir("source") {
            sh "oc new-build --name=${appName} --image-stream=openjdk:11-ubi8 --binary=true -n ${projectName} || true"
            sh "oc start-build ${appName} --from-dir=.  --follow --wait -n ${projectName} || true"
            sh "oc new-app ${appName} --name=${appName} -n ${projectName} || true"
            
            def commitHash = sh(script: 'git rev-parse --short HEAD', returnStdout: true)
            sh "oc tag ${projectName}/${appName}:latest ${projectName}/${appName}:${commitHash}"

            sh "echo \"successfully build and deploy ${appName} to ${projectName} \" "
        }
    }
}
