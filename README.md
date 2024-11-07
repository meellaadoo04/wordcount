# Mapreduce

Lo primero de todo que debemos de hacer es clonar el repositorio
```bash
git clone 

1. Ejecutamos el Clúster de Hadoop
Ejecuta el comando docker-compose para levantar el cluster con los siguientes contenedores:namenode, datanode1, datanode2, datanode3, resourcemanager, nodemanager, y historyserver

```bash 
docker-compose up -d 
```

2. Metemos un archivo creado a  HDFS
Primero, copiamos el archivo archivo.txt al contenedor del NameNode:

```bash 
docker cp archivo.txt namenode:/archivo.txt
```

Luego, creamos el directorio dentro de HDFS:

```bash 
docker exec -it namenode hdfs dfs -mkdir -p /user/dani
```

Una vez que hemos creado la ruta en HDFS lo subimos

```bash 
docker exec -it namenode hdfs dfs -put /archivo.txt /user/dani/
```

3. Ejecutamos  WordCount
Ejecutamos WordCount mediante yarn que se encuentra en el contenedor resourcemanager. Establecemos primero la ruta donde tenemos guardado al archivo txt y en que ruta queremos guardarla

```bash 
docker exec -it resourcemanager yarn jar /opt/hadoop-3.2.1/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.2.1.jar wordcount /user/dani/archivo.txt /user/dani/output
```

4. Comprobar el funcionamiento
Para verificar los resultados ejecutamos un ls para comprobar que se a generado el archivo en la salida correspondiente:
```bash 
docker exec -it namenode hdfs dfs -ls /user/dani/output
```
Y luego para acceder al contenido realizamos un cat para comprobar que se a escrito:
```bash 
docker exec -it namenode hdfs dfs -cat /user/dani/output/part-r-00000
```

