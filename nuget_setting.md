# NuGet 上傳 #

自行建立一個.NET Standard 專案並放入必要 nuget 資訊

## 範例 ##

Your NuGet API Key is:

850f2a5b-ed6f-3dc7-8ce3-3dc859f27d84
You can register this key for a given repository with the following command:

nuget setapikey 850f2a5b-ed6f-3dc7-8ce3-3dc859f27d84 -source http://35.229.249.41/repository/{repository name}/
This window will automatically close after one minute.

## 真實指令 ##

dotnet pack PackageABC.csproj -c Release --version-suffix "beta-1" --include-symbols --output nupkgs

dotnet nuget push nupkgs/PackageABC.1.0.0-beta-2.nupkg -s http://35.229.249.41/repository/nuget-hosted/ -k 850f2a5b-ed6f-3dc7-8ce3-3dc859f27d84