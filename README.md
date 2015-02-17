# OCELot.GuideLines
A set of guidelines for developing new modules for OCELot

## Folder Srtructure ##
	├── [.git]
	├── [packages]
	|   └── repositories.config
	├── [src]
	├── [tests]
	├── .gitignore
	├── LICENCE
	├── <ProjectName>.sln
	├── _site
	└── README.md

## Setting up your solution ##
1. Do not enable NuGet package restore, see http://blog.davidebbo.com/2014/01/the-right-way-to-restore-nuget-packages.html
2. Set Treat Warings as Erors to all for all code projects
3. Enable XML documentation output for all code projects
4. Enable Code Analysis on Build for all code projects

## Setting up Automated Builds ##
Team City is reccomended and documented here, but the concepts can be carried over to other providers.

There should be a template soon which can be used for new projects.

VCS Roots: One for all the dev branches & one for master, be sure to exclude gh-pages branch as thisi is used by github to host documentation about your project.

CI Build steps - I have included instructions for those that are copying the existing config (Ocelot.ErrorHandling):

1. Restore Nuget Packages
	- Set "Path to Solution"
2. Build using Visual Studio(SLN) in Debug mode with Rebuild Target
	- Set "Solution file path"
	- We build in debug mode so that the .pdb files, debug symbols and unoptimised code is there, this allows us to do acurate Code Coverage
3. Run Tests (NUnit), make sure to exclude the tests folder itself. Using JetBrains dotCover as the coverage tool, this will allows us to access the code coverage from the TeamCity web ui
	- There shouldn't be any extra options to set here if you are copying the existing config 
4. Run DotCover manually to generate the artifact to publish to SonarCube
	- For now you will have to customise the script unill we can find an easier way
	- Script:
	> %teamcity.tool.dotCover%\dotCover.exe analyse /TargetExecutable="%env.ProgramFiles(x86)%\NUnit 2.6.2\bin\nunit-console.exe" /TargetWorkingDir=%teamcity.build.checkoutDir%\tests\OCELot.ErrorHandling.Tests\bin\Debug\ /TargetArguments=OCELot.ErrorHandling.Tests.dll /Output=%teamcity.build.checkoutDir%\dotCover\dotCover.html /ReportType=HTML

5. Publish to SonarCube
	- There shouldn't be any extra options to set here if you are copying the existing config 
6. Find Duplicates
	- There shouldn't be any extra options to set here if you are copying the existing config 
7. Inspections (.NET)
	- Set "Solution file path"
	- Set project names to check in "Projects filter"

## Code Coverage ##

## Documentation ##

## Testing ##