# Hadoop Installation Guide

```bash
# Step 1: Install Java JDK 8
sudo apt install openjdk-8-jdk

# Verify installation
cd /usr/lib/jvm

# Step 2: Add this configuration to your bash file
sudo nano ~/.bashrc

# Paste the following commands into the .bashrc file
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64 
export PATH=$PATH:/usr/lib/jvm/java-8-openjdk-amd64/bin 
export HADOOP_HOME=~/hadoop-3.2.3/ 
export PATH=$PATH:$HADOOP_HOME/bin 
export PATH=$PATH:$HADOOP_HOME/sbin 
export HADOOP_MAPRED_HOME=$HADOOP_HOME 
export YARN_HOME=$HADOOP_HOME 
export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop 
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native 
export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib/native" 
export HADOOP_STREAMING=$HADOOP_HOME/share/hadoop/tools/lib/hadoop-streaming-3.2.3.jar
export HADOOP_LOG_DIR=$HADOOP_HOME/logs 
export PDSH_RCMD_TYPE=ssh

# Install SSH
sudo apt-get install ssh

# Step 3: Download and extract Hadoop
# Visit hadoop.apache.org and download the tar file
tar -zxvf ~/Downloads/hadoop-3.2.3.tar.gz

# Extract the tar file and configure Hadoop environment
cd hadoop-3.2.3/etc/hadoop
sudo nano hadoop-env.sh

# Set JAVA_HOME in hadoop-env.sh
JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64

# Step 4: Configure Hadoop
# Add this configuration in core-site.xml
sudo nano core-site.xml

<configuration> 
 <property> 
  <name>fs.defaultFS</name> 
  <value>hdfs://localhost:9000</value>  
 </property> 
 <property> 
  <name>hadoop.proxyuser.dataflair.groups</name> 
  <value>*</value> 
 </property> 
 <property> 
  <name>hadoop.proxyuser.dataflair.hosts</name> 
  <value>*</value> 
 </property> 
 <property> 
  <name>hadoop.proxyuser.server.hosts</name> 
  <value>*</value> 
 </property> 
 <property> 
  <name>hadoop.proxyuser.server.groups</name> 
  <value>*</value> 
 </property> 
</configuration>

# Add this configuration in hdfs-site.xml
sudo nano hdfs-site.xml

<configuration> 
 <property> 
  <name>dfs.replication</name> 
  <value>1</value> 
 </property> 
</configuration>

# Add this configuration in mapred-site.xml
sudo nano mapred-site.xml

<configuration> 
 <property> 
  <name>mapreduce.framework.name</name>  
  <value>yarn</value> 
 </property> 
 <property>
  <name>mapreduce.application.classpath</name> 
  <value>$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/*:$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/lib/*</value> 
 </property> 
</configuration>

# Add this configuration in yarn-site.xml
sudo nano yarn-site.xml

<configuration> 
 <property> 
  <name>yarn.nodemanager.aux-services</name> 
  <value>mapreduce_shuffle</value> 
 </property> 
 <property> 
  <name>yarn.nodemanager.env-whitelist</name> 
  <value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREP END_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAPRED_HOME</value> 
 </property> 
</configuration>

# Step 5: Set up SSH for Hadoop
ssh localhost 

ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa 
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys 

chmod 0600 ~/.ssh/authorized_keys 

# Step 6: Format the Hadoop filesystem
hadoop-3.2.3/bin/hdfs namenode -format

# Step 7: Start Hadoop
start-all.sh
