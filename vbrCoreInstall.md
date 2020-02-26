# Veeam Backup and Replication version 10 install on Win Core

- Using win 2016 core
- sql 2017 express
- sql clr types 2014
- shared management objects 2014
- report viewer 2015
- .Net 4.7.2

## .NET 4.7.2

- Download offline installer if exists
- Just install by running the .msi

## SQL items

### SQL Server 2017 Express

- Download the offline installer files if not connected to the network
- Silent install directions as follows

__in the 'SQLEXPR_x64_ENU' folder:__

.\SETUP.EXE /QS /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /INSTANCENAME=MSSQLSERVER /INSTANCEID=MSSQLSERVER /FEATURES=SQL,AS,IS,Tools /SQLSVCACCOUNT="jfasqlw2k16\administrator" /SQLSVCPASSWORD="[password]" /SQLSYSADMINACCOUNTS="jfasqlw2k16\administrator" /AGTSVCACCOUNT="jfasqlw2k16\administrator" /AGTSVCPASSWORD="[password]" /ASSVCACCOUNT="jfasqlw2k16\administrator" /ASSVCPASSWORD="[password]" /ISSVCAccount="jfasqlw2k16\administrator" /ISSVCPASSWORD="[password]" /ASSYSADMINACCOUNTS="jfasqlw2k16\administrator"

### SQL CLR Types 2014

- Download this version
- just install it by running the .msi or .exe

### SQL Shared Management Objects 2014

- Download this version
- just install it by running the .msi or .exe

### ReportViewer

- Download the appropriate version
- install via .exe or .msi

### Enabling TCP and NamedPipes for the instance

__Install MS ODBC DRIVER 17 for Sql Server__

Install both the driver and the SDK

__Adding the Powershell Module__

Import the sqlps module by entering

  Import-Module "sqlps"

__Enabling TCP and Np__

  $smo = 'Microsoft.SqlServer.Management.Smo.'  
  $wmi = new-object ($smo + 'Wmi.ManagedComputer').  
  
  \# List the object properties, including the instance names.  
  $Wmi  
  
  \# Enable the TCP protocol on the default instance.  
  $uri = "ManagedComputer[@Name='<computer_name>']/ ServerInstance[@Name='MSSQLSERVER']/ServerProtocol[@Name='Tcp']"  
  $Tcp = $wmi.GetSmoObject($uri)  
  $Tcp.IsEnabled = $true  
  $Tcp.Alter()  
  $Tcp  
  
  \# Enable the named pipes protocol for the default instance.  
  $uri = "ManagedComputer[@Name='<computer_name>']/ ServerInstance[@Name='MSSQLSERVER']/ServerProtocol[@Name='Np']"  
  $Np = $wmi.GetSmoObject($uri)  
  $Np.IsEnabled = $true  
  $Np.Alter()  
  $Np  

__restarting the instance__

  \# Get a reference to the ManagedComputer class.  
  CD SQLSERVER:\SQL\<computer_name>  
  $Wmi = (get-item .).ManagedComputer  
  \# Get a reference to the default instance of the Database Engine.  
  $DfltInstance = $Wmi.Services['MSSQLSERVER']  
  \# Display the state of the service.  
  $DfltInstance  
  \# Stop the service.  
  $DfltInstance.Stop();  
  \# Wait until the service has time to stop.  
  \# Refresh the cache.  
  $DfltInstance.Refresh();   
  \# Display the state of the service.  
  $DfltInstance  
  \# Start the service again.  
  $DfltInstance.Start();  
  \# Wait until the service has time to start.  
  \# Refresh the cache and display the state of the service.  
  $DfltInstance.Refresh(); $DfltInstance

