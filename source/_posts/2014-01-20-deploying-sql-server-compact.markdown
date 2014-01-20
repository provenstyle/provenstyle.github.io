---
layout: post
title: "Deploying SQL Server Compact"
date: 2014-01-20 17:46
comments: true
categories: sqlce
---
I wrote an application that stores information in a SQL Server Compact database.  It worked on my machine, but when QA installed it they received the following error: 

<pre>
Exception occured while configuring the databaseSystem.ArgumentException: 
Unable to find the requested .Net Framework Data Provider.  It may not be installed.
 	at System.Data.Entity.Infrastructure.SqlCeConnectionFactory.CreateConnection(String nameOrConnectionString)
	at System.Data.Entity.Internal.LazyInternalConnection.Initialize()
   	at System.Data.Entity.Internal.LazyInternalConnection.get_ProviderName()
   	at System.Data.Entity.Internal.LazyInternalContext.InitializeContext()
   	at System.Data.Entity.Internal.InternalContext.CreateObjectContextForDdlOps()
   	at System.Data.Entity.Database.Exists()
</pre>

I had added references to the following dll's and they were in my bin directory:

* System.Data.SqlServerCe.dll
* System.Data.SqlServerCe.Entity.dll
 
but SQL Server Compact has other dependencies of which I was not aware.  Namely SQL Server Compact needs its own native components.  These can either come from the GAC or from a local copy installed in the applications file path.

##Reproducing the issue
I had installed SQL Server Compact 4.0 on my machine several months back and didn't realize these libraries were installed in the Gac on my machine.  To reproduce the issue all I had to do was uninstalled SQL Server Compact 4.0.

##Possible Solutions
1. Install SQL Server Compact on the target machine for my application.

2. Include a local copy of the SQL Server Compact native components with my application.

I chose option two because the whole idea of using SQLCE on this project was so that the users did not have to install a database.  
  

##First I had to get the Microsoft SQL Server Compact files
I installed Microsoft SQL Server Compact 4 on my machine again and in the `C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private` folder are all the files I needed to deploy SqlCE with my application. After I got the files I needed I uninstalled SqlCE again.


##Next I had to add a post build event 

This copies two processor specific folders into the bin directory 

<pre>
if not exist "$(TargetDir)x86" md "$(TargetDir)x86"
xcopy /s /y "$(SolutionDir)..\Lib\Microsoft\SqlServerCe\x86\*.*" "$(TargetDir)x86"

if not exist "$(TargetDir)amd64" md "$(TargetDir)amd64"
xcopy /s /y "$(SolutionDir)..\Lib\Microsoft\SqlServerCe\amd64\*.*" "$(TargetDir)amd64"

</pre>

##Then I had to configure my Data Provider

	<system.data>
		<DbProviderFactories>
			<remove invariant="System.Data.SqlServerCe.4.0" />
			<add name="Microsoft SQL Server Compact Data Provider 4.0" invariant="System.Data.SqlServerCe.4.0" description=".NET Framework Data Provider for Microsoft SQL Server Compact" type="System.Data.SqlServerCe.SqlCeProviderFactory, System.Data.SqlServerCe, Version=4.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91" />
		</DbProviderFactories>
	</system.data>

##Lastly I needed an assembly redirect

Here is the error I encountered.

<pre>
Could not load file or assembly 'System.Data.SqlServerCe, Version=4.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference. (Exception from HRESULT: 0x80131040)
</pre>

### Assembly Redirect

	<dependentAssembly>
		<assemblyIdentity name="System.Data.SqlServerCe" publicKeyToken="89845dcd8080cc91" culture="neutral" />
		<bindingRedirect oldVersion="0.0.0.0-4.0.0.1" newVersion="4.0.0.1" />
	</dependentAssembly>

## Also noteworthy

I had already configured Entity Frame work to use SqlCE

	<configSections>
		<section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=5.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false" />
	</configSections>

	<connectionStrings>
		<add name="lvsData" connectionString="Data Source=C:\temp\appData.sdf;Max Database Size=4000;" providerName="System.Data.SqlServerCe.4.0" />
	</connectionStrings>

	<runtime>    
		<assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
			<dependentAssembly>
				<assemblyIdentity name="EntityFramework" publicKeyToken="b77a5c561934e089" culture="neutral" />
				<bindingRedirect oldVersion="0.0.0.0-5.0.0.0" newVersion="5.0.0.0" />
			</dependentAssembly>      
		</assemblyBinding>
	</runtime>

	<entityFramework>
		<defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework">
			<parameters>
				<parameter value="System.Data.SqlServerCe.4.0" />
			</parameters>
		</defaultConnectionFactory>
	</entityFramework>


   
