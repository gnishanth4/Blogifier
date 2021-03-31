String githubUrl = "https://github.com/gnishanth4/Blogifier.git"

pipeline {
  agent any
  stages {
    stage('Checkout') {
        steps {
        checkout([
            $class: 'GitSCM', 
            branches: [[name: 'master']], 
            doGenerateSubmoduleConfigurations: false, 
            extensions: [], 
            submoduleCfg: [], 
            userRemoteConfigs: [[url: """ "${githubUrl}" """]]])
     }        
    } 
    stage('Sonar Analysis') {
        steps {
            powershell("dotnet tool install --global dotnet-sonarscanner")
            powershell('dotnet sonarscanner begin /k:"Aspweb-App"  /d:sonar.login="ee5f103702e5d3eb33b0c4733e0d5f87a2ecb57a" ')
            powershell("dotnet build C:/Users/Administrator/Source/Repos/Blogifier/Blogifier.sln")  
            powershell("dotnet sonarscanner end /d:sonar.login="ee5f103702e5d3eb33b0c4733e0d5f87a2ecb57a" ")    
        }
    }   
    stage('Restore packages') {
       steps {
          powershell("msbuild -r C:/Users/Administrator/Source/Repos/Blogifier/Blogifier.sln")
      }
    }
    stage('Build') {
      steps {
        powershell("msbuild  C:/Users/Administrator/Source/Repos/Blogifier/Blogifier.sln /t:Publish /p:Configuration=Debug")        
      }
    }
     
    stage('Deploy') {
      steps {
        powershell("msdeploy -verb:sync -source:contentPath=C:/Users/Administrator/source/repos/Blogifier/src/Blogifier/bin/Debug/net5.0/publish -dest:contentPath=C:/ProgramFiles/IIS/aspweb")
    }  
  }
 }    
}
