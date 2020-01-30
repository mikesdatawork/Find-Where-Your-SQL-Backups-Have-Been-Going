![MIKES DATA WORK GIT REPO](https://raw.githubusercontent.com/mikesdatawork/images/master/Mikes_Data_Work_Header_890x170.png "Mikes Data Work")        

# Find Where Your SQL Backups Have Been Going
**Post Date: November 4, 2015**





## Contents    
- [About Process](##About-Process)  
- [SQL Logic](#SQL-Logic)  
- [Build Info](#Build-Info)  
- [Author](#Author)  
- [License](#License)       

## About-Process


<p>Here's some very basic SQL Logic to help you find where your backups are located. </p> 

![Find Where SQL Backups Have Been Going]( https://mikesdatawork.files.wordpress.com/2015/11/find_where_your_sql_backups_are_located.png "Find Where SQL Backups Have Been Going")
 
     
## SQL-Logic
```SQL
use master;
set nocount on
 
select
    'database'      = upper(bs.database_name)
,   'time_of_backup'    = replace(replace(replace(left(max(bs.backup_finish_date), 19),':', '-'), 'AM', 'am'), 'PM', 'pm') + ' ' + datename(dw, max(bs.backup_finish_date))
,   'location'      = reverse(right(reverse(upper(bmf.physical_device_name)), len(bmf.physical_device_name) - charindex('\',reverse(bmf.physical_device_name),1) + 1))
,   'backup_file'       = right(bmf.physical_device_name, charindex('\', reverse('\' + bmf.physical_device_name)) - 1)
from
    msdb.dbo.backupset bs join msdb.dbo.backupmediafamily bmf on bs.media_set_id = bmf.media_set_id
--where
--  bs.backup_finish_date > (select getdate()- 30)
--  and bs.database_name  = 'COLLECTIONS'
group by
    bs.database_name, bs.backup_finish_date, bmf.physical_device_name
order by
    bs.database_name, bs.backup_finish_date desc
```


[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

[![Gist](https://img.shields.io/badge/Gist-MikesDataWork-<COLOR>.svg)](https://gist.github.com/mikesdatawork)
[![Twitter](https://img.shields.io/badge/Twitter-MikesDataWork-<COLOR>.svg)](https://twitter.com/mikesdatawork)
[![Wordpress](https://img.shields.io/badge/Wordpress-MikesDataWork-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

     
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Mikes Data Work](https://raw.githubusercontent.com/mikesdatawork/images/master/Mikes_Data_Work_Social.png "Mikes Data Work")

