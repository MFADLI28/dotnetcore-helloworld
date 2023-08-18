node('maven') {
	def appName="hello-world-dot-net-core-fadli"
	def projectName="sofyan-test2"
	def imageRegistry="image-registry.openshift-image-registry.svc:5000"

	def gitRepo="https://github.com/MFADLI28/dotnetcore-helloworld"
	def commitHash;

    stage('Clone') {
        sh "git config --global http.sslVerify false"
        sh "git clone ${gitRepo} source "
    }
    stage('Build and Deploy') {
        dir("source") {
            commitHash = sh(script: 'git rev-parse --short HEAD', returnStdout: true)
            
            sh "oc new-build --name=${appName} --image-stream=dotnet:7.0-ubi8 --binary=true -n ${projectName} || true"
            sh "oc start-build ${appName} --from-dir=.  --follow --wait -n ${projectName} || true"
            sh "oc new-app ${appName} --name=${appName} -n ${projectName} || true"
            sh "oc tag ${projectName}/${appName}:latest ${projectName}/${appName}:${commitHash}"
	    sh "oc set triggers deployment/${appName} --from-image=${projectName}/${appName}:latest -c ${appName} -n ${projectName} || true "
        }
    }
}
