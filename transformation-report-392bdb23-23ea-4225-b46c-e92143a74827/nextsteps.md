# Next Steps

## Issues resolved
- Transformed Bookstore.Domain.csproj to net8.0
- Transformed Bookstore.Data.csproj to net8.0
- Transformed Bookstore.Web.csproj to net8.0
- Transformed Bookstore.Cdk.csproj to net8.0
- Transformed Bookstore.Domain.Tests.csproj to net8.0

## Overview

The transformation appears to be successful with no build errors reported across any of the projects in the solution. All five projects (Bookstore.Data, Bookstore.Domain.Tests, Bookstore.Cdk, Bookstore.Web, and Bookstore.Domain) have compiled without issues.

## Validation Steps

### 1. Verify Target Framework Migration

Confirm that all projects are targeting the appropriate .NET version:

```bash
dotnet list package --framework
```

Review each `.csproj` file to ensure the `<TargetFramework>` element specifies the correct cross-platform framework (e.g., `net6.0`, `net7.0`, or `net8.0`).

### 2. Run Unit Tests

Execute the test suite to ensure functionality remains intact:

```bash
dotnet test Bookstore.Domain.Tests/Bookstore.Domain.Tests.csproj --logger "console;verbosity=detailed"
```

Review test results for any failures or warnings that may indicate compatibility issues.

### 3. Verify Package Dependencies

Check for deprecated or Windows-specific NuGet packages:

```bash
dotnet list package --deprecated
dotnet list package --vulnerable
```

Update any flagged packages to their latest stable versions compatible with cross-platform .NET.

### 4. Test on Target Platforms

Run the application on each target operating system to validate cross-platform compatibility:

**Linux:**
```bash
dotnet run --project Bookstore.Web/Bookstore.Web.csproj
```

**macOS:**
```bash
dotnet run --project Bookstore.Web/Bookstore.Web.csproj
```

**Windows:**
```bash
dotnet run --project Bookstore.Web/Bookstore.Web.csproj
```

### 5. Validate Data Access Layer

Test database connectivity and operations in the Bookstore.Data project:

- Verify connection strings work across platforms (use forward slashes for file paths if using SQLite)
- Test CRUD operations against your database
- Confirm Entity Framework migrations apply correctly:

```bash
dotnet ef database update --project Bookstore.Data/Bookstore.Data.csproj
```

### 6. Review CDK Infrastructure Code

Examine the Bookstore.Cdk project for any platform-specific assumptions:

- Verify AWS CDK constructs are compatible with the new .NET version
- Test CDK synthesis:

```bash
cd Bookstore.Cdk
cdk synth
```

### 7. Check Static Files and Assets

For the Bookstore.Web project, ensure static files are properly configured:

- Verify `wwwroot` folder contents are included in the build output
- Test that CSS, JavaScript, and image files load correctly
- Confirm case-sensitive file path references (important for Linux deployments)

### 8. Performance Testing

Run performance benchmarks to identify any regressions:

```bash
dotnet run --project Bookstore.Web/Bookstore.Web.csproj --configuration Release
```

Monitor memory usage, startup time, and response times compared to the legacy version.

### 9. Review Configuration Management

Validate configuration sources work correctly:

- Test `appsettings.json` loading
- Verify environment variable overrides function as expected
- Confirm user secrets work on all platforms (if used in development)

### 10. Code Analysis

Run static code analysis to identify potential issues:

```bash
dotnet build /p:RunAnalyzers=true /p:TreatWarningsAsErrors=true
```

Address any warnings that may indicate compatibility concerns.

## Final Validation

Before considering the migration complete:

1. Execute a full solution build in Release configuration:
   ```bash
   dotnet build --configuration Release
   ```

2. Run all tests in Release mode:
   ```bash
   dotnet test --configuration Release --no-build
   ```

3. Publish the web application and verify output:
   ```bash
   dotnet publish Bookstore.Web/Bookstore.Web.csproj --configuration Release --output ./publish
   ```

4. Test the published application:
   ```bash
   cd publish
   dotnet Bookstore.Web.dll
   ```

## Documentation Updates

Update project documentation to reflect:

- New target framework version
- Cross-platform deployment instructions
- Any changes to development environment setup
- Updated dependency requirements

## Monitoring Post-Migration

After deployment to production or staging environments:

- Monitor application logs for runtime exceptions
- Track performance metrics for any degradation
- Validate all integrations with external services
- Confirm scheduled jobs and background tasks execute correctly