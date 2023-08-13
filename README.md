# DE_1st_work_with_hadoop

Файлы с текстом произведения "Война и мир" расположены в [папке](https://github.com/PolarJaba/DE_1st_work_with_hadoop/blob/main/war_and_peace).

### Получение списка контейнеров
```
docker ps
```

output:
```
CONTAINER ID   IMAGE                                                    COMMAND                  CREATED          STATUS                    PORTS                                          NAMES
c5732199d7c3   gethue/hue:4.6.0                                         "./startup.sh"           20 minutes ago   Up 20 minutes             0.0.0.0:8888->8888/tcp                         docker-hadoop-hive-parquet-hue-1
40589bbcf006   bde2020/hadoop-resourcemanager:2.0.0-hadoop2.7.4-java8   "/entrypoint.sh /run…"   20 minutes ago   Up 20 minutes (healthy)   8088/tcp                                       docker-hadoop-hive-parquet-resourcemanager-1
2353f66de33a   bde2020/hive:2.3.2-postgresql-metastore                  "entrypoint.sh /opt/…"   20 minutes ago   Up 20 minutes             10000/tcp, 0.0.0.0:9083->9083/tcp, 10002/tcp   docker-hadoop-hive-parquet-hive-metastore-1
9ddb0ab1b2c8   bde2020/hadoop-namenode:2.0.0-hadoop2.7.4-java8          "/entrypoint.sh /run…"   20 minutes ago   Up 20 minutes (healthy)   0.0.0.0:50070->50070/tcp                       docker-hadoop-hive-parquet-namenode-1
0ec9ab3b4c90   postgres:12.1-alpine                                     "docker-entrypoint.s…"   20 minutes ago   Up 20 minutes             0.0.0.0:56106->5432/tcp                        docker-hadoop-hive-parquet-huedb-1
857aa00e714c   bde2020/hive:2.3.2-postgresql-metastore                  "entrypoint.sh /bin/…"   20 minutes ago   Up 20 minutes             0.0.0.0:10000->10000/tcp, 10002/tcp            docker-hadoop-hive-parquet-hive-server-1
92e57d2c1a86   bde2020/hadoop-datanode:2.0.0-hadoop2.7.4-java8          "/entrypoint.sh /run…"   20 minutes ago   Up 20 minutes (healthy)   0.0.0.0:50075->50075/tcp                       docker-hadoop-hive-parquet-datanode-1
f6c4d8f0aef2   bde2020/hive-metastore-postgresql:2.3.0                  "/docker-entrypoint.…"   20 minutes ago   Up 20 minutes             0.0.0.0:5432->5432/tcp                         docker-hadoop-hive-parquet-hive-metastore-postgresql-1
```
### Подключение к контейнеру и создание рабочей папки 
```
docker exec -it 92e57d2c1a86 bash
root@92e57d2c1a86:/# ls
```
output:
```
bin   dev            etc     hadoop-data  lib    media  opt   root  run.sh  srv  tmp  var
boot  entrypoint.sh  hadoop  home         lib64  mnt    proc  run   sbin    sys  usr
```
input:
```
root@92e57d2c1a86:/# cd usr
root@92e57d2c1a86:/usr# mkdir world_and_peace
root@92e57d2c1a86:/usr# ls
```
output:
```
bin  games  include  lib  local  sbin  share  src  world_and_peace
```
### Объеденены и копирование файлов в контейнер
```
\4-4_hadoop\war_and_peace>type *.txt > war_and_peace.txt
docker cp war_and_peace.txt 92e57d2c1a86:/usr/world_and_peace/war_and_peace.txt
```
output:
```
Successfully copied 6.1MB to 92e57d2c1a86:/usr/world_and_peace/war_and_peace.txt
```
input:
```
root@92e57d2c1a86:/# hdfs dfs -put usr/world_and_peace/war_and_peace.txt /user/polar_jabka/war_and_peace.txt
```
### Проверка наличия файла
```
root@92e57d2c1a86:/# hdfs dfs -ls /user/polar_jabka
```
output:
```
Found 1 items
-rw-r--r--   3 root polar_jabka    6096456 2023-08-13 09:48 /user/polar_jabka/war_and_peace
```
### Изменение режима доступа
```
root@92e57d2c1a86:/# hdfs dfs -chmod 755 /user/polar_jabka/war_and_peace.txt
```
### Проверка результата
```
root@92e57d2c1a86:/# hdfs dfs -ls /user/polar_jabka
```
output:
```
Found 1 items
-rwxr-xr-x   3 root polar_jabka    6096456 2023-08-13 09:48 /user/polar_jabka/war_and_peace.txt
```
### Вывод информации о занимаемом объеме памяти
```
root@92e57d2c1a86:/# hdfs dfs -du -h /user/polar_jabka/war_and_peace.txt
```
output:
```
5.8 M  /user/polar_jabka/war_and_peace.txt
```
### Изменение параметров репликации
```
root@92e57d2c1a86:/# hdfs dfs -setrep 2 /user/polar_jabka/war_and_peace.txt
```
output:
```
Replication 2 set: /user/polar_jabka/war_and_peace.txt
```
input:
```
root@92e57d2c1a86:/# hdfs dfs -du -h /user/polar_jabka/war_and_peace.txt
```
output:
```
5.8 M  /user/polar_jabka/war_and_peace.txt
```
### Подчет количества строк в файле
```
root@92e57d2c1a86:/# hdfs dfs -cat /user/polar_jabka/war_and_peace.txt | wc -l
```
output:
```
20557
```
### Интерфейс Hue после выполнения задачи
![hue_screen.PNG](https://github.com/PolarJaba/DE_1st_work_with_hadoop/blob/main/hue_screen.PNG)
