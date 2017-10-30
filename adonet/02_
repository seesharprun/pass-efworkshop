# Practice: Populating a Data Adapter using ADO.NET

## Open the Visual Studio Solution

1. Open **Visual Studio 2017**.
1. At the top of the Visual Studio window, click the **File** menu and hover over the **Recent Projects and Solutions** option.
1. Select the **edX.DataApp.Console** solution.
1. Wait for Visual Studio to open the existing solution.

## Create a SQLCommand Instance

1. At the top of the Visual Studio window; click the **View** menu and then select the **Solution Explorer** option.
1. In the **Solution Explorer** pane; right-click the **edX.DataApp.Console** project, hover over the **Add** menu option, and then select the **New Item...** menu option.
1. In the **Add New Item** dialog, perform the following actions:
    1. Expand the **Visual C# Items** node, and then select the **Code** node. 
    1. Select the **Class** template.
    1. In the **Name** box, enter the value **DataAdapter.cs**.
    1. Click the **OK** button. 
1. Once the file has been created, Visual Studio will automatically open the **DataAdapter.cs** class file. Leave this file open.
1. In the currently open **DataAdapter.cs** file, ensure that an **using** statement exists for the **System.Threading.Tasks** namespace:
    ```
    using System.Threading.Tasks;
    ```
1. Add a new **using** statement for the **System.Data** namespace:
    ```
    using System.Data;
    ```
1. Add a new **using** statement for the **System.Data.SqlClient** namespace:
    ```
    using System.Data.SqlClient;
    ```
1. Update the **DataAdapter** class definition by setting a **public** accessor:
    ```
    public class DataAdapter
    ```
1. Within the **DataAdapter** class, add a new **RunLogic** method with the following signature:
    ```
    public async Task RunLogic(SqlConnection connection)
    {        
    }
    ```
1. Within the **RunLogic** method, add a new line of code to create a string variable with a value of **"SELECT TOP(20) CustomerID AS Id, FirstName AS First, LastName AS Last FROM Customers WHERE LEN(FirstName) < 7"**:
    ```
    string query = "SELECT TOP(20) CustomerID AS Id, FirstName AS First, LastName AS Last FROM Customers WHERE LEN(FirstName) < 7";
    ```
1. Your **RunLogic** method should now look like this:
    ```
    public void RunLogic(SqlConnection connection)
    {
        string query = "SELECT TOP(20) CustomerID AS Id, FirstName AS First, LastName AS Last FROM Customers WHERE LEN(FirstName) < 7";
    }
    ```

## Populate DataAdapter and DataTable
    
1. Add a **using** block after the last line of code that will create a new **SqlDataAdapter** instance using the existing **query** string and **connection** SqlConnection variables:
    ```
    using (SqlDataAdapter adapter = new SqlDataAdapter(query, connection))
    {
    }
    ```
1. Within the **using** block, add a new line of code to create a new **DataTable** instance:
    ```
    DataTable table = new DataTable();
    ```
1. Add a new line of code to call the **Fill** method of the existing **SqlDataAdapter** instance and pass in the **DataTable** instance as the method parameter:
    ```
    adapter.Fill(table);
    ```
1. Your **RunLogic** method should now look like this:
    ```
    public void RunLogic(SqlConnection connection)
    {
        string query = "SELECT TOP(20) CustomerID AS Id, FirstName AS First, LastName AS Last FROM Customers WHERE LEN(FirstName) < 7";
        using (SqlDataAdapter adapter = new SqlDataAdapter(query, connection))
        {
            DataTable table = new DataTable();
            adapter.Fill(table);
        }
    }
    ```

## Enumerate Results

1. Still within the **using** block, create a new **foreach** block to iterate over the **Rows** property of the **DataTable** instance:
    ```
    foreach (DataRow row in table.Rows)
    {
    }
    ```
1. Within the new **foreach** block, add another **foreach** block to iterate over the **Columns** property of the **DataTable** instance:
    ```
    foreach (DataColumn column in table.Columns)
    {
    }
    ```
1. Within the *inner* **foreach** block, add a new line of code to write the value of data in the current **row** using the **column** as an indexer to the console window:
    ```
    foreach (DataColumn column in table.Columns)
    {
        System.Console.Write("{0}\t", row[column]);
    }
    ```
1. Outside of the *inner* **foreach** block but still within the *outer* foreach block, add a new line of code to write a blank line to the console window:
    ```
    System.Console.WriteLine();
    ```
1. Your **RunLogic** method should now look like this:
    ```
    public void RunLogic(SqlConnection connection)
    {
        string query = "SELECT TOP(20) CustomerID AS Id, FirstName AS First, LastName AS Last FROM Customers WHERE LEN(FirstName) < 7";
        using (SqlDataAdapter adapter = new SqlDataAdapter(query, connection))
        {
            DataTable table = new DataTable();
            adapter.Fill(table);
            foreach (DataRow row in table.Rows)
            {
                foreach (DataColumn column in table.Columns)
                {
                    System.Console.Write("{0}\t", row[column]);
                }
                System.Console.WriteLine();
            }
        }
    }
    ```

## Validate Solution

1. At the top of the Visual Studio window; click the **View** menu and then select the **Solution Explorer** option.
1. Locate and expand the **edX.DataApp.Console** project.
1. Within the **edX.DataApp.Console** project, locate and double-click the **Program.cs** file.
1. Locate the **RunAsync** method with the following signature:
    ```
    static async Task RunAsync()
    ```
1. Within the **RunAsync** method, locate the **using** block that instantiates a new instance of the **SqlConnection** class:
    ```
    using (SqlConnection connection = new SqlConnection(connectionString))
    {
        await connection.OpenAsync();
        System.Console.WriteLine("Connection Successful");
    }
    ```
1. Within the **using** block, add the following line of code after the last line of existing code to create a new instance of the **DataAdapter** class and invoke the **RunLogic** method:
    ```
    await new DataAdapter().RunLogic(connection);
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
            await new DataAdapter().RunLogic(connection);
        }
    }
    ```
1. At the top of the Visual Studio window; click the **Debug** menu, and then select the **Start Debugging** menu option.
1. Observe the list of customers printed to the console window. Press any key to close the console window.

## Update the RunAsync Method

1. At the top of the Visual Studio window; click the **View** menu and then select the **Solution Explorer** option.
1. Locate and expand the **edX.DataApp.Console** project.
1. Within the **edX.DataApp.Console** project, locate and double-click the **Program.cs** file.
1. Locate the **RunAsync** method with the following signature:
    ```
    static async Task RunAsync()
    ```
1. Within the **RunAsync** method, locate the **using** block that instantiates a new instance of the **SqlConnection** class:
    ```
    using (SqlConnection connection = new SqlConnection(connectionString))
    {
        await connection.OpenAsync();
        System.Console.WriteLine("Connection Successful");
        new DataAdapter().RunLogic(connection);
    }
    ```
1. Remove the following line of code:
    ```
    new DataAdapter().RunLogic(connection);
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
1. Close the **Visual Studio** application instance.
