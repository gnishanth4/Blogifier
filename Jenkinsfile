String githubUrl = "https://github.com/gnishanth4/Blogifier.git"

pipeline {
    
    agent any
    
    stage('Checkout') {
        checkout([
            $class: 'GitSCM', 
            branches: [[name: 'master']], 
            doGenerateSubmoduleConfigurations: false, 
            extensions: [], 
            submoduleCfg: [], 
            userRemoteConfigs: [[url: """ "${githubUrl}" """]]])
    }
    stage('Restore packages') {
       steps {
          powershell("msbuild -r C:/Users/Administrator/Source/Repos/Blogifier/Blogifier.sln")
      }
    }
    stage('Build') {
      steps {
        powershell("msbuild  C:/Users\Administrator\Source\Repos\Blogifier\Blogifier.sln /t:Publish /p:Configuration=Debug")        
      }
    }
    stage('Deploy') {
       steps {
        powershell("msdeploy -verb:sync -source:contentPath=C:/Users/Administrator/source/repos/Blogifier/src/Blogifier/bin/Debug/net5.0/publish -dest:contentPath=C:/ProgramFiles/IIS/aspweb")
    }  
}
