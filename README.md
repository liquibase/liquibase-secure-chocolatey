[![Build status](https://ci.appveyor.com/api/projects/status/3avdga24cmxict48?svg=true)](https://ci.appveyor.com/project/adriens/chocolatey-liquibase)

# Liquibase chocolatey package

To prepare a new Liquibase release, two ways : manually or helped by
`ant` build script.

## Build

1. Update Liquibase version in `liquibase.properties`
2. Run `ant make`
3. Try local install : `choco install -fdv liquibase.nuspec`
4. Run liquibase : `liquibase --version` then `liquibase --help`
5. You are done : make a Pull Request (all required files have been generated
with all requireds fields, including checksums) : ***tools and `nuspec` only***
