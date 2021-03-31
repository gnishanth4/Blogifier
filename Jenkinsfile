String githubUrl = "https://github.com/gnishanth4/Blogifier.git"

node () {
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
          ps msbuild -r C:/Users/Administrator/Source/Repos/Blogifier/Blogifier.sln
      }
    }
    stage('Build') {
      steps {
        ps msbuild  C:/Users\Administrator\Source\Repos\Blogifier\Blogifier.sln /t:Publish /p:Configuration=Debug        
      }
    }
    stage('Deploy') {
       steps {
     ps msdeploy -verb:sync -source:contentPath=C:/Users/Administrator/source/repos/Blogifier/src/Blogifier/bin/Debug/net5.0/publish -dest:contentPath=C:/ProgramFiles/IIS/aspweb
    }  
}
