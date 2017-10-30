# Getting Started: Configuring Your Development Environment and Database

> **Important**: Prior to getting starting with the practices and labs, you should first complete this initial practice. The database you create in this practice will be used for subsequent work.

 Install Visual Studio 2017 Community Edition

We will use Visual Studio 2017 Community Edition to complete the various labs in this course. In this task, we will install the required workloads as part of the Visual Studio installation process.

1. In your browser, navigate to the Visual Studio Download web page: <https://www.visualstudio.com/downloads/>.
1. On the web page, locate the **Visual Studio 2017 Community** option and then click the **Free download** button.
1. In your browser's download dialog box, select the option to **Execute (or Run)** the downloaded file.
1. The **Visual Studio Installer** will first prompt you to view and then accept the **License Terms**. Click the **Continue** button after you have reviewed the terms.
1. The **Visual Studio Installer** will then display a list of **workloads, individual components, and language packs** that can be installed to the local machine. 
1. Click the **Workloads** tab. Select the following additional workloads:
	- .NET Desktop Development
	- Universal Windows Platform development
	- ASP.NET and Web Development
	- Data Storage and Processing
	- .NET Core Cross-Platform Development
1. Click the **Individual Components** tab. Select the following additional individual components:
	- .NET Framework 4.6.2 SDK
	- .NET Framework 4.6.2 targeting pack
1. Once you have selected the **workloads** and **individual components** to install, click the **Install** button.
1. Wait for the installer to complete the installation of the workloads to Visual Studio Community 2017.
	> **Note**: The minimal components can take more than 10 GBs of storage space on your local machine and will take anywhere from ten to thirty minutes to install.
1. Once the installation is complete, click the **Launch** button to start Visual Studio 2017 for the first time.
1. You will see a prompt indicating that you can sign in to Visual Studio using a Microsoft account to save your developer settings and connect to cloud-based services. This step is optional. You can safely skip this step by clicking the **Not now, maybe later** link.
1. On the settings page, select the development settings and theme that you prefer. Click the **Start Visual Studio** button to launch the Visual Studio Integrated Development Environment (IDE).
1. You should now see the **Start Page**. This will serve as confirmation that Visual Studio was installed successfully.

 Validate SQL Server 2016 LocalDb Installation

SQL Server 2016 LocalDb is routinely installed as part of your Visual Studio installation. In this task, we will validate that the installation was successful.

1. At the top of the Visual Studio window, click the **View** menu and then select the **Server Explorer** menu option.
1. In the **Server Explorer** pane, right-click the **Data Connections** node and then select the **Add Connection...** option.
1. In the **Choose Data Source** dialog, perform the following actions:
	1. In the **Data source** section, select the **Microsoft SQL Server** option.
	1. In the **Data provider** list, select the **.NET Framework Data Provider for SQL Server** option.
	1. Ensure that the checkbox next to the **Always use this selection** statement is checked.
	1. Click the **Continue** button.
1. In the **Add Connection** dialog, perform the following actions:
	1. In the **Server name** box, enter the value **(localdb)\MSSQLLOCALDB**.
	1. In the **Authentication** list, select the **Windows Authentication** option.
	1. In the **Connect to a database** section, locate the **Select or enter a database name** list. Select the **master** database in the list. 
	1. Click the **Test Connection** button at the bottom of the dialog. 
	1. The response popup window should state **"Test connection succeeded"**.
	1. Click the **OK** button to save the connection.
1. In the **Server Explorer** pane, you should see a node for the connection to the **localdb** database instance. 
1. Right-click the **localdb** connection node and select the **New Query** option.
1. In the query editor, enter the following query:
	```
	SELECT @@VERSION
	```
1. Click the **green arrow** button to **Execute** the query. The query should return the version of the running SQL Server instance as it's scalar result.

 Import Contoso Database

We will use an example database for the ficitional Contoso corporation throughout the course. In this task, we will install the schema and data necessary for the database.

1. Download the Contoso database **Data-Tier Application** file from the following link: [contoso.dacpac](../files/contoso.dacpac).
1. Save the downloaded **.dacpac** file to your local machine.
1. Return to the already running **Visual Studio** instance.
1. In the **Server Explorer** pane, right-click the **localdb** connection node and select the **Browse in SQL Server Object Explorer** option.
1. In the **SQL Server Object Explorer** pane, observe that your database connection has been pulled in automatically and the node has already been expanded. Locate the **Databases** folder within your **(localdb)\MSSQLLOCALDB** node.
1. Right-click the **Databases** folder and select the **Publish Data-tier Application** option.
1. In the **Publish Data-tier Application** dialog, perform the following actions:
	1. In the **File on disk** section, click the **Browse** button. Locate and select the previously saved **.dacpac** file.
	1. In the **Database name** box, enter the value **ContosoDB**.
	1. Click the **Publish** button.
	1. You will see a prompt indicating that potential data could be lost in the target database. Click the **Yes** button to continue the import.
1. The **Data Tools Operations** pane will open and show the progress of the publish action. Wait for the publish action to complete
	> **Note:** An icon with a green circle and checkmark will be shown once the publish action is completed.
1. In the **SQL Server Object Explorer** pane, right-click the **Databases** folder and select the **Refresh** option. You should now see the **ContosoDB** database. 
1. Click the **View** menu at the top of the Visual Studio window and then select the **Server Explorer** menu option.
1. Locate your existing **localdb** node that is connected to the **master** database. Right-click the node and select the **Modify Connection...** option.
1. In the **Modify Connection** dialog, perform the following actions:
	1. In the **Connect to a database** section, locate the **Select or enter a database name** list. Select the **ContosoDB** database in the list. 
	1. Click the **Test Connection** button at the bottom of the dialog. 
	1. The response popup window should state **"Test connection succeeded"**.
	1. Click the **OK** button to save the connection.
1. In the **Server Explorer** pane, you should see the node for the connection to the **localdb** database instance referesh automatically. 
1. Expand the **localdb** connection node and then expand the **Tables** folder.
1. Within the **Tables** folder, right-click the **Products** table and then select the **Show Table Data** option. A new tab will open and render the data within the **Products** table.
1. In the **Server Explorer** pane, right-click the **localdb** connection node and select the **New Query** option.
1. Locate that the database context list at the top of the query editor. Ensure that the **ContosoDB** database is selected instead of the **master** database.
1. In the query editor, enter the following query:
	```
	SELECT * FROM Customers
	```
1. Click the **green arrow** button to **Execute** the query. The query should return a list of customers in the database.
1. Close the **Visual Studio** application instance.
