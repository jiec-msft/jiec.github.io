---
layout: post
title:  "Linux eval command"
date:   2023-09-07 16:48:00 +0800
categories: Linux
---

# Description

The `eval`` command in Linux is used to execute arguments as a shell command. It combines arguments into a single string and uses it as an input to the shell and execute the commands1. The eval command takes a string as its argument, and evaluates it as if you’d typed that string on a command line. If you pass several arguments, they are first joined with spaces between them1. For example, if you have a string ls -l, you can run it as a command using the following command:
```shell
eval "ls -l"
```


This will execute the ls -l command.


# Complicated example with Azure CLI


Suppose there is a file: `envs.txt`
```yaml
SPRING_PROFILES_ACTIVE=#{spring.profiles.active}
JAVA_OPTS="-XX:+UseG1GC -XX:ParallelGCThreads=4 -XX:ConcGCThreads=1 -XX:ThreadStackSize=128 -Xss1M -XX:MaxDirectMemorySize=10M -XX:MaxMetaspaceSize=196M -XX:ReservedCodeCacheSize=240M -Xmx512m -XX:+UseStringDeduplication -XX:+HeapDumpOnOutOfMemoryError -XX:OnOutOfMemoryError='shutdown'"
liquibase.shouldRun=false
BP_JVM_VERSION=11
NEW_RELIC_APP_NAME=#{asa.app.name}-#{newrelic.app.env.name.dev}
NEW_RELIC_DISTRIBUTED_TRACING_ENABLED=true
```

define a var using this file:
```shell
envlist=$(cat envs.txt | tr '\n' ' ')
```

Check the echo result:
```shell
╰─○ echo az spring app update -n app1 -s jiec-e-2 -g jiec-rg-2 --subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx --env ${envlist} --debug
az spring app update -n app1 -s jiec-e-2 -g jiec-rg-2 --subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx --env SPRING_PROFILES_ACTIVE=#{spring.profiles.active} JAVA_OPTS="-XX:+UseG1GC -XX:ParallelGCThreads=4 -XX:ConcGCThreads=1 -XX:ThreadStackSize=128 -Xss1M -XX:MaxDirectMemorySize=10M -XX:MaxMetaspaceSize=196M -XX:ReservedCodeCacheSize=240M -Xmx512m -XX:+UseStringDeduplication -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath='./java_pid<pid>.hprof' -XX:OnOutOfMemoryError='shutdown'" liquibase.shouldRun=false BP_JVM_VERSION=11 NEW_RELIC_APP_NAME=#{asa.app.name}-#{newrelic.app.env.name.dev} NEW_RELIC_DISTRIBUTED_TRACING_ENABLED=true  --debug
```

If don't use eval:
```shell
╭─jiec at cn-jiec-0506 in ~/test-env
╰─○ az spring app update -n app1 -s jiec-e-2 -g jiec-rg-2 --subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx --env ${envlist} --debug
cli.knack.cli: Command arguments: ['spring', 'app', 'update', '-n', 'app1', '-s', 'jiec-e-2', '-g', 'jiec-rg-2', '--subscription', 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx', '--env', 'SPRING_PROFILES_ACTIVE=#{spring.profiles.active} JAVA_OPTS="-XX:+UseG1GC -XX:ParallelGCThreads=4 -XX:ConcGCThreads=1 -XX:ThreadStackSize=128 -Xss1M -XX:MaxDirectMemorySize=10M -XX:MaxMetaspaceSize=196M -XX:ReservedCodeCacheSize=240M -Xmx512m -XX:+UseStringDeduplication -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=\'./java_pid<pid>.hprof\' -XX:OnOutOfMemoryError=\'shutdown\'" liquibase.shouldRun=false BP_JVM_VERSION=11 NEW_RELIC_APP_NAME=#{asa.app.name}-#{newrelic.app.env.name.dev} NEW_RELIC_DISTRIBUTED_TRACING_ENABLED=true ', '--debug']
```

You can see the it's parsed as one string by Azure CLI:
```
'SPRING_PROFILES_ACTIVE=#{spring.profiles.active} JAVA_OPTS="-XX:+UseG1GC -XX:ParallelGCThreads=4 -XX:ConcGCThreads=1 -XX:ThreadStackSize=128 -Xss1M -XX:MaxDirectMemorySize=10M -XX:MaxMetaspaceSize=196M -XX:ReservedCodeCacheSize=240M -Xmx512m -XX:+UseStringDeduplication -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=\'./java_pid<pid>.hprof\' -XX:OnOutOfMemoryError=\'shutdown\'" liquibase.shouldRun=false BP_JVM_VERSION=11 NEW_RELIC_APP_NAME=#{asa.app.name}-#{newrelic.app.env.name.dev} NEW_RELIC_DISTRIBUTED_TRACING_ENABLED=true '
```

But we want Azure CLI to parse it as space separated env key value paires. 

So use eval, you can find the tokenize result is expected:
```shell
╰─○ eval az spring app update -n app1 -s jiec-e-2 -g jiec-rg-2 --subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx --env ${envlist} --debug
cli.knack.cli: Command arguments: ['spring', 'app', 'update', '-n', 'app1', '-s', 'jiec-e-2', '-g', 'jiec-rg-2', '--subscription', 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx', '--env', 'SPRING_PROFILES_ACTIVE=#{spring.profiles.active}', "JAVA_OPTS=-XX:+UseG1GC -XX:ParallelGCThreads=4 -XX:ConcGCThreads=1 -XX:ThreadStackSize=128 -Xss1M -XX:MaxDirectMemorySize=10M -XX:MaxMetaspaceSize=196M -XX:ReservedCodeCacheSize=240M -Xmx512m -XX:+UseStringDeduplication -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath='./java_pid<pid>.hprof' -XX:OnOutOfMemoryError='shutdown'", 'liquibase.shouldRun=false', 'BP_JVM_VERSION=11', 'NEW_RELIC_APP_NAME=#{asa.app.name}-#{newrelic.app.env.name.dev}', 'NEW_RELIC_DISTRIBUTED_TRACING_ENABLED=true', '--debug']
```





