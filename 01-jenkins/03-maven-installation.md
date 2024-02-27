##### [Maven Install](https://maven.apache.org/download.cgi) on EC2 Instance
###### [Install](https://maven.apache.org/install.html)
```bash
java --version
mvn -version
wget https://mirrors.estointernet.in/apache/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz
tar -xvf apache-maven-3.6.3-bin.tar.gz
mv apache-maven-3.6.3 /opt/
rm -f apache-maven-3.6.3-bin.tar.gz
```
###### Path Setting
```bash
nano ~/.profile
M2_HOME='/opt/apache-maven-3.6.3'
PATH="$M2_HOME/bin:$PATH"
export PATH
source ~/.profile
mvn -version
```

###### Setup on Jenkins
- Manage Jenkins > Plugins > Available plugins > Check (Maven Integration & Maven Invoker) > Install without restart
- Manage Jenkins > Tools > Maven installations (Add Maven)
- Name (M2_HOME) > Path (/opt/apache-maven-3.6.3)
- Apply > Save
- Done
