node{
    def mavenHome = tool name: 'maven3.8.6'
    
    stage('1 Clone Repo'){
        git branch: 'main', url: 'https://github.com/Patioco/pet-clinic-war.git'
    }
    stage('2 Maven Build'){
        sh "${mavenHome}/bin/mvn clean package"
        
    }
    
    stage('3 Code Quality Review'){
        withSonarQubeEnv('Sonar-Server7.8'){
            sh "${mavenHome}/bin/mvn sonar:sonar"    
        }
        
    }
    
    stage('4 Deploy Built Artifacts'){
        nexusArtifactUploader artifacts: [[artifactId: '01-maven-web-app', classifier: '', file: 'target/01-maven-web-app.war', type: 'war']], credentialsId: 'Nexus-credentials', groupId: 'in.ashokit', nexusUrl: '172.31.73.159:8181/mydevops', nexusVersion: 'nexus3', protocol: 'http', repository: 'ashokit-snapshot', version: '1.0-SNAPSHOT'
    }
    
    //stage('5 Deploy to UAT'){
    //    deploy adapters: [tomcat9(credentialsId: 'tomcat-credential', path: '', url: 'http://172.31.81.99:8080/')], contextPath: null, war: 'target/*war'
    //}
    
    stage('6 Deploy to Prod'){
        sshagent(['Tomcat-key']) {
            sh 'scp -o StrictHostKeyChecking=no target/01-maven-web-app.war ec2-user@54.224.174.13:/opt/tomcat9/webapps'
    
        }
        
    }
}
