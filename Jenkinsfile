node {

	def Resultfile = "C:\\Program Files (x86)\\Jenkins\\workspace\\TestPipeLine\\Test\\Results.trx"
	def codeCheckPath = "C:\\Users\\610169\\Documents\\sonar-scanner-4.2.0.1873-windows\\bin\\Hello.sln"
	def existsResult = fileExists Resultfile
	def existsCodeCheck = fileExists codeCheckPath
	
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
			if (existsCodeCheck){
				powershell 'ri codeCheckPath'
				powershell 'Copy-Item "C:\\Users\\610169\\Desktop\\DotnetApp\\Hello\\Hello.sln" -Destination "C:\\Users\\610169\\Documents\\sonar-scanner-4.2.0.1873-windows\\bin"'
			} else {
				powershell 'Copy-Item "C:\\Users\\610169\\Desktop\\DotnetApp\\Hello\\Hello.sln" -Destination "C:\\Users\\610169\\Documents\\sonar-scanner-4.2.0.1873-windows\\bin"'
			}
			.\\sonar-scanner.bat -X -D sonar.projectKey=onePipeline -D sonar.projectName=DotNetHello -D sonar.projectVersion=2.0 -D sonar.login=admin -D sonar.password=admin -D sonar.sources="C:\\Users\\610169\\Documents\\sonar-scanner-4.2.0.1873-windows\\bin\\Hello.sln"
		'''
		}
	}
}