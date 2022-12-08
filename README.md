# Pipeline Automation

Contains shared YAML templates and any other scripts and artifacts for CI/CD pipeline automation.

The [schemas/geico-cicd-schema.json](schemas/geico-cicd-schema.json) file is for use by the
[Azure Pipelines Extension](https://marketplace.visualstudio.com/items?itemName=ms-azure-devops.azure-pipelines)
and can be downloaded from the `geico` Azure DevOps organization as per instructions if it
needs to be updated.

The templates provide various building blocks for Azure Pipeline steps, jobs and stages. The top-level
templates are documented below.

## .NET CI/CD

The [dotnet-ci.yml](templates\dotnet-ci.yml) includes .NET restore, build and test steps and default
parameters that are suited to most projects. A sample pipeline that consumes it can be as simple as:

    name: '$(BuildDefinitionName).$(Date:yyyyMMdd)$(Rev:.r)-$(Build.SourceBranchName)'

    resources:
    repositories:
    - repository: PipelineAutomation
        type: git
        name: Apollo/PipelineAutomation

    extends:
    template: templates/dotnet-ci.yml@PipelineAutomation

See the YAML file for details on parameters you can override.

## .NET Container Image Build and Push

The [aks-dotnet-image-build-stages.yml](templates\aks-dotnet-image-build-stages.yml) pipeline is
for building a .NET project, including it in a container image and pushing it to
Azure Container Registry as per GEICO Cloud 1.0 conventions and automation.

Sample usage:

    name: '$(BuildDefinitionName).$(Date:yyyyMMdd)$(Rev:.r)-$(Build.SourceBranchName)'

    trigger:
      branches:
        include:
        - master
        - release/*

    resources:
      repositories:
      - repository: PipelineAutomation
        type: git
        name: Apollo/PipelineAutomation

    stages:
    - template: templates/aks-dotnet-image-build-stages.yml@PipelineAutomation
      parameters:
        projectSourceFolder: src/MyWebApi
        veracodeProject: GEI1234 - My Web API

See the YAML file for details on parameters you can override.

## NuGet Packages

Coming soon... once requirements have been ironed out.
