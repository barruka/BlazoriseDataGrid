# BlazoriseDataGrid

Project created to test Blazorise DataGrid CustomFilter behaviour

```
dotnet new blazorserver -n BlazoriseDataGrid
```

Added packages:
```
  <ItemGroup>
    <PackageReference Include="Blazorise.Bootstrap" Version="1.0.2" />
    <PackageReference Include="Blazorise.DataGrid" Version="1.0.2" />
    <PackageReference Include="Blazorise.Icons.FontAwesome" Version="1.0.2" />
  </ItemGroup>
```

Added in '_Imports.razor':
```
@using Blazorise
```

Added at top of 'Program.cs':
```
using Blazorise;
using Blazorise.Bootstrap;
using Blazorise.Icons.FontAwesome;
```

In 'Program.cs', before 'var app = builder.Build();' added:
```
builder.Services
    .AddBlazorise(options =>
    {
        options.Immediate = true;
    })
    .AddBootstrapProviders()
    .AddFontAwesomeIcons();

```

In '/Pages/_Layout.cshtml' before closing ```</head>```:
```
    <!-- inside of head section -->
	<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.6.1/dist/css/bootstrap.min.css" integrity="sha384-zCbKRCUGaJDkqS1kPbPd7TveP5iyJE0EjAuZQTgFLD2ylzuqKfdKlfG/eSrtxUkn" crossorigin="anonymous">
	<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.15.4/css/all.css">

	<link href="_content/Blazorise/blazorise.css" rel="stylesheet" />
	<link href="_content/Blazorise.Bootstrap/blazorise.bootstrap.css" rel="stylesheet" />
```

In '/Pages/_Layout.cshtml' in body section after '@RenderBody()':
```
    <!-- inside of body section and after the div/app tag  -->
	<script src="https://cdn.jsdelivr.net/npm/jquery@3.5.1/dist/jquery.slim.min.js" integrity="sha384-DfXdz2htPH0lsSSs5nCTpuj/zy4C+OGpamoFVy38MVBnE+IbbVYUew+OrCXaRkfj" crossorigin="anonymous"></script>
	<script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.1/dist/umd/popper.min.js" integrity="sha384-9/reFTGAW83EW2RDu2S0VKaIzap3H66lZH81PoYlFhbGU+6BZp6G7niu735Sk7lN" crossorigin="anonymous"></script>
	<script src="https://cdn.jsdelivr.net/npm/bootstrap@4.6.1/dist/js/bootstrap.min.js" integrity="sha384-VHvPCCyXqtD5DqJeNxl2dtTyhF78xXNXdkwX1CZeRusQfRKp+tA7hAShOK/B/fQ2" crossorigin="anonymous"></script>
```

Added 'DataGridTest.razor' in '/Pages':
```csharp
@page "/datagridtest"

@using Blazorise.DataGrid


Custom Filter: <TextEdit @bind-Text="@customFilterValue"></TextEdit>

<DataGrid TItem="Employee" Data="@employeeList" CustomFilter="@OnCustomFilter" Responsive>
    <DataGridColumn Field="@nameof( Employee.FirstName )" Caption="Name" Editable="false"></DataGridColumn>
</DataGrid>

@code {
    private class Employee
    {
        public string FirstName { get; set; } = String.Empty;
    }

    private List<Employee> employeeList = new()
    {
        new() { FirstName = "David" },
        new() { FirstName = "MLaden" },
        new() { FirstName = "John" },
        new() { FirstName = "Ana" },
        new() { FirstName = "Jessica" }
    };

    private string customFilterValue;

    private bool OnCustomFilter(Employee model)
    {
        Console.WriteLine($"OnCustomFilter {DateTime.Now.ToString("yyyyMMddHHmmssffff")} {model.FirstName} {customFilterValue}"
        );

        // We want to accept empty value as valid or otherwise
        // datagrid will not show anything.
        if (string.IsNullOrEmpty(customFilterValue))
            return true;

        return model.FirstName?.Contains(customFilterValue, StringComparison.OrdinalIgnoreCase) == true;
    }

}
```

Run the project and hit '/datagridtest'. Check debug console:
```
OnCustomFilter 202203310853453928 David 
OnCustomFilter 202203310853453962 MLaden 
OnCustomFilter 202203310853453962 John 
OnCustomFilter 202203310853453962 Ana 
OnCustomFilter 202203310853453962 Jessica 
OnCustomFilter 202203310853455377 David 
OnCustomFilter 202203310853455378 MLaden 
OnCustomFilter 202203310853455378 John 
OnCustomFilter 202203310853455378 Ana 
OnCustomFilter 202203310853455378 Jessica 
OnCustomFilter 202203310853455991 David 
OnCustomFilter 202203310853455991 MLaden 
OnCustomFilter 202203310853455992 John 
OnCustomFilter 202203310853455992 Ana 
OnCustomFilter 202203310853455992 Jessica
```