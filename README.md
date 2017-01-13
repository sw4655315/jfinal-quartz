jfinal-quartz
============
原址传送门：
[Dreampie  jfinal-quartz (https://github.com/Dreampie/jfinal-quartz)](https://github.com/Dreampie/jfinal-quartz)

use  it  very easy:

```java
//quartz start plugin
plugins.add(new QuartzPlugin());

//write job
public class DemoJob implements Job {
  public void execute(JobExecutionContext jobExecutionContext) throws JobExecutionException {
    //get param  from  job
    Map data = jobExecutionContext.getJobDetail().getJobDataMap();
    System.out.println("hi,"+data.get("name")+"," + new Date().getTime());
  }
}

//start it

//quartzKey   must  different  every task
//addParam  to  add param in job
//run  cron
new QuartzCronJob(new QuartzKey(1, "test", "test"), "*/5 * * * * ?", DemoJob.class)

.addParam("name", "quartz").start();
//run once
new QuartzOnceJob(new QuartzKey(2, "test", "test"), new Date(), DemoJob.class)

.addParam("name", "quartz").start();

```


###配置文件
默认配置文件在classpath下：
>classpath : quartz / jobs.properties
classpath : quartz / quartz.properties

可以指定自己路径的配置文件,在jfinalConfig中：
```
@Override
    public void configPlugin(Plugins me) {
        
        //定时任务
        QuartzPlugin quartz = new QuartzPlugin();
        quartz.setJobs("quartzJob.properties");
        quartz.setConfig("quartzConfig.properties")
        me.add(quartz);
    }
```

配置文件说明：
jobs.properties
```
#job的执行类
job.channel.class=cn.dreampie.function.Job
#job组
job.channel.group=default
#job唯一ID
job.channel.id=1
#job的执行周期
job.channel.cron=*/5 * * * * ?
#job是否开启
job.channel.enable=false
```

quartz.properties
```
#==================================================
# 配置实例名和id
#==================================================
org.quartz.scheduler.instanceName = defaultScheduler
org.quartz.scheduler.instanceId: default
org.quartz.scheduler.skipUpdateCheck: true

#==================================================
# 配置线程池
#==================================================
org.quartz.threadPool.class: org.quartz.simpl.SimpleThreadPool
org.quartz.threadPool.threadCount: 5
org.quartz.threadPool.threadPriority: 5

#==================================================
# 配置JobStore
#==================================================
org.quartz.jobStore.misfireThreshold: 600000

#org.quartz.jobStore.class=org.quartz.impl.jdbcjobstore.JobStoreTX
org.quartz.jobStore.class=org.quartz.simpl.RAMJobStore
#org.quartz.jobStore.driverDelegateClass=org.quartz.impl.jdbcjobstore.StdJDBCDelegate
#org.quartz.jobStore.useProperties=false
#org.quartz.jobStore.dataSource=db.migration.default
#org.quartz.jobStore.tablePrefix=QRTZ_
#org.quartz.jobStore.isClustered=true

#==================================================
# 配置数据库
#==================================================
#org.quartz.dataSource.db.migration.default.connectionProvider.class = cn.dreampie.common.plugin.quartz.QuartzConnectionProvider
```
