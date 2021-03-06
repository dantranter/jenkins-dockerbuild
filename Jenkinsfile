/**
 * This pipeline will run a Docker image build
 */

def label = "docker-${UUID.randomUUID().toString()}"

podTemplate(label: label, yaml: """
apiVersion: v1
kind: Pod
spec:
  serviceAccountName: dockerbuild
  containers:
  - name: docker
    image: docker:1.11
    command: ['cat']
    tty: true
    volumeMounts:
    - name: dockersock
      mountPath: /var/run/docker.sock
  volumes:
  - name: dockersock
    hostPath:
      path: /var/run/docker.sock
"""
  ) {

  def image = "jenkins/jnlp-slave"
  node(label) {
    stage('Build Docker image') {
      git 'https://github.com/jenkinsci/docker-jnlp-slave.git'
      container('docker') {
        sh "docker build -t ${image} ."
      }
    }
/*    stage('Deploy Docker image to OpenShift') {
      git 'https://github.com/jenkinsci/docker-jnlp-slave.git'
      container('docker') {
        sh "docker push docker-registry-default.127.0.0.1.nip.io:80/pushed/e3950b3f70a0:latest"
      }
    }
*/
  }
}
