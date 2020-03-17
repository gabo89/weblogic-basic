
# WEBLOGIC BASICS

open nautilus with current directory set
```
cd /home/myUser/some/long/path/to/a/directory
nautilus .
nautilus /opt/project/weblogic
```
-------------------------------

if pwd and $HOME are the same

```
find $(pwd) -iname "*zoom*"
```
if not you can try 

```
find $HOME -iname "*zoom*"
--> /home/gabocuadros/Descargas/zoom_x86_64.rpm
```
then just do

yum install /home/gabocuadros/Descargas/zoom_x86_64.rpm

-------------------------------

-- to fixes local and remote branches remenber 

```
git pull origin <master>
git push origin <master>
```

configure your profile for git 
```
git config --global user.email "gabrielcuadros@gmail.com"
git config --global user.name  "gabocuadros"
```
considering you always work with master branches


-------------------------------

install sun jdk 
```
sudo yum install jdk-8u241-linux-x64.rpm
```

find where is oracle java  installed
```
rpm -ql jdk1.8 | grep jdk1.8.0_241
```
set java sun jdk to alternatives
```
sudo alternatives --install /usr/bin/java java /usr/java/jdk1.8.0_241-amd64/bin/java 3

```

select new java o default

```
sudo update-alternatives --config java
```

check new oracle java with version
```
java -version
```


view groups to check that gid 2000 is not used
```
getent group
```

view your max and min user ids 
```
grep -E '^UID_MIN|^UID_MAX' /etc/login.defs
```
check that user id 1100 is not used 

```
getent passwd | cut -d : -f3 | sort -nr
```

create oracle user , 

```
groupadd -g 2000 oinstall
useradd -u 1100 -g oinstall oracle
usermod -a -G oinstall gabocuadros
passwd oracle

chown -R oracle:oinstall /opt/project/weblogic/weblogic_app
chmod -R 775 /opt/project/weblogic/weblogic_app
```

view my hostname y domainname

```
hostname 
domainname
```

view my java home
```
alternatives --config java
```

also can be used
```
dirname $(dirname $(readlink -f $(which javac)))
```

posible result are 
```
usr/lib/jvm/java-1.8.0......
```

config oracle profile

```
sed '/export PATH/d' /home/oracle/.bash_profile
echo "MW_HOME=/opt/project/weblogic/weblogic_app" >> /home/oracle/.bash_profile
echo "export MW_HOME" >> /home/oracle/.bash_profile
echo "WLS_HOME=$MW_HOME/wlserver_12.1" >> /home/oracle/.bash_profile
echo "export WLS_HOME" >> /home/oracle/.bash_profile
echo "JAVA_HOME=/usr/java/jdk1.8.0_241-amd64; >> /home/oracle/.bash_profile
echo "export JAVA_HOME" >> /home/oracle/.bash_profile
echo "PATH=/usr/java/jdk1.8.0_241-amd64/bin:$PATH;" >> /home/oracle/.bash_profile
echo "export PATH" >> /home/oracle/.bash_profile
echo "USER_MEM_ARGS=\"-Xms32m -Xmx200m -XX:MaxPermSize=350m\"" >> /home/oracle/.bash_profile
source /home/oracle/.bash_profile
```
