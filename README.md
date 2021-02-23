# [dotnet-format issue #993](https://github.com/dotnet/format/issues/993)

### SETUP
- `npm install`
- `dotnet restore`

### STEPS TO REPRODUCE
1. `dotnet build --no-incremental`, expect to see output:
```
Build succeeded.
Example.cs(7,5): warning RCS0053: Fix formatting of an initializer.
    1 Warning(s)
    0 Error(s)
```
2. `dotnet format -wsa warn --check`, expect to see output:
```
  Formatting code files in workspace '{...}\dotnet-format-issue-993.csproj'.
  Program.cs(14,40): The type or namespace name 'App' could not be found (are you missing a using directive or an assembly reference?) (CS0246)
  Program.cs(14,40): The type or namespace name 'App' could not be found (are you missing a using directive or an assembly reference?) (CS0246)
  Example.cs(7,5): Fix formatting of an initializer. (RCS0053)
  Formatted code file '{...}\Program.cs'.
  Formatted code file '{...}\Example.cs'.
  Format complete in 2836ms.
```
3. lint-staged
    - make any chnage in `Example.cs` file so it could be staged to GIT (e.g. add comment / new line)
    - `git add Example.cs` - stage the file
    - `lint-staged` to check file for format errors
    - it runs commands from [.lintstagedrc](.lintstagedrc): `dotnet format -wsa warn --check --include`

### EXPECTED BEHAVIOUR
`dotnet format` should return non-zero exit code so that `lint-staged` could report failure and deny commit

### ACTUAL BEHAVIOUR
`lint-staged` reports a success and allows to commit files.
