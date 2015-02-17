# OCELot.GuideLines
A set of guidelines for developing new modules for OCELot. For the latest version, please visit [https://github.com/entelect/OCELot.GuideLines](https://github.com/entelect/OCELot.GuideLines)

Your libray module should be prefixed with OCELot. so for example, if you are making a module that will be used for error handeling and you want to name it ErrorHandling, your module will then be OCELot.ErrorHandling . To make things easier to set up in team city, we will from now on, be refering to this module name as *OCELot.%system.teamcity.projectName%*
## GitHub Repository ##
- When creating the repo, it should belong to the entelect Organisation.
- The repo name should follow the same naming convention as set out in the above section. I.e. : *OCELot.%system.teamcity.projectName%*

## General Folder Srtructure ##
	Ocelot.%system.teamcity.projectName%
	├── [.git]
	├── [packages]
	|   └── repositories.config
	├── [src]
	|   └── OCELot.%system.teamcity.projectName%\
	├── [tests]
	|   └── OCELot.%system.teamcity.projectName%.Tests\
	├── .gitignore
	├── LICENCE
	├── OCELot.%system.teamcity.projectName%.sln
	├── _site
	└── README.md

## Setting up your solution ##
1. Do not enable NuGet package restore, see http://blog.davidebbo.com/2014/01/the-right-way-to-restore-nuget-packages.html
2. Set Treat Warings as Erors to all for all code projects
3. Enable XML documentation output for all code projects
4. Enable Code Analysis on Build for all code projects

## Setting up Automated Builds ##
Team City is reccomended and documented here, but the concepts can be carried over to other providers. There is a CI template that should be used for all new modules..

**VCS Roots**: One for all the develpment branches & one for master, be sure to exclude gh-pages branch as thisi is used by github to host documentation about your project.

**CI Build steps** - I have included instructions for those that are copying the existing config (Ocelot.ErrorHandling):

Images have been provided in the git repository: [https://github.com/entelect/OCELot.GuideLines/tree/master/AutomatedBuilds/Continuous%20Integration](https://github.com/entelect/OCELot.GuideLines/tree/master/AutomatedBuilds/Continuous%20Integration)

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
	> %teamcity.tool.dotCover%\dotCover.exe analyse /TargetExecutable="%env.ProgramFiles(x86)%\NUnit 2.6.2\bin\nunit-console.exe" /TargetWorkingDir=%teamcity.build.checkoutDir%\tests\OCELot.%system.teamcity.projectName%.Tests\bin\Debug\ /TargetArguments=OCELot.%system.teamcity.projectName%.Tests.dll /Output=%teamcity.build.checkoutDir%\dotCover\dotCover.html /ReportType=HTML

5. Publish to SonarCube
	- There shouldn't be any extra options to set here if you are copying the existing config 
6. Find Duplicates
	- There shouldn't be any extra options to set here if you are copying the existing config 
7. Inspections (.NET)
	- Set "Solution file path"
	- Set project names to check in "Projects filter"

**Build Template Parmeters**

- %MajorVersion% - The major version number for this project
- %MinorVersion% - The minor version number for this project

## Code Coverage ##

## Documentation ##

## Testing ##