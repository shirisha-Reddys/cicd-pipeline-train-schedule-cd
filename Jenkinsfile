pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
      
        }
        
        stage('DeployToStaging){
              when{
         branch 'master'
              }
              steps{
                  withCredentials{[usernamePassword(credentialsId:'webserver-login',usernameVariable:'USERNAME', passwordVariable:'USERPASS')]}{    
    sshPublisher(
    failOnError:true,
continueOnerror:false,        
        publishers:[
            sshPublisherDesc{
                configName:'staging',
        sshCredentials:[
            username:"$USERNAME",
            encryptedPassphrase:"$USERPASS"
            ],
        transfers:[
        sshTransfer(
            sourceFiles:'dist/trainSchedule.zip',
            removePrefix:'dist/',
        remoteDirectory:'/tmp',
        execCommand:'sudo /user/bin/systemctl stop train-shedule && rm-rf /opt/train-shedule/* && unzip /tmp/trainSchedule.zip -d /opt/train-shedule &&  sudo /usr/bin/systemctl start train-shedule
            
    
    
    
    )
    ]
    )
    ]
    )
            }
            }
}
