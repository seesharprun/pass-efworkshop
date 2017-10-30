# Practice: Modeling Relationships using Entity Framework

## Open the Visual Studio Solution

1. Open **Visual Studio 2017**.
1. At the top of the Visual Studio window, click the **File** menu and hover over the **Recent Projects and Solutions** option.
1. Select the **PASS.DataApp.CoreConsole** solution.
1. Wait for Visual Studio to open the existing solution.

## Create a New Entity Framework Model Class

1. At the top of the Visual Studio window; click the **View** menu and then select the **Solution Explorer** option.
1. In the **Solution Explorer** pane; right-click the **PASS.DataApp.Console** project, hover over the **Add** menu option, and then select the **New Item...** menu option.
1. In the **Add New Item** dialog, perform the following actions:
    1. Expand the **Visual C# Items** node, and then select the **Code** node. 
    1. Select the **Class** template.
    1. In the **Name** box, enter the value **ProductCategory.cs**.
    1. Click the **OK** button. 
1. Once the file has been created, Visual Studio will automatically open the **ProductCategory.cs** class file. Leave this file open.
1. In the currently open **ProductCategory.cs** file, ensure that an **using** statement exists for the **System** namespace:
    ```
    using System;
    ```
1. Add a new **using** statement for the **System.ComponentModel.DataAnnotations** namespace:
    ```
    using System.ComponentModel.DataAnnotations;
    ```
1. Update the **ProductCategory** class definition by setting a **public** accessor:
    ```
    public class ProductCategory
    ```
1. Add a new **int** property named **ProductCategoryId** with public **get** and **set** accessors:
    ```
    public int ProductCategoryId { get; set; }
    ```
1. Update the **ProductCategoryId** property by adding a **Key** attribute to the property:
    ```
    [Key]
    public int ProductCategoryId { get; set; }
    ```
1. Add a new **string** property named **Name** with public **get** and **set** accessors:
    ```
    public string Name { get; set; }
    ```
1. Your **ProductCategory** class should now look like this:
    ```
    public class ProductCategory
    {
        [Key]
        public int ProductCategoryId { get; set; }

        public string Name { get; set; }
    }
    ```
1. Save and close the **ProductCategory.cs** class.

## Update an Existing Entity Framework Model Class

1. At the top of the Visual Studio window; click the **View** menu and then select the **Solution Explorer** option.
1. In the **Solution Explorer** pane; expand the **PASS.DataApp.Console** project and then double-click the **Product.cs** file.
1. In the currently open **Product.cs** file, add a new **ProductCategory** property named **ProductCategory** with public **get** and **set** accessors and the **virtual** keyword:
    ```
    public virtual ProductCategory ProductCategory { get; set; }
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

        public virtual ProductCategory ProductCategory { get; set; }
    }
    ```
1. Save and close the **Product.cs** class.

## Update the Entity Framework Context Class

1. At the top of the Visual Studio window; click the **View** menu and then select the **Solution Explorer** option.
1. In the **Solution Explorer** pane; expand the **PASS.DataApp.Console** project and then double-click the **ContosoContext.cs** file.
1. In the currently open **ContosoContext.cs** file, add a new **DbSet<ProductCategory>** property named **ProductCategories** with public **get** and **set** accessors and the **virtual** keyword:
    ```
    public virtual DbSet<ProductCategory> ProductCategories { get; set; }
    ```
1. Your **ContosoContext** class should now look like this:
    ```
    public class ContosoContext : DbContext
    {
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            string connectionString = @"Data Source=(localdb)\MSSQLLOCALDB;Initial Catalog=ContosoDB;";
            optionsBuilder.UseSqlServer(connectionString);
        }

        public virtual DbSet<Product> Products { get; set; }

        public virtual DbSet<ProductCategory> ProductCategories { get; set; }
    }
    ```
1. Save and close the **ContosoContext.cs** class.

## Implement Query Logic

1. At the top of the Visual Studio window; click the **View** menu and then select the **Solution Explorer** option.
1. In the **Solution Explorer** pane; right-click the **PASS.DataApp.Console** project, hover over the **Add** menu option, and then select the **New Item...** menu option.
1. In the **Add New Item** dialog, perform the following actions:
    1. Expand the **Visual C# Items** node, and then select the **Code** node. 
    1. Select the **Class** template.
    1. In the **Name** box, enter the value **ProductAndCategoryQuery.cs**.
    1. Click the **OK** button. 
1. Once the file has been created, Visual Studio will automatically open the **ProductAndCategoryQuery.cs** class file. Leave this file open.
1. In the currently open **ProductAndCategoryQuery.cs** file, ensure that an **using** statement exists for the **System** namespace:
    ```
    using System;
    ```
1. Ensure that an **using** statement exists for the **System.Collections.Generic** namespace:
    ```
    using System.Collections.Generic;
    ```
1. Add a new **using** statement for the **System.Threading.Tasks** namespace:
    ```
    using System.Threading.Tasks;
    ```
1. Add a new **using** statement for the **System.Linq** namespace:
    ```
    using System.Linq;
    ```
1. Add a new **using** statement for the **Microsoft.EntityFrameworkCore** namespace:
    ```
    using Microsoft.EntityFrameworkCore;
    ```
1. Update the **ProductAndCategoryQuery** class definition by setting a **public** accessor:
    ```
    public class ProductAndCategoryQuery
    ```
1. Within the **ProductAndCategoryQuery** class, add a new **RunLogic** method with the following signature:
    ```
    public async Task RunLogic(ContosoContext context)
    {        
    }
    ```
1. Within the **RunLogic** method, add a new line of code to get an **IEnumerable<Product>** variable that contains a query that would return all results in the **Products** table. (**Note**: Do not add a semicolon to the end of the line of code):
    ```
    IEnumerable<Product> products = await context.Products   
    ```
1. Add a new line of code to fluently extend the last line of code by including the **ProductCategory** related entity:
    ```
    IEnumerable<Product> products = await context.Products
        .Include(p => p.ProductCategory)
    ```
1. Add a new line of code to fluently extend the last line of code by filtering the results to only **Product** entities that have a **ListPrice** value between **$1250.00** and **$1450.00**:
    ```
    IEnumerable<Product> products = await context.Products
        .Include(p => p.ProductCategory)
        .Where(p => p.ListPrice > 1250m && p.ListPrice < 1450m)
    ```
1. Add a new line of code to fluently extend the last line of code by getting the top **20** records that match the query:
    ```
    IEnumerable<Product> products = await context.Products
        .Include(p => p.ProductCategory)
        .Where(p => p.ListPrice > 1250m && p.ListPrice < 1450m)
        .Take(20)
    ```
1. Add a new line of code to fluently extend the last line of code by enumerating the results to a **List<Product>** asynchronously:
    ```
    IEnumerable<Product> products = await context.Products
        .Include(p => p.ProductCategory)
        .Where(p => p.ListPrice > 1250m && p.ListPrice < 1450m)
        .Take(20)
        .ToListAsync();
    ```
1. Add a new line of code to enumerator over the **products** variable using the **foreach** keyword:
    ```
    foreach(Product product in products)
    {
    }
    ```
1. Within the **foreach** block, add a line of code to write information about each product to the console window:
    ```
    Console.WriteLine($"[{product.ProductCategory.Name}]\t{product.Name,35}\t{product.ListPrice:C}");
    ```
    
## Validate Solution

1. At the top of the Visual Studio window; click the **View** menu and then select the **Solution Explorer** option.
1. Locate and expand the **PASS.DataApp.Console** project.
1. Within the **PASS.DataApp.Console** project, locate and double-click the **Program.cs** file.
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
        await new ProductQuery().RunLogic(context);
    }
    ```
1. Within the **using** block, remove the following line of code:
    ```
    await new ProductQuery().RunLogic(context);
    ```
1. Add the following line of code after the last line of existing code to create a new instance of the **ProductAndCategoryQuery** class and invoke the **RunLogic** method:
    ```
    await new ProductAndCategoryQuery().RunLogic(context);
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
            await new ProductAndCategoryQuery().RunLogic(context);
        }
    }
    ```
1. At the top of the Visual Studio window; click the **Debug** menu, and then select the **Start Debugging** menu option.
1. Observe the list of products printed to the console window. Press any key to close the console window.
1. Close the **Visual Studio** application instance.
