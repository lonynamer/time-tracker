node('docker-build'){
  cleanWs()
  def mvnHOME
  def dockerHOME
  stage('Preparations'){
    checkout scm
    //Check out from other repository
    //checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'http://github.com/omrisiri/course']]])
    mvnHome = tool 'M3'
    dockerHome = tool 'Docker-Client'
  }
  stage('Build'){
    sh """
       ${mvnHome}/bin/mvn clean package
    """
  }
  stage('Arvhive Artifacts'){
    archiveArtifacts "target/*.war"
//    sh "cp -rf /home/jenkins/workspace/dockerbuild/web/target/time-tracker-web-0.3.1.war ./clock.war"
  }
  stage('Deploy On Dockers'){
    sh """
       echo "FROM tomcat" >> Dockerfile
       echo "ADD target/*.war /usr/local/tomcat/webapps/" >> Dockerfile
       cat Dockerfile
       ${dockerHome}/bin/docker -H tcp://172.17.0.1:2376 build -t lonydeploy .
       ${dockerHome}/bin/docker -H tcp://172.17.0.1:2376 run -d -p 80:8080 -t lonydeploy
    """
  }
} 
