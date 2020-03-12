node {

	def Resultfile = "C:\\Program Files (x86)\\Jenkins\\workspace\\TestPipeLine\\Test\\Results.trx"
	def existsResult = fileExists Resultfile
	
	stage('Checkout') {
			git 'https://github.com/junjiewe/DotnetApp.git'
	}
	stage('Build') {
		powershell '''
			cd "C:\\Program Files (x86)\\Microsoft Visual Studio\\2019\\Community\\MSBuild\\Current\\Bin\\"
			.\\MSBuild.exe "C:\\Users\\610169\\Desktop\\DotnetApp\\Hello\\Hello.sln" /p:Configuration=Release
		'''
	}
	stage('Test') {
		if (existsResult){
			powershell 'ri Resultfile'
			powershell 'ni -name Resultfile'
		}else {
			powershell 'ni -name Resultfile'
		}

		powershell '''
			cd "C:\\Program Files (x86)\\Microsoft Visual Studio\\2019\\Community\\Common7\\IDE\\"
			.\\MSTest.exe /testcontainer:"C:\\Users\\610169\\Desktop\\DotnetApp\\Hello\\HelloTest\\bin\\Debug\\HelloTest.dll" /resultsfile:Resultfile
		'''
	}
	stage('SonarQube analysis'){
		withSonarQubeEnv('SonarQube') { 
		
		powershell '''
			cd "C:\\Users\\610169\\Documents\\sonar-scanner-4.2.0.1873-windows\\bin"
			.\\sonar-scanner.bat -D sonar.projectKey=onePipe -D Sonar.ProjectName=TestApp -D Sonar.ProjectVersion=1.0 -D Sonar.Login=admin -D Sonar.Password=admin sonar.sources="C:\\Users\\610169\\Desktop\\DotnetApp\\Hello\\Hello.sln"'''
		}
	}
}