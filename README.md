# power-shell
powershell
** 20-07-2022--------powershell
pwd------->working directery
cd---change directery
copy-item
Copy-Item -Path C:\alpha\break.txt -Destination C:\bravo\
get-command---get the all cmdlets

The Get-Help:

cmdlet is the basic help system of Windows PowerShell to retrieve all the information such as syntax, function, alias,

Get-Host---->is used to retrieve information about the version for Windows PowerShell. Here, "Get" is the verb and "Host" acts as the noun.

Windows PowerShell has three types of parameters:

Optional parameter--[]

Mandatory parameter-->

Positional parameter-->(path,destination)

Get-Command -Name Get-Host----->
powershell alias---(shortname)
Alias is an alternate name that can be created for any Windows PowerShell cmdlet.

Windows PowerShell also has predefined built-in aliases for certain cmdlets.

Get-Alias------>

Step 1: get-alias--->The Get-Alias cmdlet displays all the aliases.

Step 2: get-alias -name w* ---->The -Name parameter can be used to retrieve a list of aliases which starts with a specific name. The below example displays all the aliases starting with w or W (case insensitive).

Step 3: get-alias definition get-command-->Definition parameter is used to retrieve the aliases of a particular cmdlet.

New-Alias------->

Step 1: new-alias -name mani -value get-command----> In the below example, an alias named ‘commands’ is created for the Get-Command cmdlet.

Step 2: new-alias -name mani -value get-host----->(error)-->New-Alias cmdlet cannot be used to assign an already existing alias to another cmdlet.

If we try to assign 'commands' to Get-Host cmdlet, it will throw an error as shown below.

Step 3: Aliases created using New-Alias cmdlet are not saved after you exit the Windows PowerShell session.

Set-Alias----->

Step 1: set-alias -name mani -value get-host - get-alias -defination get-host

Using the Set-Alias cmdlet, the alias 'commands' will now be referring to the Get-Host cmdlet instead of Get-Command.

Step 2: set-alias -name host -value get-host - get-alias -defination get-host

The Set-Alias cmdlet will create a new alias named 'Host' for the Get-Host cmdlet if the alias does not already exist.

Step 3: set-alias -name gcm -value get-host-------->(error)

Set-Alias cannot assign an in-built alias to another cmdlet.(gcm is read only constants)

Remove-Item----->Remove-Item alias:/mani

find the second highest number

gps|sort -Property id

gps|sort -Descending id|select -First 2|select -Last 1|select id

gps|sort -property id -descending|select -last 1
gps|sort -property id -descending|select -last 2|select -first 1
pipeline

Get-Help Get-Process---->alias--->gps,ps

get-help get-service---->alias---->gsv

select first 5 and last number of first 5------>gps|select -First 5|select -last 1
gps|select -first 5|sort -property id
gps|select -first 5|sort -property id -descending
get-service|where{$.status -like "Running"}|where{$.name -like "c*"}
-----> -gt

< ------> -lt
= ------> -ge

<= -------> -le
== -------> -eq
** Problem Statement 1: The admin desk has asked John to retrieve the details of the first 3 services running in his local system and export the contents in a csv(comma separated value) format.

Step 1 : Understand how to retrieve the services from the local system.

Get-Service
Step 2 : Filter the results to retrieve only the running services.

Get-Service | Where-Object {$_.Status -eq "Running"}
Step 3 : Selecting only a specific number of services, first 3 services as per our requirement.

Get-Service | Where-Object {$_.Status -eq "Running"} | Select -First 3
Step 4 : Exporting the final result to a file.

Get-Service | Where-Object {$_.Status -eq "Running"} | Select -First 3 | Export-CSV -Path "C:\PowerShell\Requirement1.csv"
21-07-2022
*** sort-object
gps|sort ----->default based on property name
gps|sort -property cpu ---->it is based on cpu asending order
gps|sort -Property cpu -Descending------>it is based on cpu descending order
*** measure-object
gps|Measure-Object
gps|Measure-Object -Property cpu -Sum -Average -Maximum -Minimum
gps|Measure-Object -Property cpu,id -Sum -Average -Maximum -Minimum
gps|Measure-Object -Line -Word -Character -Property name
gps|Measure-Object -Property cpu -Maximum |select -ExpandProperty maximum
gps|Measure-Object -Property cpu -Maximum |select -ExpandProperty count
**** export,import
Get-Content -Path C:\alpha\break.txt ----->get the content
get-process | Export-Csv -Path C:\alpha\break.csv -------->export the csv file format
get-process | Out-File -FilePath C:\alpha\break2.txt -------->convert to txt file format
Get-Content -Path C:\alpha\break.csv --------->get the content of csv file
Import-Csv -Path C:\alpha\break.csv --------> read the data of csv file
**** get-eventlog
problem no - 3
Problem Statement : John has received a csv file named EventLogs containing the details of the application event logs from the admin desk. He has been asked to fetch the source details of the application logs having errors from the csv file into another text file. The source details need to be displayed in 5 columns.
Get-EventLog -List ------------>get the all the information of eventlog

Get-EventLog -LogName Application ------->get the information of application

Step 1 : Importing the contents from the csv file.

Get-EventLog -LogName Application|Export-Csv -Path C:\alpha\eventlog.csv ------------->export the eventlog.csv

Get-EventLog -LogName Application -EntryType Error -------------->entrytype error only

Step 2 : Understand how to retrieve the source details of the application event logs having errors.

Get-EventLog -LogName Application -EntryType Error|select -Property source --------->entrytype error and select only source property

Import-Csv -Path C:\alpha\eventlog.csv|where{$_.Entrytype -eq "Error"} -------->entrytype equal to error

Import-Csv -Path C:\alpha\eventlog.csv|where{$_.Entrytype -like "Err*"} -------> entrytype starting with err

Import-Csv -Path C:\alpha\eventlog.csv|where{$_.Entrytype -clike "Err*"} ------>

Step 3 : Formatting the data to be displayed in 5 columns and redirecting to a text file.

Import-Csv -Path C:\alpha\eventlog.csv|where{$_.Entrytype -eq "Error"}|Format-Wide -Property source -column 5 ----->

Import-Csv -Path C:\alpha\eventlog.csv|where{$_.Entrytype -eq "Error"}|Format-List ------> formated data in list format

Import-Csv -Path C:\alpha\eventlog.csv|where{$_.Entrytype -eq "Error"}|Format-table -------->formated data in table format

Import-Csv -Path C:\alpha\eventlog.csv|where{$_.Entrytype -eq "Error"}|Format-Wide -Property source -Column 5|Out-File -FilePath C:\alpha\eventlog.txt

---------->converted the csv file to txt file

problem no - 4
Problem Statement : John wants to retrieve the contents from the C: drive of his system.
He then creates a new folder named "MyData" in the C: drive to save some confidential information.
After a random check from the CCD, John has been asked to move all personal information to D: drive of his system.
Get-ChildItem ---------->get the information of working directery

Get-ChildItem C:\Users\balla.manikanta -------->get the information of user directery

Get-ChildItem -Recurse C:\Users\balla.manikanta -------->recurse is used to get the information of subfloders

create new directory
New-Item -Path C:\bravo -Name manikanta -type directory --------->create floder

New-Item -Path C:\bravo\manikanta -Name break_file -type file ------->create file

New-Item -Path C:\bravo\manikanta -Name break_file3 -type file -Value "kgfgfgenffdfsjhvhegbb" ----------->add the content on the file

move-item
move-Item -path C:\bravo\manikanta\break_file3 -Destination C:\alpha -------->move the file one floder to another floder
remove-item
** Remove-Item -Path C:\alpha\break_file3
Step 1 : Retrieve contents from a specific directory.
Get-ChildItem -Path C:\
Step 2 : Create a new directory
New-Item -Path C:\ -Name MyData -Type Directory
Step 3 : Move the MyData directory from one location to another.
Move-Item -Path C:\MyData -Destination D:
New-Item -Path C:\bravo -Name ExploreFiles -type file
Remove-Item -Path C:\bravo\ExploreFiles
PROBLEM :1
Problem Statement : The admin desk has asked John to retrieve the details of the first 3 services running in his local system and export the contents in a csv(comma separated value) format.

The below cmdlets will help you retrieve the desired results for each step of our requirement.

Step 1 : Understand how to retrieve the services from the local system.

Get-Service
Step 2 : Filter the results to retrieve only the running services.

Get-Service | Where-Object {$_.Status -eq "Running"}
Step 3 : Selecting only a specific number of services, first 3 services as per our requirement.

Get-Service | Where-Object {$_.Status -eq "Running"} | Select -First 3
Step 4 : Exporting the final result to a file.

Get-Service | Where-Object {$_.Status -eq "Running"} | Select -First 3 | Export-CSV -Path "C:\PowerShell\Requirement1.csv"
Import-Csv C:\For-session\PROBLEM1.CSV|OUT-FILE C:\For-session\PROBLEM1.TXT
ProblemStatement:
John has been asked to retrieve all the services starting with "s" that are currently in stopped state and export it to a text file. [Perform a case sensitive search]
SOL:
GSV|SELECT S*|WHERE {$_.Status -eq "STOPPED"}|EXPORT-CSV C:\For-session\STOPPED.CSV
IMPORT-CSV C:\For-session\STOPPED.CSV|OUT-FILE C:\For-session\STOPPED.TXT
ProblemStatement:
Display the last five sorted output of Get-Service cmdlet on the basis of the "Displayname" field.
SOL:
GSV|SORT|SELECT -Last 5
ProblemStatement:
Identify all the aliases in your system and redirect the output to a text file.
SOL:
GET-ALIAS|Out-File C:\For-session\ALIAS.TXT
Identify all the services that are running in your system whose name starts with “A” (perform case sensitive comparison).
SOL:
GSV A*|WHERE {$_.STATUS -eq "RUNNING"}
Problem Statement : The employees in the organization have reported to the admin desk that they have been facing issues while working with their systems. The systems have slowed down drastically and there is a lot of delay while working with certain applications. The admin desk has asked John to write a PowerShell script to fetch the details of top 5 processes consuming high CPU time. The script should also retrieve details about the the total number of processes running in the system along with the value of highest CPU time consumed.
In order to understand how to retrieve the process details as per the given problem statement, you will be taken through the following steps.

Step 1 : Understand how to retrieve the processes from the local system.

GPS
Step 2 : Sort the results to retrieve only the top 5 processes consuming high CPU.

GPS|SORT -Descending CPU|SELECT -First 5
Step 3 : Fetch the total number processes running in the system and calculate the highest CPU being consumed.

GPS|SORT -Descending CPU|Measure-Object -Property CPU -Maximum|SELECT -First 5
The following pages will help you understand the concepts to implement the above steps for solving the given problem statement.

SOL:
GPS|SORT -Descending CPU|Measure-Object -Property CPU -Maximum|SELECT -First 5
ProblemStatement:
Retrieve the process details(such as ProcessName, ID, VM, CPU) which consumes the maximum amount of virtual memory.
SOL:
GPS|SORT VM -Descending|SELECT -FIRST 1|SELECT NAME,ID,VM,CP
