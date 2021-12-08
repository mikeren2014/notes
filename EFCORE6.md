# [EF Core 6](https://docs.microsoft.com/en-us/ef/core/what-is-new/ef-core-6.0/whatsnew)

## Temporal Table

## Migration Bundle

- Tool
    - Create
        
            dotnet tool install --global dotnet-ef
    - update
    
            dotnet tool update --global dotnet-ef

- Command
    - Create migration

            dotnet ef migrations add
    - Bundle

            dotnet ef migrations bundle

            dotnet ef migrations bundle --help
            
    - Execute

            efbundle.exe --connection <connection string>


# [Temporal Table](https://docs.microsoft.com/en-us/sql/relational-databases/tables/creating-a-system-versioned-temporal-table?view=sql-server-ver15)

## Create


        CREATE TABLE dbo.Employee
        (
            [EmployeeID] int NOT NULL PRIMARY KEY CLUSTERED
            , [Name] nvarchar(100) NOT NULL
            , [ValidFrom] datetime2 GENERATED ALWAYS AS ROW START
            , [ValidTo] datetime2 GENERATED ALWAYS AS ROW END
            , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
        )
        WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.EmployeeHistory));

## Drop Table

        ALTER TABLE [dbo].[Department] SET ( SYSTEM_VERSIONING = OFF  )
        GO

        /****** Object:  Table [dbo].[Department]    Script Date: 11/29/2021 8:23:54 AM ******/
        DROP TABLE [dbo].[Department]
        GO

        /****** Object:  Table [dbo].[MSSQL_TemporalHistoryFor_901578250]    Script Date: 11/29/2021 8:23:54 AM ******/
        DROP TABLE [dbo].[MSSQL_TemporalHistoryFor_901578250]
        GO

## Query

- FOR SYSTEM_TIME Clause
   - AS OF <date_time>
   - FROM <start_date_time> TO <end_date_time>
   - BETWEEN <start_date_time> AND <end_date_time>
   - CONTAINED IN (<start_date_time> , <end_date_time>)
   - ALL