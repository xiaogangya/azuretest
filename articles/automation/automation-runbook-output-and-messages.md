<properties 
   pageTitle="Runbook Output and Messages in Azure Automation | Microsoft Azure"
   description="Desribes how to create and retrieve output and error messages from runbooks in Azure Automation."
   services="automation"
   documentationCenter=""
   authors="bwren"
   manager="stevenka"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/17/2015"
   ms.author="bwren" />

# Runbook output and messages in Azure Automation

Most Azure Automation runbooks will have some form of output such as an error message to the user or a complex object intended to be consumed by another workflow. Windows PowerShell provides [multiple streams](http://blogs.technet.com/heyscriptingguy/archive/2014/03/30/understanding-streams-redirection-and-write-host-in-powershell.aspx) to send output from a script or workflow. Azure Automation works with each of these streams differently, and you should follow best practices for how to use each when you are creating a runbook.

The following table provides a brief description of each of the streams and their behavior in the Azure Management Portal both when running a published runbook and when [testing a runbook](http://msdn.microsoft.com/library/azure/dn879147.aspx). Further details on each stream are provided in subsequent sections.

| Stream | Description | Published | Test|
|:---|:---|:---|:---|
|Output|Objects intended to be consumed by other runbooks.|Written to the job history.|Displayed in the Test Output Pane.|
|Warning|Warning message intended for the user.|Written to the job history.|Displayed in the Test Output Pane.|
|Error|Error message intended for the user. Unlike an exception, the runbook continues after an error message by default.|Written to the job history.|Displayed in the Test Output Pane.|
|Verbose|Messages providing general or debugging information.|Written to job history only if verbose logging is turned on for the runbook.|Displayed in the Test Output pane only if $VerbosePreference is set to Continue in the runbook.|
|Progress|Records automatically generated before and after each activity in the runbook. The runbook should not attempt to create its own progress records since they are intended for an interactive user.|Written to job history only if progress logging is turned on for the runbook.|Not displayed in the Test Output Pane.|
|Debug|Messages intended for an interactive user. Should not be used in runbooks.|Not written to job history.|Not written to Test Output Pane.|

## Output stream

The Output stream is intended for output of objects created by a script or workflow when it runs correctly. In Azure Automation, this stream is primarily used for objects intended to be consumed by [parent runbooks that call the current runbook](automation-child-runbooks.md). When you [call a runbook inline](automation-child-runbooks.md/#InlineExecution) from a parent runbook, it returns data from the output stream to the parent. You should only use the output stream to communicate general information back to the user if you know the runbook will never be called by another runbook. As a best practice, however, you should typically use the [Verbose Stream](#Verbose) to communicate general information to the user.

You can write data to the output stream using [Write-Output](http://technet.microsoft.com/library/hh849921.aspx) or by putting the object on its own line in the runbook.

	#The following lines both write an object to the output stream.
	Write-Output –InputObject $object
	$object

### Output from a function

When you write to the output stream in a function that is included in your runbook, the output is passed back to the runbook. If the runbook assigns that output to a variable, then it is not written to the output stream. Writing to any other streams from within the function will write to the corresponding stream for the runbook.

Consider the following sample runbook.

	Workflow Test-Runbook
	{
	   Write-Verbose "Verbose outside of function"
	   Write-Output "Output outside of function"
	   $functionOutput = Test-Function
	
	   Function Test-Function
	   {
	      Write-Verbose "Verbose inside of function"
	      Write-Output "Output inside of function"
	   }
	}

The output stream for the runbook job would be:

	Output outside of function

The verbose stream for the runbook job would be:

	Verbose outside of function
	Verbose inside of function

### Declaring output data type

A workflow can specify the data type of its output using the [OutputType attribute](http://technet.microsoft.com/library/hh847785.aspx). This attribute has no effect during runtime, but it provides an indication to the runbook author at design time of the expected output of the runbook. As the toolset for runbooks continues to evolve, the importance of declaring output data types at design time will increase in importance. As a result, it is a best practice to include this declaration in any runbooks that you create.

The following sample runbook outputs a string object and includes a declaration of its output type. If your runbook outputs an array of a certain type, then you should still specify the type as opposed to an array of the type.

	Workflow Test-Runbook
	{
	   [OutputType([string])]
	
	   $output = "This is some string output."
	   Write-Output $output
	}

## Message streams

Unlike the output stream, message streams are intended to communicate information to the user. There are multiple message streams for different kinds of information, and each is handled differently by Azure Automation.

### Warning and error streams

The Warning and Error streams are intended to log problems that occur in a runbook. They are written to the job history when a runbook is executed, and are included in the Test Output Pane in the Azure Management Portal when a runbook is tested. By default, the runbook will continue executing after a warning or error. You can specify that the runbook should be suspended on a warning or error by setting a [preference variable](#PreferenceVariables) in the runbook before creating the message. For example, to cause a runbook to suspend on an error as it would an exception, set **$ErrorActionPreference** to Stop.

Create a warning or error message using the [Write-Warning](https://technet.microsoft.com/library/hh849931.aspx) or [Write-Error](http://technet.microsoft.com/library/hh849962.aspx) cmdlet. Activities may also write to these streams.

	#The following lines create a warning message and then an error message that will suspend the runbook.
	
	$ErrorActionPreference = "Stop"
	Write-Warning –Message "This is a warning message."
	Write-Error –Message "This is an error message that will stop the runbook because of the preference variable."

### Verbose stream

The Verbose message stream is for general information about the runbook operation. Since the [Debug Stream](#Debug) is not available in a runbook, verbose messages should be used for debug information. By default, verbose messages from published runbooks will not be stored in the job history. To store verbose messages, configure published runbooks to Log Verbose Records on the Configure tab of the runbook in the Azure Management Portal. In most cases, you should keep the default setting of not logging verbose records for a runbook for performance reasons. Turn on this option only to troubleshoot or debug a runbook.

When [testing a runbook](http://msdn.microsoft.com/library/azure/dn879147.aspx), verbose messages are not displayed even if the runbook is configured to log verbose records. To display verbose messages while [testing a runbook](http://msdn.microsoft.com/library/azure/dn879147.aspx), you must set the $VerbosePreference variable to Continue. With that variable set, verbose messages will be displayed in the Test Output Pane of the Azure Management Portal.

Create a verbose message using the [Write-Verbose](http://technet.microsoft.com/library/hh849951.aspx) cmdlet.

	#The following line creates a verbose message.
	
	Write-Verbose –Message "This is a verbose message."

### Debug stream

The Debug stream is intended for use with an interactive user and should not be used in runbooks.

## Progress records

If you configure a runbook to log progress records (on the Configure tab of the runbook in the Azure Management Portal), then a record will be written to the job history before and after each activity is run. In most cases, you should keep the default setting of not logging progress records for a runbook in order to maximize performance. Turn on this option only to troubleshoot or debug a runbook. When testing a runbook, progress messages are not displayed even if the runbook is configured to log progress records.

The [Write-Progress](http://technet.microsoft.com/library/hh849902.aspx) cmdlet is not valid in a runbook, since this is intended for use with an interactive user.

## Preference variables

Windows PowerShell uses [preference variables](http://technet.microsoft.com/library/hh847796.aspx) to determine how to respond to data sent to different output streams. You can set these variables in a runbook to control how it will respond to data sent into different streams.

The following table lists the preference variables that can be used in runbooks with their valid and default values. Note that this table only includes the values that are valid in a runbook. Additional values are valid for the preference variables when used in Windows PowerShell outside of Azure Automation.

| Variable| Default Value| Valid Values|
|:---|:---|:---|
|WarningPreference|Continue|Stop<br>Continue<br>SilentlyContinue|
|ErrorActionPreference|Continue|Stop<br>Continue<br>SilentlyContinue|
|VerbosePreference|SilentlyContinue|Stop<br>Continue<br>SilentlyContinue|

The following table lists the behavior for the preference variable values that are valid in runbooks.

| Value| Behavior|
|:---|:---|
|Continue|Logs the message and continues executing the runbook.|
|SilentlyContinue|Continues executing the runbook without logging the message. This has the effect of ignoring the message.|
|Stop|Logs the message and suspends the runbook.|

## Retrieving runbook output and messages

### Azure management portal

You can view the details of a runbook job in the Azure Management Portal from the Jobs tab of a runbook. The Summary of the job will display the input parameters and the [Output Stream](#Output) in addition to general information about the job and any exceptions if they occurred. The History will include messages from the [Output Stream](#Output) and [Warning and Error Streams](#WarningError) in addition to the [Verbose Stream](#Verbose) and [Progress Records](#Progress) if the runbook is configured to log verbose and progress records.

### Windows PowerShell

In Windows PowerShell, you can retrieve output and messages from a runbook using the [Get-AzureAutomationJobOutput](http://msdn.microsoft.com/library/dn690268.aspx) cmdlet. This cmdlet requires the ID of the job and has a parameter called Stream where you specify which stream to return. You can specify Any to return all streams for the job.

The following example starts a sample runbook and then waits for it to complete. Once completed, its output stream is collected from the job.

	$job = Start-AzureAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" 
	
	$doLoop = $true
	While ($doLoop) {
	   $job = Get-AzureAutomationJob –AutomationAccountName "MyAutomationAccount" -Id $job.Id
	   $status = $job.Status
	   $doLoop = (($status -ne "Completed") -and ($status -ne "Failed") -and ($status -ne "Suspended") -and ($status -ne "Stopped") 
	}
	
	Get-AzureAutomationJobOutput –AutomationAccountName "MyAutomationAccount" -Id $job.Id –Stream Output

## Related articles

- [Track a runbook job](automation-runbook-execution.md)
- [Child Runbooks](http://msdn.microsoft.com/library/azure/dn857355.aspx)
test
