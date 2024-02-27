##### [Maven Install](https://maven.apache.org/install.html) on EC2 Instance
###### Install
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