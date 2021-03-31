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
    stage ('UnitTests') {
        steps {  
    bat returnStatus: true, script: "\"C:/Program Files/dotnet/dotnet.exe\" test \"${workspace}/Blogifier.sln\" --logger \"trx;LogFileName=unit_tests.xml\" --no-build"
    step([$class: 'MSTestPublisher', testResultsFile:"**/unit_tests.xml", failOnError: true, keepLongStdio: true])  
        
       }
    } 
    stage('Deploy') {
      steps {
        powershell("msdeploy -verb:sync -source:contentPath=C:/Users/Administrator/source/repos/Blogifier/src/Blogifier/bin/Debug/net5.0/publish -dest:contentPath=C:/ProgramFiles/IIS/aspweb")
    }  
  }
 }    
}
