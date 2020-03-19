# Using Talend with MySQL
Talend is one of the fast-growing Data Integration solution to enable deep analytics and data integration requirements. 
In this lab, we will use Talend Open Studio to work with MySQL

## Prequisites
You must have JDK 1.8 installed, download and install from https://www.oracle.com/java/technologies/javase-jdk8-downloads.html
Simply create a batch file to specify the JDK 1.8 VM, for example, talend.bat
```
TOS_DI-win-x86_64.exe -vm "C:\Program Files\Java\jdk1.8.0_241\bin"
```

## Install Talend Open Studio
Download and install Telend Open Studio on Windows from https://www.talend.com/products/talend-open-studio/.
Talend Open Studio (TOS) is an Eclipse-based tools. 

### Install additional plugins
Once you have installed TOS, run TOS

![Install](img/T1.png)

You will be prompted to install additional features/plugins. Go ahead to install the **Required third-party libraries**. Optionally, you can install the **Optional third-party libraries** (It will take quite a bit of time to install both the third-party libraries)

![Features](img/T7.png)

### Fix compatibilities issues
If you are running Talend on Windows platform, some of the display panels don't display properly and you can't resize the panels.
You can run "Troubleshoot Compatibility" on the Talend program to have Windows fix the display issue.
![Fix](img/T12.png)
![Fix2](img/T13.png)

## Create a JSON data set
Create a JSON data set by creating a new Metadata object, **File Json**
1. Specify a new of the Json, says **accounts**
![J1](img/T15.png)

2. Specify **Input Json**
![J2](img/T16.png)

3. Specify the JSON files, in this case, **accounts_noindex.json**
![J3](img/T17.png)

4. Specify the fields to be extracted. First, select **account_number** and drag it to the **Path Loop Expression** panel. 
Next select all the remaining fields, and drag them to the **Fields to extract** panel. Click on **Refresh Preview**
![J4](img/T18.png)

5. Review the last panel and click on **Finish**
![J5](img/T19.png)

## Create a DB Connection to MySQL
Create a DB Connection object to MySQL
1. 




