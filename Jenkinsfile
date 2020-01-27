pipeline {
  options {
    buildDiscarder(logRotator(numToKeepStr: '5', artifactNumToKeepStr: '0'))
  }
  agent {
    kubernetes {
      //cloud 'kubernetes'
      yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: maven
    image: danielrmartin/sko:1.0
    command: ['cat']
    tty: true
"""
    }
  }
  stages {
    stage('Run Maven') {
      steps {
        container('maven') {
          sh 'mvn deploy -f ./complete/pom.xml'
        }
      }
    }
    stage ('Run Sonarqube'){
      steps{
        container('maven'){
          sh 'mvn sonar:sonar -f ./complete/pom.xml'
        }}}
  }
  post {
    successful{
      //step([
              $class: 'ElectricFlowPipelinePublisher',
              configuration: 'flow-sko-demo',
              projectName: 'flow-sko',
              pipelineName: 'flow-sko-uc-1',
              addParam: '{"pipeline":{"pipelineName":"flow-sko-demo","parameters":"[{\\\"parameterName\\\": \\\"PipelineParam\\\", \\\"parameterValue\\\": \\\"185\\\"}]"}}'
      //])
    }
   }
}
