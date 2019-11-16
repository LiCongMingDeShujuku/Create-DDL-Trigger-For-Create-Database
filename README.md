![CLEVER DATA GIT REPO](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/0-clever-data-github.png "李聪明的数据库")

# 为创建数据库制造DDL触发器
#### Create DDL Trigger For Create Database
**发布-日期: 2016年8月30日 (评论)**

![#](images/create-ddl-trigger-with-sql.png?raw=true "#")

## Contents

- [中文](#中文)
- [English](#English)
- [SQL Logic](#Logic)
- [Build Info](#Build-Info)
- [Author](#Author)
- [License](#License) 


## 中文
数据库管理的过程自动化。
下面是一些创建DDL触发器的快速SQL逻辑（logic），只要创建新的数据库就会触发该触发器。这设置起来不是很难，但是写一篇有关此的帖子也没有坏处。当你设置新的SQL Server环境并且想确保在统一标准内配置所有数据库时，会非常有用。
在这种情况下，我写了一个触发器执行一个名叫'SET DATABASE CONFIGS'的作业，在这个作业中有一些SQL逻辑（logic）可以设置还原模型等。更进一步，每当将新数据库添加到此服务器环境时，我的作业也将通知数据库组。
下面是触发器的逻辑（logic）：

## English
Process Automation For Database Management.
Here’s some quick SQL logic to create a DDL Trigger which is fired whenever a new database is created. This isn’t too terribly hard to setup, but doesn’t hurt to put a post out there for you guys. This is incredibly useful whenever you’re setting up a new SQL Server environment, and you want to make sure that all the databases are configured within a uniform standard.
In this case… I have a trigger executing a Job simply called ‘SET DATABASE CONFIGS’, and within this job there is some SQL logic that sets recovery models etc. To take it a step further; my job will also notify the database group whenever new databases are added to this server environment.
Here’s the Trigger logic:

---
## Logic
```SQL

use master;
set nocount on
if exists(select 1 from sys.server_triggers where name = 'ddl_trig_run_job_on_create_database')
drop trigger ddl_trig_run_job_on_create_database on all server
go
 
create trigger ddl_trig_run_job_on_create_database on all server for create_database as
declare @new_database varchar(255)
set @new_database = cast(eventdata().query('/event_instance/databasename[1]/text()') as NVarchar(128))
exec msdb..sp_start_job @job_name = 'SET DATABASE CONFIGS';
go

```

[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

- **李聪明的数据库 Lee's Clever Data**
- **Mike的数据库宝典 Mikes Database Collection**
- **李聪明的数据库** "Lee Songming"

[![Gist](https://img.shields.io/badge/Gist-李聪明的数据库-<COLOR>.svg)](https://gist.github.com/congmingshuju)
[![Twitter](https://img.shields.io/badge/Twitter-mike的数据库宝典-<COLOR>.svg)](https://twitter.com/mikesdatawork?lang=en)
[![Wordpress](https://img.shields.io/badge/Wordpress-mike的数据库宝典-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

---
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Lee Songming](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/1-clever-data-github.png "李聪明的数据库")

