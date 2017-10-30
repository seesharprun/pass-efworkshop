# Practice: Using the IEnumerable<T> Generic Type with ADO.NET

## Open the Visual Studio Solution

1. Open **Visual Studio 2017**.
1. At the top of the Visual Studio window, click the **File** menu and hover over the **Recent Projects and Solutions** option.
1. Select the **PASS.DataApp.Console** solution.
1. Wait for Visual Studio to open the existing solution.

## Create a Model Class

1. At the top of the Visual Studio window; click the **View** menu and then select the **Solution Explorer** option.
1. In the **Solution Explorer** pane; right-click the **PASS.DataApp.Console** project, hover over the **Add** menu option, and then select the **New Item...** menu option.
1. In the **Add New Item** dialog, perform the following actions:
    1. Expand the **Visual C# Items** node, and then select the **Code** node. 
    1. Select the **Class** template.
    1. In the **Name** box, enter the value **Customer.cs**.
    1. Click the **OK** button. 
1. Once the file has been created, Visual Studio will automatically open the **Customer.cs** class file. Leave this file open.
1. In the currently open **Customer.cs** file, update the **Customer** class definition by setting a **public** accessor:
    ```
    public class Customer
    ```
1. Add a new **int** property named **Id** with public **get** and **set** accessors:
    ```
    public string Id { get; set; }
    ```
1. Add a new **string** property named **Company** with public **get** and **set** accessors:
    ```
    public string Company { get; set; }
    ```
1. Add a new **string** property named **Email** with public **get** and **set** accessors:
    ```
    public string Email { get; set; }
    ```
1. Add a new **string** property named **Phone** with public **get** and **set** accessors:
    ```
    public string Phone { get; set; }
    ```
1. Your **Customer** class should now look like this:
    ```
    public class Customer
    {
        public int Id { get; set; }

        public string Company { get; set; }

        public string Email { get; set; }

        public string Phone { get; set; }
    }
    ```

## Create a SQLCommand Instance

1. At the top of the Visual Studio window; click the **View** menu and then select the **Solution Explorer** option.
1. In the **Solution Explorer** pane; right-click the **PASS.DataApp.Console** project, hover over the **Add** menu option, and then select the **New Item...** menu option.
1. In the **Add New Item** dialog, perform the following actions:
    1. Expand the **Visual C# Items** node, and then select the **Code** node. 
    1. Select the **Class** template.
    1. In the **Name** box, enter the value **GenericQuery.cs**.
    1. Click the **OK** button. 
1. Once the file has been created, Visual Studio will automatically open the **GenericQuery.cs** class file. Leave this file open.
1. In the currently open **GenericQuery.cs** file, ensure that an **using** statement exists for the **System.Collections.Generic** namespace:
    ```
    using System.Collections.Generic;
    ```
1. Ensure that an **using** statement exists for the **System.Threading.Tasks** namespace:
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
1. Update the **GenericQuery** class definition by setting a **public** accessor:
    ```
    public class GenericQuery
    ```
1. Within the **GenericQuery** class, add a new **RunLogic** method with the following signature:
    ```
    public async Task<IEnumerable<Customer>> RunLogic(SqlConnection connection)
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
1. Add a new line of code to set the property of the **command** variable's **CommandText** property to **"SELECT TOP(25) CompanyName, EmailAddress FROM Customer"**:
    ```
    command.CommandText = "SELECT TOP(25) CustomerID, CompanyName, EmailAddress, Phone FROM Customer";
    ```
    
## Convert Results to a Generic Collection

1. Add a new line of code to create a new instance of the **List<>** generic type with the **Customer** type used as the generic type parameter:
    ```
    List<Customer> customerList = new List<Customer>();
    ```
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
1. Within the **while** block, add a code block to create a new **Customer** instance and set the various properties of the class to their appropriate ordinal columns from the query result:
    ```
    Customer item = new Customer()
    {
        Id = reader.GetInt32(0),
        Company = reader.GetString(1),
        Email = reader.GetString(2),
        Phone = reader.GetString(3)
    };
    ```
1. Add a new line of code to add the new instance of the **Customer** class named **item** to the **customerList** variable using the **Add** method:
    ```
    customerList.Add(item);
    ```
1. Outside of the **while** block, return the **customerList** variable as the result of the method:
    ```
    return customerList;
    ```
1. Your **RunLogic** method should now look like this:
    ```
    public async Task<IEnumerable<Customer>> RunLogic(SqlConnection connection)
    {
        SqlCommand command = connection.CreateCommand();
        command.CommandType = CommandType.Text;
        command.CommandText = "SELECT TOP(25) CustomerID, CompanyName, EmailAddress, Phone FROM Customers";
        List<Customer> customerList = new List<Customer>();
        SqlDataReader reader = await command.ExecuteReaderAsync();
        while (await reader.ReadAsync())
        {
            Customer item = new Customer()
            {
                Id = reader.GetInt32(0),
                Company = reader.GetString(1),
                Email = reader.GetString(2),
                Phone = reader.GetString(3)
            };
            customerList.Add(item);
        }
        return customerList;
    }
    ```

## Enumerate Results

1. At the top of the Visual Studio window; click the **View** menu and then select the **Solution Explorer** option.
1. Locate and expand the **PASS.DataApp.Console** project.
1. Within the **PASS.DataApp.Console** project, locate and double-click the **Program.cs** file.
1. In the currently open **Program.cs** file, ensure that an **using** statement exists for the **System.Collections.Generic** namespace:
    ```
    using System.Collections.Generic;
    ```
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
1. Within the **using** block, add the following line of code after the last line of existing code to create a new instance of the **GenericQuery** class and invoke the **RunLogic** method. The result of the **RunLogic** method should be saved in a variable named **customers**:
    ```
    IEnumerable<Customer> customers = await new GenericQuery().RunLogic(connection);
    ```
1. Create a **foreach** block that enumerates over the **customers** collection and uses a local variable named **customer** for each instance:
    ```
    foreach (Customer customer in customers)
    {        
    }
    ```
1. Within the **foreach** block, add a line of code to write the three variables to the console window using a special formatted string:
    ```
    System.Console.WriteLine($"[{customer.Id:000}]\t{customer.Company}\t{customer.Email}");
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
            System.Console.WriteLine($"[{customer.Id:000}]\t{customer.Company,30}\t{customer.Email}");
        }
    }
    ```

## Validate Solution


1. Your **RunAsync** method should now look like this:
    ```
    static async Task RunAsync()
    { 
        string connectionString = @"Data Source=(localdb)\MSSQLLOCALDB;Initial Catalog=ContosoDB;";
        using (SqlConnection connection = new SqlConnection(connectionString))
        {
            await connection.OpenAsync();
            System.Console.WriteLine("Connection Successful");
            await new GenericQuery().RunLogic(connection);
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
        new GenericQuery().RunLogic(connection);
    }
    ```
1. Remove the following line of code:
    ```
    new GenericQuery().RunLogic(connection);
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
