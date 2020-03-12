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
		def scannerhome = tool 'Sonar'
		withSonarQubeEnv('SonarQube') { 
		
		powershell '''cd "scannerhome"'''
		}
	}
}