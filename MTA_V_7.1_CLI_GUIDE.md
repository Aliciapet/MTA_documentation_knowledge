# CLI Guide
Migration Toolkit for Applications 7.1
Learn how to use the Migration Toolkit for Applications CLI to migrate your applications.
Red Hat Customer Content Services

Abstract
This guide describes how to use the Migration Toolkit for Applications CLI to simplify migration of Java applications.

## Chapter 1. Introduction
1.1. About the CLI Guide 
This guide is for engineers, consultants, and others who want to use the Migration Toolkit for Applications (MTA) to migrate Java applications, .NET applications, or other components. In MTA 7.1 and later, you can use MTA to analyze applications written in languages other than Java. To run analysis on applications written in languages other than Java, add a custom rule set and do not specify a target language. This guide describes how to install and run the CLI, review the generated reports, and take advantage of additional features.

Important
.NET migration is a Developer Preview feature only. Developer Preview features are not supported by Red Hat in any way and are not functionally complete or production-ready. Do not use Developer Preview features for production or business-critical workloads. Developer Preview features provide early access to upcoming product features in advance of their possible inclusion in a Red Hat product offering, enabling customers to test functionality and provide feedback during the development process. These features might not have any documentation, are subject to change or removal at any time, and testing is limited. Red Hat might provide ways to submit feedback on Developer Preview features without an associated SLA.

Important
Analyzing applications written in a language other than Java is a Developer Preview feature only. Developer Preview features are not supported by Red Hat in any way and are not functionally complete or production-ready. Do not use Developer Preview features for production or business-critical workloads. Developer Preview features provide early access to upcoming product features in advance of their possible inclusion in a Red Hat product offering, enabling customers to test functionality and provide feedback during the development process. These features might not have any documentation, are subject to change or removal at any time, and testing is limited. Red Hat might provide ways to submit feedback on Developer Preview features without an associated SLA.

1.2. About the Migration Toolkit for Applications 
What is the Migration Toolkit for Applications?
Migration Toolkit for Applications (MTA) accelerates large-scale application modernization efforts across hybrid cloud environments on Red Hat OpenShift. This solution provides insight throughout the adoption process, at both the portfolio and application levels: inventory, assess, analyze, and manage applications for faster migration to OpenShift via the user interface.

In MTA 7.1 and later, when you add an application to the Application Inventory, MTA automatically creates and executes language and technology discovery tasks. Language discovery identifies the programming languages used in the application. Technology discovery identifies technologies, such as Enterprise Java Beans (EJB), Spring, etc. Then, each task assigns appropriate tags to the application, reducing the time and effort you spend manually tagging the application.

MTA uses an extensive default questionnaire as the basis for assessing your applications, or you can create your own custom questionnaire, enabling you to estimate the difficulty, time, and other resources needed to prepare an application for containerization. You can use the results of an assessment as the basis for discussions between stakeholders to determine which applications are good candidates for containerization, which require significant work first, and which are not suitable for containerization.

MTA analyzes applications by applying one or more rulesets to each application considered to determine which specific lines of that application must be modified before it can be modernized.

MTA examines application artifacts, including project source directories and application archives, and then produces an HTML report highlighting areas needing changes.

How does the Migration Toolkit for Applications simplify migration?
The Migration Toolkit for Applications looks for common resources and known trouble spots when migrating applications. It provides a high-level view of the technologies used by the application.

MTA generates a detailed report evaluating a migration or modernization path. This report can help you to estimate the effort required for large-scale projects and to reduce the work involved.

1.2.1. Supported Migration Toolkit for Applications migration paths 
The Migration Toolkit for Applications (MTA) supports the following migrations:

Migrating from third-party enterprise application servers, such as Oracle WebLogic Server, to JBoss Enterprise Application Platform (JBoss EAP).
Upgrading to the latest release of JBoss EAP.
Migrating from a Windows-only .NET 4.5+ Framework to cross-platform .NET 8.0. (Developer Preview)
MTA provides a comprehensive set of rules to assess the suitability of your applications for containerization and deployment on Red Hat OpenShift Container Platform (RHOCP). You can run an MTA analysis to assess your applications' suitability for migration to multiple target platforms.

1.3. The MTA CLI 
The CLI is a command-line tool in the Migration Toolkit for Applications that you can use to assess and prioritize migration and modernization efforts for applications. It provides numerous reports that highlight the analysis without using the other tools. The CLI includes a wide array of customization options. By using the CLI, you can tune MTA analysis options or integrate with external automation tools.

## Chapter 2. Installing and Running the CLI
2.1. Installing the CLI 
You can install the CLI on Linux, Windows, or macOS operating systems using the downloadable .zip file.

Prerequisites

Red Hat Container Registry Authentication for registry.redhat.io. Red Hat distributes container images from registry.redhat.io, which requires authentication. For more details, see Red Hat Container Registry Authentication.
2.1.1. Installing the CLI .zip file 
Procedure

Navigate to the MTA Download page and download the OS-specific CLI file or the src file:

mta-7.1.1-cli-linux.zip
mta-7.1.1-cli-macos.zip
mta-7.1.1-cli-windows.zip
mta-7.1.1-cli-src.zip
Extract the .zip file to a directory of your choice. The .zip file extracts a single binary, called mta-cli.

When you encounter <MTA_HOME> in this guide, replace it with the actual path to your MTA installation.

2.1.2. Installing the CLI using Podman 
You can install the CLI using podman pull.

Prerequisites

Red Hat Container Registry Authentication for registry.redhat.io. Red Hat distributes container images from registry.redhat.io, which requires authentication. See Red Hat Container Registry Authentication for additional details.
Podman must be installed.
Podman
Podman is a daemonless, open source, Linux-native tool designed to make it easy to find, run, build, share, and deploy applications using Open Containers Initiative (OCI) Containers and Container Images. Podman provides a command-line interface (CLI) familiar to anyone who has used the Docker Container Engine. For more information on installing and using Podman, see Podman installation instructions.

Procedure

Use Podman to authenticate to registry.redhat.io by running the following command:

$ podman login registry.redhat.io
Enter the user name and password:

Username: <username>
Password: <***********>
Copy the binary PATH to enable system-wide use by running the following command:

$ podman cp $(podman create registry.redhat.com/mta-toolkit/mta-mta-cli-rhel9:{ProductVersion}):/usr/local/bin/mta-cli ./
Warning
Although installation using Podman is possible, downloading and installing the .zip file is the preferred installation.

2.1.3. Installing the CLI for use with Docker on Windows (Developer Preview) 
You can install the CLI for use with Docker on Windows. This is the required approach when migrating applications built with .NET framework 4.5 or later on Windows to cross-platform .NET 8.0.

Prerequisites

A host with Windows 11+ 64-bit version 21H2 or higher.
You have download the Docker Desktop for Windows installer. See Install Docker Desktop on Windows for additional details.
Procedure

Open a PowerShell with Administrator privileges.
Ensure Hyper-V is installed and enabled:

PS C:\Users\<your_user_name>> Enable-WindowsOptionalFeature -Online `
   -FeatureName Microsoft-Hyper-V-All
PS C:\Users\<your_user_name>> Enable-WindowsOptionalFeature -Online `
   -FeatureName Containers
Note
You may need to reboot Windows.

Install Docker Desktop on Windows.

Double-click Docker_Desktop_Installer.exe to run the installer. By default, Docker Desktop is installed at C:\Program Files\Docker\Docker.
Deselect the Use WSL 2 instead of Hyper-V option on the Configuration page to ensure that Docker will run Windows containers as the backend instead of Linux containers.
In PowerShell, create a folder for MTA:

PS C:\Users\<your_user_name>> mkdir C:\Users\<your_user_name>\MTA
Replace <your_user_name> with the username for your home directory.

Extract the mta-7.1.1-cli-windows.zip file to the MTA folder:

PS C:\Users\<your_user_name>> cd C:\Users\<your_user_name>\Downloads
Replace <your_user_name> with the username for your home directory.

PS C:\Users\<your_user_name>> Expand-Archive `
   -Path "{ProductShortNameLower}-{ProductVersion}-cli-windows.zip" `
   -DestinationPath "C:\Users\<your_user_name>\MTA"
Replace <your_user_name> with the username for your home directory.

Ensure Docker is running Windows containers:

PS C:\Users\<your_user_name>> docker version
Client:
 Version:           27.0.3
 API version:       1.46
 Go version:        go1.21.11
 Git commit:        7d4bcd8
 Built:             Sat Jun 29 00:03:32 2024
 OS/Arch:           windows/amd64 1
 Context:           desktop-windows
Server: Docker Desktop 4.32.0 (157355)
 Engine:
  Version:          27.0.3
  API version:      1.46 (minimum version 1.24)
  Go version:       go1.21.11
  Git commit:       662f78c
  Built:            Sat Jun 29 00:02:13 2024
  OS/Arch:          windows/amd64 2
  Experimental:     false
1
2
Ensure the OS/Arch setting is windows/amd64.
Set the PODMAN_BIN environment variable to use Docker:

PS C:\Users\<your_user_name>> $env:PODMAN_BIN="C:\Windows\system32\docker.exe"
Set the DOTNET_PROVIDER_IMG environment variable to use the upstream dotnet-external-provider:

PS C:\Users\<your_user_name>> $env:DOTNET_PROVIDER_IMG="quay.io/konveyor/dotnet-external-provider:v0.5.0"
Set the RUNNER_IMG environment variable to use the upstream image:

PS C:\Users\<your_user_name>> $env:RUNNER_IMG="quay.io/konveyor/kantra:v0.5.0"
2.2. Installing MTA on a disconnected environment 
On a connected device, first download and save the MTA binary. Then download and save the Podman images, the MTA CLI image and the provider image that you need.

Download the required MTA CLI binary from the Migration Toolkit for Applications Red Hat Developer page:

CLI for Linux x86_64
CLI for Linux aarch64
CLI for MacOS x86_64
CLI for MacOS aarch64
CLI for Windows x86_64
CLI for Windows aarch64
On a connected device, download and save the images.
Copy the binary to the disconnected device.
In addition, you must save and import the associated container images by using Podman.
2.2.1. Downloading the Podman images 
Prerequisites

Podman installed. For more information, see Podman.
Procedure

Use Podman to authenticate to registry.redhat.io:

$ podman login registry.redhat.io
Enter your username and then your password for registry.redhat.io:

Username: <registry_service_account_username>
Password: <registry_service_account_password>
You should see the following output:

Login Succeeded!
Use Podman to pull the image from the registry:

$ podman pull registry.redhat.io/mta/mta-cli-rhel9:7.1.0
Use Podman to pull the provider image that you need from the registry:

For Java, run:

$ podman pull registry.redhat.io/mta/mta-java-external-provider-rhel9:7.1.0
For .NET, run:

$ podman pull registry.redhat.io/mta/mta-dotnet-external-provider-rhel9:7.1.0
Save the images:

$ podman save <image> -o <my_image.image>
Copy the .image file and the binary onto a USB or directly to the file system of the disconnected device.
On the disconnected device, run

$ podman load --input <my_image.image>
2.2.2. CLI known issues 
Limitations with Podman on Microsoft Windows

The CLI is built and distributed with support for Microsoft Windows.

However, when running any container image based on Red Hat Enterprise Linux 9 (RHEL9) or Universal Base Image 9 (UBI9), the following error can be returned when starting the container:

Fatal glibc error: CPU does not support x86-64-v2
This error is caused because Red Hat Enterprise Linux 9 or Universal Base Image 9 container images must be run on a CPU architecture that supports x86-64-v2.

For more details, see (Running Red Hat Enterprise Linux 9 (RHEL) or Universal Base Image (UBI) 9 container images fail with "Fatal glibc error: CPU does not support x86-64-v2").

CLI runs the container runtime correctly. However, different container runtime configurations are not supported.

Although unsupported, you can run CLI with Docker instead of Podman, which would resolve this issue.

To achieve this, you replace the PODMAN_BIN path with the path to Docker.

For example, if you experience this issue, instead of issuing:

PODMAN_BIN=/usr/local/bin/docker mta-cli analyze
You replace PODMAN_BIN with the path to Docker:

<Docker Root Dir>=/usr/local/bin/docker mta-cli analyze
While this is not supported, it would allow you to explore CLI while you work to upgrade your hardware or move to hardware that supports x86_64-v2.

2.3. Running the CLI 
You can run the Migration Toolkit for Applications (MTA) CLI against one or more applications.

Before MTA 7.1.0, if you wanted to run the CLI against multiple applications, you ran a series of --analyze commands, each against an application, and each generating a separate report. This option, which is still fully supported, is described in Running the MTA CLI against an application.

In MTA 7.1.0 and later, you can run the CLI against multiple applications by using the --bulk option, to generate a single report. This option, which is presented as a Developer Preview, is described in Running the MTA CLI against multiple applications and generating a single report (Developer Preview).

Important
Running the CLI against one or more applications is a Developer Preview feature only. Developer Preview features are not supported by Red Hat in any way and are not functionally complete or production-ready. Do not use Developer Preview features for production or business-critical workloads. Developer Preview features provide early access to upcoming product features in advance of their possible inclusion in a Red Hat product offering, enabling customers to test functionality and provide feedback during the development process. These features might not have any documentation, are subject to change or removal at any time, and testing is limited. Red Hat might provide ways to submit feedback on Developer Preview features without an associated SLA.

2.3.1. Running the MTA CLI against an application 
You can run the Migration Toolkit for Applications (MTA) CLI against an application.

Procedure

Open a terminal and navigate to the <MTA_HOME>/ directory.
Run the mta-cli script, or mta-cli.exe for Windows, and specify the appropriate arguments:

$ ./mta-cli analyze --input <path_to_input> \
    --output <path_to_output> --source <source_name> --target <target_source> \
--input: The application to be evaluated.
--output: The output directory for the generated reports.
--source: The source technology for the application migration. For example, weblogic.
--target: The target technology for the application migration. For example, eap8.
Access the report.
2.3.1.1. MTA command examples 
Running MTA on an application archive
The following command analyzes the example EAR archive named jee-example-app-1.0.0.ear for migrating from JBoss EAP 5 to JBoss EAP 7:

$ <MTA_HOME>/mta-cli analyze \
    --input <path_to_jee-example-app-1.0.0.ear> \
    --output <path_to_report_output> --source eap5 --target eap7 \
Running MTA on source code
The following command analyzes the source code of an example application called customer-management for migrating to JBoss EAP 8.

$ <MTA_HOME>/mta-cli analyze --mode source-only --input <path_to_customer-management> \
    --output <path_to_report_output> --target eap8 --packages org.jboss.eap
Running cloud-readiness rules
The following command analyzes the example EAR archive named jee-example-app-1.0.0.ear for migrating to JBoss EAP 7. It also evaluates the archive for cloud readiness:

$ <MTA_HOME>/mta-cli analyze --input <path_to_jee-example-app-1.0.0.ear> \
    --output <path_to_report_output> \
    --target eap7
2.3.2. Running the MTA CLI against multiple applications and generating a single report (Developer Preview) 
You can now run the Migration Toolkit for Applications (MTA) CLI against multiple applications and generate a combined report. This can save you time and give you a better idea of how to prepare a set of applications for migration.

This feature is currently a Developer Preview feature.

Important
Running the CLI against one or more applications is a Developer Preview feature only. Developer Preview features are not supported by Red Hat in any way and are not functionally complete or production-ready. Do not use Developer Preview features for production or business-critical workloads. Developer Preview features provide early access to upcoming product features in advance of their possible inclusion in a Red Hat product offering, enabling customers to test functionality and provide feedback during the development process. These features might not have any documentation, are subject to change or removal at any time, and testing is limited. Red Hat might provide ways to submit feedback on Developer Preview features without an associated SLA.

Procedure

Open a terminal and navigate to the <MTA_HOME>/ directory.
Run the mta-cli script, or mta-cli.exe for Windows, and specify the appropriate arguments, entering one input per analyze command, but entering the same output directory for all inputs. For example, to analyze applications A, B, and C:

Enter the following command for input A:

$ ./{mta-cli} analyze --bulk --input=<path_to_input_A> --output=<path_to_output_ABC> --source <source_A> --target <target_A>
--input: The application to be evaluated.
--output: The output directory for the generated reports.
--source: The source technology for the application migration. For example, weblogic.
--target: The target technology for the application migration. For example, eap8.
Enter the following command for input B:

$ ./{mta-cli} analyze --bulk --input=<path_to_input_B> --output=<path_to_output_ABC>  --source <source_B> --target <target_B>
Enter the following command for input C:

$ ./{mta-cli} analyze --bulk --input=<path_to_input_C> --output=<path_to_output_ABC>  --source <source_C> --target <target_C>
MTA generates a single report, listing all issues that need to be resolved before the applications can be migrated.

Access the report.
2.3.3. Performing analysis using the command line 
Analyze supports running source code and binary analysis using analyzer-lsp.

To run analysis on application source code, run the following command:


mta-cli analyze --input=<path_to_source_code> --output=<path_to_output_directory>
All flags:

Analyze application source code

Usage:
  mta-cli analyze [flags]

Flags:

      --analyze-known-libraries           Analyze known open-source libraries.
      --context-lines (int)               Number of lines of source code to
                                          include in the output for each
                                          incident (default: `100`).
  -d, --dependency-folders (stringArray)  Directory for dependencies.
      --enable-default-rulesets           Run default rulesets with analysis
                                          (default: `true`).
  -h, --help                              Help for analyze.
      --http-proxy (string)               HTTP proxy string URL.
      --https-proxy (string)              HTTPS proxy string URL.
      --incident-selector (string)        An expression to select incidents
                                          based on custom variables.
                                          Example:
                                          !package=io.demo.config-utils
  -i, --input (string)                    Path to application source code or
                                          a binary.
      --jaeger-endpoint (string)          Jaeger endpoint to collect traces.
      --json-output                       Create analysis and dependency
                                          output as JSON.
      --list-sources                      List rules for available
                                          migration sources.
      --list-targets                      List rules for available
                                          migration targets.
  -l, --label-selector (string)           Run rules based on specified label
                                          selector expression.
      --maven-settings (string)           Path to the custom maven
                                          settings file to use.
      --overwrite                         Overwrite output directory.
      --skip-static-report                Do not generate the static report.
  -m, --mode (string)                     Analysis mode, must be one of
                                          `full` or `source-only`
                                          (default: `full`).
      --no-proxy (string)                 Proxy-excluded URLs
                                          (relevant only with proxy).
  -o, --output (string)                   Path to the directory for analysis
                                          output.
      --overwrite                         Overwrite output directory.
      --rules (stringArray)               Filename or directory containing
                                          rule files.
      --skip-static-report                Do not generate the static report.
  -s, --source (string)                   Source technology to consider
                                          for analysis.
                                          To specify multiple sources,
                                          repeat the parameter:
                                          `--source <source_1>
                                          --source <source_2>` etc.
  -t, --target (string)                   Target technology to consider
                                          for analysis.
                                          To specify multiple targets,
                                          repeat the parameter:
                                          `--target <target_1>
                                          --target <target_2>` etc.


Global Flags:
      --log-level uint32                  Log level (default: 4).
      --no-cleanup                        Do not cleanup temporary resources.
Expand
Note
The list of flags above does not include the --bulk flag because this flag is only offered as part of a Developer Preview feature. That feature is described in Support for providing a single report when analyzing multiple applications on the CLI.

Usage example

Get an example application to run analysis on.
List available target technologies.

mta-cli analyze --list-targets
Run an analysis with a specified target technology, for example cloud-readiness.

mta-cli analyze --input=<path-to/example-applications/example-1> --output=<path-to-output-dir> --target=cloud-readiness
Several analysis reports are created in your specified output path:

$ ls ./output/ -1
analysis.log
dependencies.yaml
dependency.log
output.yaml
static-report
output.yaml is the file that contains the issues report.
static-report contains the static HTML report.
dependencies.yaml contains the dependencies report.
2.3.4. Performing transformation using the command line 
Transform has two subcommands - openrewrite and rules.

Transform application source code or mta XML rules

Usage:
  mta-cli transform [flags]
  mta-cli transform [command]

Available Commands:
  openrewrite Transform application source code using OpenRewrite recipes
  rules       Convert XML rules to YAML

Flags:
  -h, --help   help for transform

Global Flags:
      --log-level uint32   log level (default 4)
      --no-cleanup         do not clean up temporary resources

Use "mta-cli transform [command] --help" for more information about a command.
2.3.4.1. OpenRewrite 
The openrewrite subcommand allows running OpenRewrite recipes on source code.

Transform application source code using OpenRewrite recipes

Usage:
  mta-cli transform openrewrite [flags]

Flags:
  -g, --goal string     target goal (default "dryRun")
  -h, --help            help for openrewrite
  -i, --input string    path to application source code directory
  -l, --list-targets    list all available OpenRewrite recipes
  -s, --maven-settings string   path to a custom maven settings file to use
  -t, --target string   target openrewrite recipe to use. Run --list-targets to get a list of packaged recipes.

Global Flags:
      --log-level uint32   log level (default 4)
      --no-cleanup         do not clean up temporary resources
To run transform openrewrite on application source code, run the following command:


mta-cli transform openrewrite --input=<path/to/source/code> --target=<exactly_one_target_from_the_list>
Note
You can only use a single target to run the transform overwrite command.

2.3.4.2. Rules 
The rules subcommand allows converting mta XML rules to analyzer-lsp YAML rules using windup-shim.

Convert XML rules to YAML

Usage:
  mta-cli transform rules [flags]

Flags:
  -h, --help                help for rules
  -i, --input stringArray   path to XML rule file(s) or directory
  -o, --output string       path to output directory

Global Flags:
      --log-level int   log level (default 5)
To run transform rules on application source code, run the following:


mta-cli transform rules --input=<path/to/xmlrules> --output=<path/to/output/dir>
Usage example

Get an example application to transform source code.
View the available OpenRewrite recipes.

mta-cli transform openrewrite --list-targets
Run a recipe on the example application.

mta-cli transform openrewrite --input=<path-to/jakartaee-duke> --target=jakarta-imports
Inspect the jakartaee-duke application source code diff to see the transformation.

2.4. Accessing reports 
When you run the Migration Toolkit for Applications, a report is generated in the <OUTPUT_REPORT_DIRECTORY> that you specify using the --output argument in the command line.

The output directory contains the following files and subdirectories:

<OUTPUT_REPORT_DIRECTORY>/
├── index.html          // Landing page for the report
├── <EXPORT_FILE>.csv   // Optional export of data in CSV format
├── archives/           // Archives extracted from the application
├── mavenized/          // Optional Maven project structure
├── reports/            // Generated HTML reports
├── stats/              // Performance statistics
Procedure

Obtain the path of the index.html file of your report from the output that appears after you run MTA:

Report created: <OUTPUT_REPORT_DIRECTORY>/index.html
              Access it at this URL: file:///<OUTPUT_REPORT_DIRECTORY>/index.html
Open the index.html file by using a browser.

The generated report is displayed.

2.5. Analyzing multi-language applications with CLI 
You can run the application analysis on applications written in multiple languages. You can perform the analysis either of the following ways:

Select the supported language provider to run the analysis for.
Override the existing supported language provider with your own unsupported language provider and run the analysis for this unsupported provider.
2.5.1. Analyzing a multi-language application for the selected supported language provider 
When analyzing a multi-language application with Migration Toolkit for Applications (MTA) CLI, you can explicitly set a supported language provider according to your application language to run the analysis for.

Prerequisites

You are running the latest version of MTA CLI.
Procedure

List language providers supported for the analysis:

$ mta-cli analyze --list-providers
Run the application analysis for the selected language provider:

$ mta-cli analyze --input <_path_to_the_source_repository_> --output <_path_to_the_output_directory_> --provider <_language_provider_> --rules <_path_to_custom_rules_>
Note that if you do not set the --provider option, the analysis might fail because it detects unsupported providers. The analysis will complete without --provider only if all discovered providers are supported.

2.5.2. Analyzing a multi-language application for an unsupported language provider 
When analyzing a multi-language application with Migration Toolkit for Applications (MTA) CLI, you can run the analysis for an unsupported language provider. To do so, you must override an existing supported language provider with your own unsupported language provider by using the --override-provider-settings option.

Important
You must create a configuration file for your unsupported language provider before overriding the supported provider.

Prerequisites

You created a configuration file for your unsupported language provider.
Procedure

Override an existing supported language provider with your unsupported provider:

$ mta-cli analyze --provider-override <path_to_configuration_file> --output=<path_to_the_output_directory> --rules <path_to_custom_rules>
Note that if you do not set the --provider option, the analysis might fail because it detects unsupported providers. The analysis will complete without --provider only if all discovered providers are supported.

## Chapter 3. Reviewing the reports
The report examples shown in the following sections are a result of analyzing the com.acme and org.apache packages in the jee-example-app-1.0.0.ear example application, which is located in the MTA GitHub source repository.

The report was generated using the following command.

$ <MTA_HOME>/bin/mta-cli --input /home/username/mta-cli-source/test-files/jee-example-app-1.0.0.ear/ --output /home/username/mta-cli-reports/jee-example-app-1.0.0.ear-report --target eap6 --packages com.acme org.apache
Use a browser to open the index.html file located in the report output directory. This opens a landing page that lists the applications that were processed. Each row contains a high-level overview of the story points, number of incidents, and technologies encountered in that application.
3.1. Application report 
3.1.1. Dashboard 
Access this report from the report landing page by clicking on the application name in the Application List.

The dashboard gives an overview of the entire application migration effort. It summarizes:

The incidents and story points by category
The incidents and story points by level of effort of the suggested changes
The incidents by package
3.1.2. Issues report 
Access this report from the dashboard by clicking the Issues link.

This report includes details about every issue that was raised by the selected migration paths. The following information is provided for each issue encountered:

A title to summarize the issue.
The total number of incidents, or times the issue was encountered.
The rule story points to resolve a single instance of the issue.
The estimated level of effort to resolve the issue.
The total story points to resolve every instance encountered. This is calculated by multiplying the number of incidents found by the story points per incident.
Each reported issue may be expanded, by clicking on the title, to obtain additional details. The following information is provided.

A list of files where the incidents occurred, along with the number of incidents within each file. If the file is a Java source file, then clicking the filename will direct you to the corresponding Source report.
A detailed description of the issue. This description outlines the problem, provides any known solutions, and references supporting documentation regarding either the issue or resolution.
A direct link, entitled Show Rule, to the rule that generated the issue.
Issues are sorted into four categories by default. Information on these categories is available at ask Category.

3.1.3. Insights 
Important
Insights is a Technology Preview feature only. Technology Preview features are not supported with Red Hat production service level agreements (SLAs) and might not be functionally complete. Red Hat does not recommend using them in production. These features provide early access to upcoming product features, enabling customers to test functionality and provide feedback during the development process.

For more information about the support scope of Red Hat Technology Preview features, see Technology Preview Features Support Scope.

Previously, a violation generated by a rule with zero effort was listed as an issue in the static report. This is now listed as an insight instead. Issues are generated by general rules, whereas string tags are generated by tagging rules. String tags indicate the presence of a technology but do not show the code location. With the introduction of Insights, you can see the technology used in the application along with its usage in the code.

For example, a rule searching for deprecated API usage in the code that does not impact the current migration but can be tracked and fixed when needed in the future.

Unlike issues, insights do not need to be fixed for a successful migration. They are generated by any rule that doesn’t have a positive effort value and category assigned. They might have a message and tag.

Note
Insights are generated automatically if applicable or present. Currently, MTA supports generating Insights when application anaylsis is done using CLI.
Example: Insights generated by a tagging rule with undefined effort


- customVariables: []
  description: Embedded library - Apache Wicket
  labels:
  - konveyor.io/include=always
  links: []
  ruleID: mvc-01000
  tag:
  - Apache Wicket
  - Embedded library - Apache Wicket
  when:
    builtin.file:
      pattern: .*wicket.*\.jar
Example: Insights generated by a non-tagging rule with zero effort


- category: potential
  customVariables: []
  description: RESTful Web Services @Context annotation has been deprecated
  effort: 0
  message: Future versions of this API will no longer support `@Context` and related
    types such as `ContextResolver`.
  ruleID: jakarta-ws-rs-00001
  when:
    java.referenced:
      location: ANNOTATION
      pattern: jakarta.ws.rs.core.Context
3.1.4. Application details report 
Access this report from the dashboard by clicking the Application Details link.

The report lists the story points, the Java incidents by package, and a count of the occurrences of the technologies found in the application. Next is a display of application messages generated during the migration process. Finally, there is a breakdown of this information for each archive analyzed during the process.

Expand the jee-example-app-1.0.0.ear/jee-example-services.jar to review the story points, Java incidents by package, and a count of the occurrences of the technologies found in this archive. This summary begins with a total of the story points assigned to its migration, followed by a table detailing the changes required for each file in the archive. The report contains the following columns.
Note that if an archive is duplicated several times in an application, it will be listed just once in the report and will be tagged with [Included multiple times].
The story points for archives that are duplicated within an application will be counted only once in the total story point count for that application.

3.1.5. Technologies report 
Access this report from the dashboard by clicking the Technologies link.

The report lists the occurrences of technologies, grouped by function, in the analyzed application. It is an overview of the technologies found in the application, and is designed to assist users in quickly understanding each application’s purpose.

The image below shows the technologies used in the jee-example-app.
3.1.6. Source report 
The Source report displays the migration issues in the context of the source file in which they were discovered.
3.2. Technologies report 
Access this report from the report landing page by clicking the Technologies link.

This report provides an aggregate listing of the technologies used, grouped by function, for the analyzed applications. It shows how the technologies are distributed, and is typically reviewed after analyzing a large number of applications to group the applications and identify patterns. It also shows the size, number of libraries, and story point totals of each application.

Clicking any of the headers, such as Markup, sorts the results in descending order. Selecting the same header again will resort the results in ascending order. The currently selected header is identified in bold, next to a directional arrow, indicating the direction of the sort.
3.3. Selecting packages 
A space-delimited list of the packages to be evaluated by MTA. It is highly recommended to use this argument.

Usage
In most cases, you are interested only in evaluating custom application class packages and not standard Java EE or third party packages. The <PACKAGE_N> argument is a package prefix; all subpackages will be scanned. For example, to scan the packages com.mycustomapp and com.myotherapp, use --packages com.mycustomapp com.myotherapp argument on the command line.
While you can provide package names for standard Java EE third party software such as org.apache, it is usually best not to include them as they should not impact the migration effort.
