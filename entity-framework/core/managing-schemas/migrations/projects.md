---
title: Migrations with Multiple Projects - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
---
Using a Separate Project
========================
You may want to store your migrations in a different assembly than the one containing your `DbContext`. You can also
use this strategy to maintain multiple sets of migrations, for example, one for development and another for
release-to-release upgrades.

To do this...

1. Create a new class library.

2. Add a reference to your DbContext assembly.

3. Move the migrations and model snapshot files to the class library.
   * If you haven't added any, add one to the DbContext project then move it.

4. Configure the migrations assembly:

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. Add a reference to your migrations assembly from the startup assembly.
   * If this causes a circular dependency, update the output path of the class library:

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStarupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

If you did everything correctly, you should be able to add new migrations to the project.

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migraitons add NewMigration --project MyApp.Migrations
```
