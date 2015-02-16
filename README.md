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

## Setting up CI ##
Team City is reccomended and documented here, but the concepts can be carried over to other providers.

VCS Roots: One for all the dev branches & one for master, be sure to exclude gh-pages branch as thisi is used by github to host documentation about your project.

CI Build steps:

1. Restore Nuget Packages
2. Build using Visual Studio(SLN) in release mode with Rebuild Target
3. Run Tests (NUnit), make sure to exclude the tests folder itself. Use JetBrains dotCover as the coverage tool
4. Run DotCover manually to generate the artifact to publish to SonarCube
5. Publish to SonarCube

## Code Coverage ##

## Documentation ##

## Testing ##