# Entity Framework Core

标签（空格分隔）： EFCore

---

## 自动生成 SQL 的相关命令

| dotnet 命令                                 | PowerShell 命令                   | 作用                                                        |
| ------------------------------------------- | --------------------------------- | ----------------------------------------------------------- |
| dotnet ef migrations add InitialCreate      | Add-Migration InitialCreate       | 对当前的 EF Model 的更改，增加一个 Migration 的配置文件     |
| dotnet ef database update                   | Update-Database                   | 更新当前的 Migration 到数据库中                             |
| dotnet ef migrations remove                 | Remove-Migration                  | 删除一个最新的 Migration                                    |
| dotnet ef database update LastGoodMigartion | Update-Database LastGoodMigration | 指定一个 Migration 去更新数据库                             |
| dotnet ef migrations script                 | Script-Migration                  | 将当前的 Migrationn 生成 SQL 脚本，SQL 脚本可以直接拿来使用 |

**使用命令前需要引用`Microsoft.EntityFrameworkCore.Tools`这个包**
**还要在`.csproj`文件中手动添加`<DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.2" />`，不知道这是不是这个版本的一个 Bug**
