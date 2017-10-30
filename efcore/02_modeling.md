# Practice: Modeling Tables using C\# Classes in Entity Framework

## Open the Visual Studio Solution

1. Open **Visual Studio 2017**.
1. At the top of the Visual Studio window; click the **File** menu and hover over the **Recent Projects and Solutions** option.
1. Select the **edX.DataApp.CoreConsole** solution.
1. Wait for Visual Studio to open the existing solution.

## Modify a Database Table

1. At the top of the Visual Studio window, click the **View** menu and then select the **Server Explorer** menu option.
1. In the **Server Explorer** pane, right-click the **Data Connections** node and then select the **Add Connection...** option.
1. In the **Add Connection** dialog, perform the following actions:
	1. In the **Server name** box, enter the value **(localdb)\MSSQLLOCALDB**.
	1. In the **Authentication** list, select the **Windows Authentication** option.
	1. In the **Connect to a database** section, locate the **Select or enter a database name** list. Select the **ContosoDB** database in the list. 
	1. Click the **Test Connection** button at the bottom of the dialog. 
	1. The response popup window should state **"Test connection succeeded"**.
	1. Click the **OK** button to save the connection.
1. In the **Server Explorer** pane, you should see a node for the connection to the **localdb** database instance. 
1. Right-click the **localdb** connection node and select the **New Query** option.
1. In the query editor, enter the following query:
	```
    ALTER TABLE 
        Products 
    ADD 
        ReleaseDate datetime2 NULL,
	    SafetyReviewResult bit NULL,
        ExternalId uniqueidentifier DEFAULT NEWSEQUENTIALID() NOT NULL
	```
1. Click the **green arrow** button to **Execute** the query. The query should return a message indicating that it has succeeded.
1. In the query editor, replace the existing query with the following new query:
	```
    UPDATE 
        Products
    SET 
        SafetyReviewResult = 1
    WHERE
        ProductNumber LIKE 'FR-%'
	```
1. Click the **green arrow** button to **Execute** the query. The query should return a message indicating that multiple rows have been affected by the query.
1. At the top of the Visual Studio window, click the **View** menu and then select the **Server Explorer** menu option.
1. In the **Server Explorer** pane; expand the **localdb** connection node and then expand the **Tables** node.
1. Right-click the **Tables** node and then select the **Refresh** menu option.
1. Right-click the **Products** table node and then select the **Show Table Data** menu option.
1. Observe the new columns in your database table.

## Update an Entity Framework Model Class

1. At the top of the Visual Studio window; click the **View** menu and then select the **Solution Explorer** option.
1. In the **Solution Explorer** pane; expand the **edX.DataApp.Console** project and then double-click the **Product.cs** file.
1. In the currently open **Product.cs** file, ensure that an **using** statement exists for the **System** namespace:
    ```
    using System;
    ```
1. Add a new **nullable DateTime** property named **ReleaseDate** with public **get** and **set** accessors:
    ```
    public DateTime? ReleaseDate { get; set; }
    ```
1. Add a new **nullable boolean** property named **SafetyReviewResult** with public **get** and **set** accessors:
    ```
    public bool? SafetyReviewResult { get; set; }
    ```
1. Add a new **Guid** property named **ExternalId** with public **get** and **set** accessors:
    ```
    public Guid ExternalId { get; set; }
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

        public DateTime? ReleaseDate { get; set; }

        public bool? SafetyReviewResult { get; set; }

        public Guid ExternalId { get; set; }
    }
    ```
1. Save and close the **Product.cs** class.

## Implement Query Logic

1. At the top of the Visual Studio window; click the **View** menu and then select the **Solution Explorer** option.
1. In the **Solution Explorer** pane; right-click the **edX.DataApp.Console** project, hover over the **Add** menu option, and then select the **New Item...** menu option.
1. In the **Add New Item** dialog, perform the following actions:
    1. Expand the **Visual C# Items** node, and then select the **Code** node. 
    1. Select the **Class** template.
    1. In the **Name** box, enter the value **ProductQuery.cs**.
    1. Click the **OK** button. 
1. Once the file has been created, Visual Studio will automatically open the **ProductQuery.cs** class file. Leave this file open.
1. In the currently open **ProductQuery.cs** file, ensure that an **using** statement exists for the **System** namespace:
    ```
    using System;
    ```
1. Ensure that an **using** statement exists for the **System.Collections.Generic** namespace:
    ```
    using System.Collections.Generic;
    ```
1. Add a new **using** statement for the **System.Linq** namespace:
    ```
    using System.Linq;
    ```
1. Update the **ProductQuery** class definition by setting a **public** accessor:
    ```
    public class ProductQuery
    ```
1. Within the **ProductQuery** class, add a new **RunLogic** method with the following signature:
    ```
    public void RunLogic(ContosoContext context)
    {        
    }
    ```
1. Within the **RunLogic** method, add a new line of code to get an **IEnumerable<Product>** variable that contains a query that would return all results in the **Products** table. (**Note**: Do not add a semicolon to the end of the line of code):
    ```
    IEnumerable<Product> products =   
    ```
1. Add a new line of code to create a LINQ query using the **from** keyword and a **product** variable for local expression evaluation:
    ```
    IEnumerable<Product> products = 
        from product in context.Products
    ```
1. Add a new line of code to expand the LINQ query by filtering the results to records that have a value of **true** for the **SafetyReviewResult** property. To accomplish this, reference the **SafetyReviewResult** nullable property and use the null coalescing operator to return **false** if the property is **null**:
    ```
    IEnumerable<Product> products = 
        from product in context.Products
        where product.SafetyReviewResult ?? false
    ```
1. Add a new line of code to expand the LINQ query by enumerating the results to a **IEnumerable<Product>** variable:
    ```
    IEnumerable<Product> products = 
        from product in context.Products
        where product.SafetyReviewResult ?? false
        select product;
    ```
1. Add a new line of code to enumerator over the **products** variable using the **foreach** keyword:
    ```
    foreach(Product product in products)
    {
    }
    ```
1. Within the **foreach** block, add a line of code to write information about each product to the console window:
    ```
    Console.WriteLine($"[{product.ProductNumber}]\t{product.Name, 35}\tPassed Review: {product.SafetyReviewResult}");
    ```
    
## Validate Solution

1. At the top of the Visual Studio window; click the **View** menu and then select the **Solution Explorer** option.
1. Locate and expand the **edX.DataApp.Console** project.
1. Within the **edX.DataApp.Console** project, locate and double-click the **Program.cs** file.
1. Locate the **RunAsync** method with the following signature:
    ```
    static async Task RunAsync()
    ```
1. Within the **RunAsync** method, locate the **using** block that instantiates a new instance of the **ContosoContext** class:
    ```
    using (ContosoContext context = new ContosoContext())
    {
        var creator = context.GetService<IDatabaseCreator>() as RelationalDatabaseCreator;
        await creator.ExistsAsync();
        Console.WriteLine("Connection Successful");
    }
    ```
1. Within the **using** block, add the following line of code after the last line of existing code to create a new instance of the **ProductQuery** class and invoke the **RunLogic** method:
    ```
    new ProductQuery().RunLogic(context);
    ```
1. Your **RunAsync** method should now look like this:
    ```
    static async Task RunAsync()
    {
        using (ContosoContext context = new ContosoContext())
        {
            var creator = context.GetService<IDatabaseCreator>() as RelationalDatabaseCreator;
            await creator.ExistsAsync();
            Console.WriteLine("Connection Successful");
            new ProductQuery().RunLogic(context);
        }
    }
    ```
1. At the top of the Visual Studio window; click the **Debug** menu, and then select the **Start Debugging** menu option.
1. Observe the list of products printed to the console window. Press any key to close the console window.
1. Close the **Visual Studio** application instance.
