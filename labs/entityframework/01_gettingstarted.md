# Getting Started: Referencing Entity Framework in your .NET Application

## Creating a .NET Console Application

1. Open **Visual Studio 2017**.
1. At the top of the Visual Studio window; click the **File** menu, hover over the **New** menu option and then select the **Project...** menu option.
1. In the **New Project** dialog, perform the following actions:
    1. Expand the **Templates** node, expand the **Visual C#** node, and then select the **Windows Classic Desktop** node. 
    1. Select the **Console App (.NET Framework)** template.
    1. In the **Name** box, enter the value **PASS.DataApp.DXConsole**.
    1. Ensure that the checkbox next to the **Create directory for solution** statement is selected.
    1. Click the **OK** button.
1. Wait for Visual Studio to create the new **solution** and **project**.
1. Once the project has been created, Visual Studio will automatically open the **Program.cs** class file. Leave this file open.

## Adding the Entity Framework Library

1. Click the **View** menu at the top of the Visual Studio window and then select the **Solution Explorer** option.
1. In the **Solution Explorer** pane, expand the **PASS.DataApp.DXConsole** project to reveal the project files.
1. Right-click the **References** node and select the **Manage NuGet Packages...** menu option.
1. In the **NuGet Package Manager** dialog, click the **Browse** tab.
1. In the **Browse** tab, perform the following actions:
    1. In the **Search** box, enter the text **EntityFramework** and then press the *Enter/Return* key.
    1. In the search results, locate and select the **EntityFramework by Microsoft** result. A pane will appear to the right with details about the package.
    1. In the pane, select the **6.2.0** version.
    1. Click the **Install** button.
    1. In the **Preview** dialog, view the list of changes to your project dependencies. Click the **OK** button.
    1. In the **License Acceptance** dialog, view the license information for each package and then click the **I Accept** button.
1. Close the **NuGet Package Manager** dialog.

## Implementing A Model Class

1. In the **Solution Explorer** pane, right-click the **PASS.DataApp.DXConsole** project, hover over the **Add** menu option and then select the **New Item...** menu option.
1. In the **Add New Item** dialog, perform the following actions:
    1. Expand the **Visual C# Items** node, and then select the **Code** node. 
    1. Select the **Class** template.
    1. In the **Name** box, enter the value **Product.cs**.
    1. Click the **OK** button. 
1. Once the file has been created, Visual Studio will automatically open the **Product.cs** class file. Leave this file open.
1. In the currently open **Product.cs** file, ensure that an **using** statement exists for the **System** namespace:
    ```
    using System;
    ```
1. Add a new **using** statement for the **System.ComponentModel.DataAnnotations** namespace:
    ```
    using System.ComponentModel.DataAnnotations;
    ```
1. Update the **Product** class definition by setting a **public** accessor:
    ```
    public class Product
    ```
1. Add a new **int** property named **ProductId** with public **get** and **set** accessors:
    ```
    public int ProductId { get; set; }
    ```
1. Update the **ProductId** property by adding a **Key** attribute to the property:
    ```
    [Key]
    public int ProductId { get; set; }
    ```
1. Add a new **string** property named **Name** with public **get** and **set** accessors:
    ```
    public string Name { get; set; }
    ```
1. Add a new **string** property named **ProductNumber** with public **get** and **set** accessors:
    ```
    public string ProductNumber { get; set; }
    ```
1. Add a new **string** property named **Color** with public **get** and **set** accessors:
    ```
    public string Color { get; set; }
    ```
1. Add a new **decimal** property named **StandardCost** with public **get** and **set** accessors:
    ```
    public decimal StandardCost { get; set; }
    ```
1. Add a new **decimal** property named **ListPrice** with public **get** and **set** accessors:
    ```
    public decimal ListPrice { get; set; }
    ```
1. Your **Product** class should now look like this:
    ```
    public class Product
    {
        [Key]
        public int ProductId { get; set; }

        public string Name { get; set; }

        public string ProductNumber { get; set; }

        public string Color { get; set; }

        public decimal StandardCost { get; set; }

        public decimal ListPrice { get; set; }
    }
    ```
1. Save and close the **Product.cs** class.

## Implementing A Entity Framework Context Class

1. In the **Solution Explorer** pane, right-click the **PASS.DataApp.DXConsole** project, hover over the **Add** menu option and then select the **New Item...** menu option.
1. In the **Add New Item** dialog, perform the following actions:
    1. Expand the **Visual C# Items** node, and then select the **Code** node. 
    1. Select the **Class** template.
    1. In the **Name** box, enter the value **ContosoContext.cs**.
    1. Click the **OK** button. 
1. Once the file has been created, Visual Studio will automatically open the **ContosoContext.cs** class file. Leave this file open.
1. In the currently open **ContosoContext.cs** file, add a new **using** statement for the **System.Data.Entity** namespace:
    ```
    using System.Data.Entity;
    ```
1. Update the **ContosoContext** class definition by setting a **public** accessor and inheriting from the **DbContext** class:
    ```
    public class ContosoContext : DbContext
    ```
1. Add a new constructor to the **ContosoContext** class using the following signature that contains no parameters:
    ```
    public ContosoContext() 
    { }
    ```
1. Update the new constructor by adding a call to the base class and passing in the following a string parameter with a value of ``Data Source=(localdb)\MSSQLLOCALDB;Initial Catalog=ContosoDB;``:
    ```
    public ContosoContext() : base(@"Data Source=(localdb)\MSSQLLOCALDB;Initial Catalog=ContosoDB;")
    { }
    ```
1. In the **ContosoContext** class, add a new **DbSet<Product>** property named **Products** with public **get** and **set** accessors and the **virtual** keyword:
    ```
    public virtual DbSet<Product> Products { get; set; }
    ```
1. Your **ContosoContext** class should now look like this:
    ```
    public class ContosoContext : DbContext
    {
        public ContosoContext() : base(@"Data Source=(localdb)\MSSQLLOCALDB;Initial Catalog=ContosoDB;")
        { }

        public virtual DbSet<Product> Products { get; set; }
    }
    ```
1. Save and close the **ContosoContext.cs** class.

## Implementing Basic Entity Framework Logic

1. In the currently open **Program.cs** file, ensure that an **using** statement exists for the **System** namespace:
    ```
    using System;
    ```
1. Ensure that an **using** statement exists for the **System.Threading.Tasks** namespace:
    ```
    using System.Threading.Tasks;
    ```
1. Add a new **using** statement for the **System.Data.Entity** namespace:
    ```
    using System.Data.Entity;
    ```
1. Add a new **RunAsync** method with the following signature:
    ```
    static async Task RunAsync()
    ```
1. Within the **RunAsync** method, Add a using block that instantiates a new instance of the **ContosoContext** class:
    ```
    using (ContosoContext context = new ContosoContext())
    {
    }
    ```
1. Within the using block, add a new line of code to execute a string query against the SQL database asynchronously and then return the result as a string:
    ```
    string response = await context.Database.SqlQuery<string>("SELECT @@VERSION").SingleOrDefaultAsync();
    ```
1. Add a new line of code to write the message **"Connection Successful"** to the console window along with the output of the previous line of code:
    ```
    Console.WriteLine($"Connection Successful: {response}");
    ```
1. Your **RunAsync** method should now look like this:
    ```
    static async Task RunAsync()
    {
        using (ContosoContext context = new ContosoContext())
        {
            string response = await context.Database.SqlQuery<string>("SELECT @@VERSION").SingleOrDefaultAsync();
            Console.WriteLine($"Connection Successful: {response}");
        }
    }
    ```

## Validate Solution

1. In the currently open **Program.cs** file, locate the **Main** method with the following signature:
    ```
    static void Main(string[] args)
    ```
1. Within the **Main** method, delete the existing line of code that writes **"Hello World!"** to the console window. You should now have an empty **Main** method:
    ```
    static void Main(string[] args)
    {        
    }
    ```
1. Add the following line of code to invoke the **RunAsync** method and wait for the asynchronous **Task** to complete:
    ```
    RunAsync().Wait();
    ```
1. After the last line of code, add a new line of code to write the message **"Application has completed execution. Press any key to exit."** to the console window:
    ```
    Console.WriteLine("Application has completed execution. Press any key to exit.");
    ```
1. After the last line of code, add a new line of code to read the new **keypress** from the console window:
    ```
    Console.ReadKey();
    ```
1. Your **Run** method should now look like this:
    ```
    static void Main(string[] args)
    {
        RunAsync().Wait();
        Console.WriteLine("Application has completed execution. Press any key to exit.");
        Console.ReadKey();
    }
    ```
1. At the top of the Visual Studio window; click the **Debug** menu, and then select the **Start Debugging** menu option.
1. Observe the **"Connection Successful"** message in the console window. Press any key to close the console window.
1. Close the **Visual Studio** application instance.
