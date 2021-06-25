# 修改bash_profile
```
vim ~/.bash_profile
```

```
# Java
export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_281.jdk/Contents/Home
export PATH=$JAVA_HOME/bin:$PATH
export CLASSPATH=.:$JAVA_HOME/lib/tools.jar:$JAVA_HOME/lib/dt.jar

# Maven
export MVN_HOME=/Users/JP23475/Library/maven/apache-maven-3.6.3/bin
export PATH=$MVN_HOME:$PATH

# Mysql
export MySQL_HOME=/usr/local/mysql/bin
export PATH=$MySQL_HOME:$PATH

# Hbase
# export HBASE_HOME=/Users/JP23475/Library/hbase/hbase-2.2.6
export HBASE_HOME=/Users/JP23475/Library/hbase/hbase-1.2.0
export PATH=$HBASE_HOME/bin:$PATH

export PATH
```