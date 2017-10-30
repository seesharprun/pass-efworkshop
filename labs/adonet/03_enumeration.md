# Practice: Enumerating Over Results of a Database Query


## Open the Visual Studio Solution

1. Open **Visual Studio 2017**.
1. At the top of the Visual Studio window, click the **File** menu and hover over the **Recent Projects and Solutions** option.
1. Select the **PASS.DataApp.Console** solution.
1. Wait for Visual Studio to open the existing solution.

## Create a SQLCommand Instance

1. At the top of the Visual Studio window; click the **View** menu and then select the **Solution Explorer** option.
1. In the **Solution Explorer** pane; right-click the **PASS.DataApp.Console** project, hover over the **Add** menu option, and then select the **New Item...** menu option.
1. In the **Add New Item** dialog, perform the following actions:
    1. Expand the **Visual C# Items** node, and then select the **Code** node. 
    1. Select the **Class** template.
    1. In the **Name** box, enter the value **BasicQuery.cs**.
    1. Click the **OK** button. 
1. Once the file has been created, Visual Studio will automatically open the **BasicQuery.cs** class file. Leave this file open.
1. In the currently open **BasicQuery.cs** file, ensure that an **using** statement exists for the **System.Threading.Tasks** namespace:
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
1. Update the **BasicQuery** class definition by setting a **public** accessor:
    ```
    public class BasicQuery
    ```
1. Within the **BasicQuery** class, add a new **RunLogic** method with the following signature:
    ```
    public async Task RunLogic(SqlConnection connection)
    {        
    }
    ```
1. Within the **RunLogic** method, add a new line of code to create an instance of the **SqlCommand** class using the **CreateCommand** method of the **SqlConnection**  instance.
    ```
    SqlCommand command = connection.CreateCommand();
    ```
1. Add a new line of code to set the property of the **command** variable's **CommandType** property to **CommandType.Text**:
    ```
    command.CommandType = CommandType.Text;
    ```
1. Add a new line of code to set the property of the **command** variable's **CommandText** property to **"SELECT TOP(25) CustomerID, FirstName, LastName FROM Customer"**:
    ```
    command.CommandText = "SELECT TOP(25) CustomerID, FirstName, LastName FROM Customer";
    ```
    
## Enumerate Results

1. Add a new line of code to invoke the **ExecuteReaderAsync** method of the **command** variable, **await** the result and then store it in a variable of type **SqlDataReader** named **reader**:
    ```
    SqlDataReader reader = await command.ExecuteReaderAsync();
    ```
1. Add a **while** block after the last line of code that will run as long as the result of the **readAsync** method is **true** after an asynchronous **await**:
    ```
    while (await reader.ReadAsync())
    {
    }
    ```
1. Within the **while** block, add a line of code get the **Int32** value of the **first** ordinal column (*CustomerID*) and store it in a variable named **firstName**:
    ```
    int id = reader.GetInt32(0);
    ```
1. Add a line of code get the **String** value of the **second** ordinal column (*FirstName*) and store it in a variable named **firstName**:
    ```
    string firstName = reader.GetString(1);
    ```
1. Add a line of code get the **String** value of the **third** ordinal column (*LastName*) and store it in a variable named **lastName**:
    ```
    string lastName = reader.GetString(2);
    ```
1. Add a line of code to write the three variables to the console window using a special formatted string:
    ```
    System.Console.WriteLine($"[{id:000}]\t{lastName}, {firstName}");
    ```
1. Your **RunLogic** method should now look like this:
    ```
    public async Task RunLogic(SqlConnection connection)
    {
        SqlCommand command = connection.CreateCommand();
        command.CommandType = CommandType.Text;
        command.CommandText = "SELECT TOP(25) CustomerID, FirstName, LastName FROM Customers";
        SqlDataReader reader = await command.ExecuteReaderAsync();
        while (await reader.ReadAsync())
        {
            int id = reader.GetInt32(0);
            string firstName = reader.GetString(1);
            string lastName = reader.GetString(2);
            System.Console.WriteLine($"[{id:000}]\t{lastName}, {firstName}");
        }
    }
    ```

## Validate Solution

1. At the top of the Visual Studio window; click the **View** menu and then select the **Solution Explorer** option.
1. Locate and expand the **PASS.DataApp.Console** project.
1. Within the **PASS.DataApp.Console** project, locate and double-click the **Program.cs** file.
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
1. Within the **using** block, add the following line of code after the last line of existing code to create a new instance of the **BasicQuery** class and invoke the **RunLogic** method:
    ```
    await new BasicQuery().RunLogic(connection);
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
            await new BasicQuery().RunLogic(connection);
        }
    }
    ```
1. At the top of the Visual Studio window; click the **Debug** menu, and then select the **Start Debugging** menu option.
1. Observe the list of customers printed to the console window. Press any key to close the console window.

## Update the RunAsync Method

1. At the top of the Visual Studio window; click the **View** menu and then select the **Solution Explorer** option.
1. Locate and expand the **PASS.DataApp.Console** project.
1. Within the **PASS.DataApp.Console** project, locate and double-click the **Program.cs** file.
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
        new BasicQuery().RunLogic(connection);
    }
    ```
1. Remove the following line of code:
    ```
    new BasicQuery().RunLogic(connection);
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
