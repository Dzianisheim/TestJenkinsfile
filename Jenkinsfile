def adtionalConfigNodes = """
  <connectionStrings>
    <add name="ADConnectionString" connectionString="LDAP://icx.local/DC=icx,DC=local" />
  </connectionStrings>
  <system.web>
    <membership defaultProvider="ADMembershipProvider">
      <providers>
        <clear />
        <add name="ADMembershipProvider" type="System.Web.Security.ActiveDirectoryMembershipProvider" connectionUsername="ITECHARTDTA\\itechart.dta" connectionPassword="n1x82r\$L3Kt4LT" connectionStringName="ADConnectionString" attributeMapUsername="sAMAccountName" enableSearchMethods="true" />
      </providers>
    </membership>
  </system.web>
"""
def configFileName = 'iTechArt.DTA.Web.exe.config'
def appsettingsFileId = 'appsettings.json:62e75ffc-f357-471a-b7ab-059cb078f102'
def webConfigFileId = 'web.config:a1131397-50d1-4b5c-8dcc-062b84aa15a4'

node {
    stage('Preparation') {
        //git branch: 'develop', credentialsId: 'ITECHARTDTA\\GitCredentials', url: 'https://git.itechart-group.com/scm/git/itechart.dta'
        def scmVars = checkout([
            $class: 'GitSCM', 
            branches: [[name: '*/develop']], 
            doGenerateSubmoduleConfigurations: false, 
            extensions: [], 
            submoduleCfg: [], 
            userRemoteConfigs: [[credentialsId: 'ITECHARTDTA\\GitCredentials', url: 'https://git.itechart-group.com/scm/git/itechart.dta']]
        ])
        
        println env.GIT_BRANCH
        
        env.GIT_COMMIT = scmVars.GIT_COMMIT
        env.GIT_BRANCH = scmVars.GIT_BRANCH
        
        dir('SourceCode\\DeviceTrackingApplication') {
            bat 'nuget restore'
        }
    }
    
    stage('Test') {

    }
    
    stage('Publish') {
        dir('SourceCode\\DeviceTrackingApplication\\iTechArt.DTA.Web') {
            bat 'npm i'
            bat 'bower i'
            bat 'gulp frontend:rebuild'
            
            bat 'dotnet publish --configuration Release --output bin\\PublishOutput'
            
            dir('bin\\PublishOutput') {
                bat 'del /S appsettings.*.json'
                bat 'del /S appsettings.json'
                bat 'del /S web.config'
                
                def fileContents = readFile file: configFileName, encoding: 'UTF-8'
                fileContents = fileContents.replace('<configuration>', '<configuration>' + adtionalConfigNodes)
                writeFile file: configFileName, text: fileContents, encoding: "UTF-8"
                
                configFileProvider([
                    configFile(fileId: appsettingsFileId, replaceTokens: true, targetLocation: 'appsettings.json'),
                    configFile(fileId: webConfigFileId, replaceTokens: true, targetLocation: 'web.config')
                ]) {
                    bat 'RD /S /Q C:\\App\\DTA-TEMP'
                    bat '(robocopy .\\ C:\\App\\DTA-TEMP /E)  ^& IF %ERRORLEVEL% LEQ 4 exit /B 0'
                }

                deleteDir()
            }
        }
    }
}