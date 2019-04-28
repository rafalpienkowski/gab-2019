# Global Azure Bootcamp 2019
Repository dedicated for the workshop that took place during Global Azure Bootcamp 2019. Workshop's goal was the introduction to Azure DevOps.
The presentation which has been used during the workshop you can find [here](https://github.com/rafalpienkowski/presentations/blob/master/azure-devops/azure_dev_ops.pdf).

# Azure DevOps Build pipeline
The working pipeline for simple .Net Core MVC application could look like this:

```yaml
# ASP.NET Core
# Build and test ASP.NET Core projects targeting .NET Core.
# Add steps that run tests, create a NuGet package, deploy, and more:
# https://docs.microsoft.com/azure/devops/pipelines/languages/dotnet-core

trigger:
- master

pool:
  vmImage: 'Ubuntu-16.04'

variables:
  buildConfiguration: 'Release'

steps:
- task: DotNetCoreCLI@2
  displayName: 'Build application'
  inputs:
    command: 'build'
    projects: 'src/GAB/GAB.csproj'
    arguments: --configuration $(buildConfiguration)

- task: DotNetCoreCLI@2
  displayName: 'Unit tests'
  inputs:
    command: 'test'
    projects: '**/*Tests/*.csproj'
    arguments: '--configuration $(buildConfiguration)'

- task: DotNetCoreCLI@2
  displayName: 'Publishing project'
  inputs:
    command: 'publish'
    projects: 'src/GAB/GAB.csproj'
    publishWebProjects: True
    arguments: '--configuration $(buildConfiguration) --output $(Build.ArtifactStagingDirectory)'
    zipAfterPublish: True

- task: PublishBuildArtifacts@1
  displayName: 'Publishing artifacts'
```

# Contributing
If you found any typos, bugs or have any suggestion feel free to make a PR or contact with me.

# License
This project is licensed under the MIT License 