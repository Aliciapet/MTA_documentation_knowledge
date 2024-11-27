# Introduction to the Migration Toolkit for Applications 
Migration Toolkit for Applications (MTA) is a set of tools that you can use to accelerate large-scale application modernization efforts across hybrid cloud environments on Red Hat OpenShift. MTA looks for common resources and known trouble spots when migrating applications. It provides a high-level view of the technologies used by the application. MTA also generates a detailed report that evaluates a migration or modernization path. By using this report, you can estimate the effort required for large-scale projects and reduce the work involved.

By using the MTA, you can perform the following tasks:

Use the MTA extensive default questionnaire to assess your applications, or create your own custom questionnaire to estimate the difficulty, time, and other resources needed to prepare an application for containerization. You can use the results of an assessment for discussions between stakeholders to determine which applications are suitable for containerization.
Analyze applications by applying one or more sets of rules to each application. You can use these rules to determine which specific lines of the application must be modified before the application can be modernized.
Examine application artifacts, including project source directories and application archives, and produce an HTML report that highlights areas that require changes.

## The MTA features 
Migration Toolkit for Applications (MTA) provides the following features to simplify upgrades with more migration paths:

* New application inventory and assessment modules to assist organizations in managing, classifying, and tagging their applications while assessing application suitability for deployment in containers, including flagging potential risks for migration strategies.

* Full integration with source code and binary repositories to automate the retrieval of applications for analysis along with proxy integration, including HTTP and HTTPS proxy configuration managed in the user interface.

* Improved analysis capabilities with new analysis modes, including source and dependency modes that parse repositories to gather dependencies and add these dependencies to the overall scope of the analysis. You can also use a simplified user experience to configure the analysis scope, including open source libraries.

* Enhanced Role-Based Access Control (RBAC) powered by Red Hat Single Sign-On to define new differentiated personas (administrator, architect, and migrator) with different permissions to suit the needs of each user, including credentials management for multiple credential types.

* Administration perspective to provide tool-wide configuration management for administrators.

* Support for Red Hat OpenShift on AWS (ROSA) is introduced in MTA 7.0.0.

* Support added for Azure Red Hat OpenShift (ARO) is introduced in MTA 7.0.0.

## The MTA rules
The Migration Toolkit for Applications (MTA) contains rule-based migration tools (analyzers) that you can use to analyze the application user interfaces (APIs),technologies, and architectures used by the applications you plan to migrate. MTA analyzer rules use the following rule pattern:
when(condition)
 message(message)
 tag(tags)

You can use the MTA rules internally to perform the following tasks:
* Extract files from archives.
* Decompile files.
* Scan and classify file types.
* Analyze XML and other file content.
* Analyze the application code.
* Build the reports.
MTA builds a data model based on the rule execution results and stores component data and relationships in a graph database. This database can then be queried and updated as required by the migration rules and for reporting purposes.

# The Migration Toolkit for Applications tools
You can use the following Migration Toolkit for Applications (MTA) tools for assistance in the various stages of your migration and modernization efforts:

User interface
Migration Toolkit for Applications Operator
CLI
IDE add-ons for the following applications:

Eclipse
Visual Studio Code, Visual Studio Codespaces, and Eclipse Che
IntelliJ IDEA
Maven plugin

## The MTA Operator
By using the Migration Toolkit for Applications Operator, you can install the user interface on OpenShift Container Platform versions 4.13-4.15.

## The MTA user interface
By using the user interface for the Migration Toolkit for Applications, you can perform the following tasks:

Assess the risks involved in containerizing an application for hybrid cloud environments on Red Hat OpenShift.
Analyze the changes that must be made in the code of an application to containerize the application.

## The MTA CLI
The CLI is a command-line tool in the Migration Toolkit for Applications that you can use to assess and prioritize migration and modernization efforts for applications. It provides numerous reports that highlight the analysis without using the other tools. The CLI includes a wide array of customization options. By using the CLI, you can tune MTA analysis options or integrate with external automation tools.

## The MTA IDE add-ons
You can migrate and modernize applications by using the Migration Toolkit for Applications (MTA) add-ons for the following applications:

Eclipse
Visual Studio Code, Visual Studio Codespaces, and Eclipse Che
IntelliJ IDEA, both the Community and Ultimate versions
You can use these add-on to perform the following tasks:

Analyze your projects by using customizable sets of rules.
Mark issues in the source code.
Fix the issues by using the provided guidance.
Use the automatic code replacement, if possible.
