# Practice: Referencing ADO.NET in your .NET Application

## Creating a .NET Console Application

1. Open **Visual Studio 2017**.
1. At the top of the Visual Studio window; click the **File** menu, hover over the **New** menu option and then select the **Project...** menu option.
1. In the **New Project** dialog, perform the following actions:
    1. At the top of the dialog, select **.NET Framework 4.6.2** as your .NET version.
    1. Expand the **Templates** node, expand the **Visual C#** node, and then select the **Windows Classic Desktop** node. 
    1. Select the **Console App (.NET Framework)** template.
    1. In the **Name** box, enter the value **edX.DataApp.Console**.
    1. Ensure that the checkbox next to the **Create directory for solution** statement is selected.
    1. Click the **OK** button.
1. Wait for Visual Studio to create the new **solution** and **project**.
1. Once the project has been created, Visual Studio will automatically open the **Program.cs** class file. Leave this file open.
1. Click the **View** menu at the top of the Visual Studio window and then select the **Solution Explorer** option.
1. In the **Solution Explorer** pane, expand the **edX.DataApp.Console** project to reveal the project files. 

## Implementing Base ADO.NET Logic

1. In the currently open **Program.cs** file, ensure that an **using** statement exists for the **System.Threading.Tasks** namespace:
    ```
    using System.Threading.Tasks;
    ```
1. Add a new **using** statement for the **System.Data.SqlClient** namespace:
    ```
    using System.Data.SqlClient;
    ```
1. Add a new **RunAsync** method with the following signature:
    ```
    static async Task RunAsync()
    ```
1. Within the **RunAsync** method, add a new line to create a *string* variable named **connectionString** with a value of ``Data Source=(localdb)\MSSQLLOCALDB;Initial Catalog=ContosoDB;``:
    ```
    string connectionString = @"Data Source=(localdb)\MSSQLLOCALDB;Initial Catalog=ContosoDB;";
    ```
1. After the last line of code, add a using block that instantiates a new instance of the **SqlConnection** class using the **connectionString** variable as a parameter:
    ```
    using (SqlConnection connection = new SqlConnection(connectionString))
    {
    }
    ```
1. Within the using block, add a new line of code to invoke the **OpenAsync** method of the **SqlConnection** class and **await** the result of that invocation:
    ```
    await connection.OpenAsync();
    ```
1. After the last line of code within the using block, add a new line of code to write the message **"Connection Successful"** to the console window:
    ```
    System.Console.WriteLine("Connection Successful");
    ```
1. Your **RunAsync** method should now look like this:
    ```
    static async Task RunAsync()
    { 
        string connectionString = @"Data Source=(localdb)\MSSQLLOCALDB;Initial Catalog=ContosoDB;";
        using (SqlConnection connection = new SqlConnection(connectionString))
        {
            await connection.OpenAsync();
            System.Console.WriteLine("Connection Successful");
        }
    }
    ```

## Validate Solution

1. In the currently open **Program.cs** file, locate the **Main** method with the following signature:
    ```
    static void Main(string[] args)
    ```
1. Within the **Main** method, add the following line of code to invoke the **RunAsync** method and wait for the asynchronous **Task** to complete:
    ```
    RunAsync().Wait();
    ```
1. After the last line of code, add a new line of code to write the message **"Application has completed execution. Press any key to exit."** to the console window:
    ```
    System.Console.WriteLine("Application has completed execution. Press any key to exit.");
    ```
1. After the last line of code, add a new line of code to read the new **keypress** from the console window:
    ```
    System.Console.ReadKey();
    ```
1. Your **Run** method should now look like this:
    ```
    static void Main(string[] args)
    {
        RunAsync().Wait();
        System.Console.WriteLine("Application has completed execution. Press any key to exit.");
        System.Console.ReadKey();
    }
    ```
1. At the top of the Visual Studio window; click the **Debug** menu, and then select the **Start Debugging** menu option.
1. Observe the **"Connection Successful"** message in the console window. Press any key to close the console window.
1. Close the **Visual Studio** application instance.
