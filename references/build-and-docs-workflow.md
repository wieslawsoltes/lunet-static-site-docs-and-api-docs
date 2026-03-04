# Lunet Build and Documentation Workflow

## Local Build + Test

Run from repository root:

```bash
cd src
dotnet build -c Release
dotnet test -c Release
```

## Build Site with Local Lunet

```bash
cd site
dotnet ../src/Lunet/bin/Release/net10.0/lunet.dll --stacktrace build --dev
```

## Key Entry Points

- `readme.md`
- `site/docs/readme.md`
- `site/architecture.md`
- `src/Lunet.Application/LunetApp.cs`
- `src/Lunet.Core/Core/SiteObject.cs`
- `src/Lunet.Core/Core/ContentPlugin.cs`
- `src/Lunet.Core/Core/SiteApplication.cs`

## Plugin Docs Root

- `site/docs/plugins/readme.md`

## API Docs Feature

- `site/docs/plugins/api.md`
- `site/docs/plugins/api-dotnet.md`
- `src/Lunet.Api/Lunet.Api.csproj`
- `src/Lunet.Api.DotNet/Lunet.Api.DotNet.csproj`
- `src/Lunet.Api.DotNet.Extractor/Lunet.Api.DotNet.Extractor.csproj`
