
![CLEVER DATA GIT REPO](https://github.com/congmingshuju/git-resources/blob/master/images/0-clever-data-github.png "李聪明 数据")


# 在AlwaysOn环境中如何配置Sharepoint权限
### Apply Permissions For Sharepoint Configurations In AlwaysOn
**发布-日期:  2016年7月27日 (评论)**


## Contents

- [中文](#中文)
- [English](#English)
- [SQL-Logic](#Logic)
- [Build Quality](#Build-Quality)
- [Author](#Author)
- [License](#License) 


## 中文
假设你有一个全新的SQL AlwaysOn环境用于SQL 2012或SQL 2014，现在Sharepoint管理员正在连接到监听器名称，进行安装和配置Sharepoint。每次尝试创建数据库时，Sharepoint配置过程可能会遇到一些权限错误。所以，你作为“DBA”应当确保为Sharepoint管理员帐户在群集中的每个Node上设置了所有可用权限，在此示例中为MyDomainMySharepointAccount。这时候它已被设置为OS和SQL Server的系统管理员，并且已添加到数据库所有者权限中。
(Basically it’s been added to Master dbo, and the local administrators group on the Server it’s self, and it’s already added to the Sysadmin role under each SQL Server instance.)
（基本上它已被添加到Master dbo，以及它自己的服务器上的本地管理员组，并且它已经添加到每个SQL Server instance下的Sysadmin角色中。）


## English
Lets say you have a brand new SQL AlwaysOn environment for SQL 2012 or SQL 2014 and now the Sharepoint Admins are connecting to the Listener name so they can install and configure Sharepoint. The Sharepoint configuration process may run into a few permissions errors whenever they try to create their databases. So you ‘the DBA’ will check to ensure that all the appropriate permissions are set on each Node in the Cluster for the Sharepoint Administrator account which in this example is MyDomainMySharepointAccount. You notice that it’s set as the Sysadmin for both the OS and SQL Server and has already been added to the database owner permissions.


然而,你仍然会在Sharepoint配置过程的“指定配置数据库设置”下遇到问题。他们看到了这个错误：
However; you still get issues under ‘Specify Configuration Database Settings’ for the Sharepoint configuraiton process. They see this error:

无法在MyAvailabilityGroupName处连接到SQL上的数据库主服务器。数据库可能不存在，或者当前用户没有连接权限。
Cannot connect to database master at SQL Server at MyAvailabilityGroupName. The database might not exist or the current user does not have permission to connect to it.

以微软的惯例来说，它仅仅被添加到OS和SQL服务器的适当角色中，并不意味着其他的微软应用程序会检查那些设置。在这种情况下，你需要在Securables中授予特定权限，以便Sharepoint安装程序将会进行验证是否已设置适当的权限。

Well… in usual Microsoft fashion just because it’s been added to the appropriate roles for the OS, and SQL Server it doesn’t mean the setup process from another Microsoft application will choose to check those to validate. In this case you’ll need to grant specific permissions in the Securables so the Sharepoint setup process will validate that the appropriate permissions have been set.

你可以通过2种方式执行此操作...通过SSMS客户端GUI添加安全性，或者运行以下逻辑。注意：这需要在群集配置中的所有节点上运行。
You can do this in 2 ways of course… Add the securables through the SSMS client GUI, or simply run the following logic. Note: This will need to be run on all Nodes in the Cluster configuration.

## Logic
```SQL
use [master]
GO
 
CREATE USER [MyDomain\MySharepointAccount] 
	FOR LOGIN [MyDomain\MySharepointAccount]
ALTER ROLE [db_owner] 
	ADD MEMBER [MyDomain\MySharepointAccount]

GRANT ALTER ANY AVAILABILITY GROUP TO 	[MyDomain\MySharepointAccount];
GRANT CONTROL SERVER TO 				[MyDomain\MySharepointAccount];
GRANT CREATE ANY DATABASE TO 			[MyDomain\MySharepointAccount];

```

![Perform 3 Grants](images/image0023.png?raw=true "Grant Any Availability Groip")



[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Quality 
| [![Build status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg=true)](https://ci.appveyor.com/project/tygerbytes/resourcefitness) | [![Coveralls](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?branch=master)](https://coveralls.io/github/tygerbytes/ResourceFitness?branch=master) | [![nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](https://www.nuget.org/packages/TW.Resfit.Core/) |
|-|-|-|

>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](https://ci.appveyor.com/project/tygerbytes/resourcefitness/history)

## Author

- **李聪明 数据 Clever Data**
- **Mike的数据库宝典 Mikes Database Collection**
- **李聪明** "Lee Songming"

[![Gist](https://img.shields.io/badge/Gist-李聪明数据-<COLOR>.svg)](https://gist.github.com/congmingshuju)
[![Twitter](https://img.shields.io/badge/Twitter-mike的数据库宝典-<COLOR>.svg)](https://twitter.com/mikesdatawork?lang=en)
[![Wordpress](https://img.shields.io/badge/Wordpress-mike的数据库宝典-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

---
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Lee Songming](https://github.com/congmingshuju/git-resources/blob/master/images/clever-data-gist-z5.png "李聪明 数据")



