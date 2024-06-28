**Read, Filter, map & reduce**   
val textFile = sc.textFile("hdfs://ybolcldrmstr.yotabites.com:8020/user/pratap/Read.md")    
textFile.count()   
textFile.filter(line => line.contains("Spark")).count() // How many lines contain "Spark"?    
textFile.collect().foreach(println)   
textFile.map(s=>s.length).collect     
textFile.map(s=>s.length).reduce(_+_)     


**Visualize Partition**    
val distData = sc.parallelize(1 to 50, 10)    
distData.glom().collect()(4)    
distData.glom().collect()(5)    


**Map**    
sc.parallelize(List(1,2,3)).map(x=>List(x,x,x)).collect   

**Flat Map**    
sc.parallelize(List(1,2,3)).flatMap(x=>List(x,x,x)).collect   

**Basic groupBy example in scala**    
scala> val x = sc.parallelize(Array("Joseph", "Jimmy", "Tina","Thomas", "James", "Cory","Christine", "Jackeline", "Juan"), 3)   
   
scala> val y = x.groupBy(word => word.charAt(0))    
scala> y.collect     


// Another short syntax   
 scala> val y = x.groupBy(_.charAt(0))     


**Distinct**  
val rdd1 = sc.parallelize(Seq((1,"jan",2016),(3,"nov",2014),(16,"feb",2014),(3,"nov",2014)))     
rdd1.distinct().collect     

**GroupByKey**  
val data = sc.parallelize(Array(('k',5),('s',3),('s',4),('p',7),('p',5),('t',8),('k',6)),3)     
val group = data.groupByKey().collect()    
group.foreach(println)     

**Map Reduce**   
val words = Array("one","two","two","four","five","six","six","eight","nine","ten")    
val data = sc.parallelize(words).map(w => (w,1)).reduceByKey(_+_)     
data.foreach(println)    


**Sort**    
val data = sc.parallelize(Seq(("maths",52), ("english",75), ("science",82), ("computer",65), ("maths",85)))     
val sorted = data.sortByKey()     
sorted.foreach(println)    
val sorted1 = data.sortBy(_._2)    
val sorted2 = data.sortBy(_._2,false)    
val sorted3 = data.sortBy(_._1)    


**Join**     
val data = sc.parallelize(Array(('A',1),('b',2),('c',3)))     
val data2 =sc.parallelize(Array(('A',4),('A',6),('b',7),('c',3),('c',8)))    
val result = data.join(data2)     
println(result.collect().mkString(","))    

**Coalesce**    
val rdd1 = spark.sparkContext.parallelize(Array("jan","feb","mar","april","may","jun"),3)    
val result = rdd1.coalesce(2)    
result.foreach(println)    


**Take**    
val data = sc.parallelize(Array(('k',5),('s',3),('s',4),('p',7),('p',5),('t',8),('k',6)),3)     
val group = data.groupByKey().collect()    
val twoRec = result.take(2)     
twoRec.foreach(println)     


**Top**
val data = sc.textFile("hdfs://ybolcldrmstr.yotabites.com:8020/user/pratap/Read.md")   
val mapFile = data.map(line => (line,line.length))   
val res = mapFile.top(3)   
res.foreach(println)   


**Read JSON**    
val sqlContext = new org.apache.spark.sql.SQLContext(sc)    
val path = "hdfs://ybolcldrmstr.yotabites.com:8020/user/pratap/git.json"    
val git = sqlContext.read.json(path)    
git.show(5)
git.printSchema()     
git.registerTempTable("git")    
sqlContext.sql("SELECT * from git limit 10").collect.foreach(println)    
git.select("Contributors").show(5)     


**Read & Write Avro**    
import com.databricks.spark.avro._    
val sqlContext = new org.apache.spark.sql.SQLContext(sc)   
val path = "hdfs://ybolcldrmstr.yotabites.com:8020/user/pratap/git.avro"    
val df = sqlContext.read.avro(path)    
df.show(5)     
df.printSchema()     
df.registerTempTable("git")    
sqlContext.sql("SELECT * from git limit 10").collect.foreach(println)    
df.select("Contributors").show(5)     

Write Avro file     
import com.databricks.spark.avro._    
df.write.format("com.databricks.spark.avro").save("/user/pratap/tmp/avro")     

**Convert Json to Parquet**    
val sqlContext = new org.apache.spark.sql.SQLContext(sc)    
val path = "hdfs://ybolcldrmstr.yotabites.com:8020/user/pratap/git.json"    
val git = sqlContext.read.json(path)    
git.write.parquet("hdfs://ybolcldrmstr.yotabites.com:8020/user/pratap/git_parquet/")    


**Read Parquet**
val sqlContext = new org.apache.spark.sql.SQLContext(sc)    
val path = "hdfs://ybolcldrmstr.yotabites.com:8020/user/pratap/git_parquet/part-r-00000-8c66790c-1525-44f7-b3a4-bd9c0ac89bfd.gz.parquet"    
val git = sqlContext.read.parquet(path)    
git.printSchema()   
git.show(5)  
git.registerTempTable("git")    
sqlContext.sql("SELECT * from git limit 10").collect.foreach(println)    
git.select("Contributors").show(5)     
git.select("*").show(5)     


**Read XML**     
$SPARK_HOME/bin/spark-shell --packages com.databricks:spark-xml_2.10:0.3.5     

val sqlContext = new org.apache.spark.sql.SQLContext(sc)    
val df = sqlContext.read.format("com.databricks.spark.xml").option("rowTag", "Transaction").load("hdfs://ybolcldrmstr.yotabites.com:8020/user/pratap/samplexml.xml")    
df.printSchema()     
df.show(5)   
val flattened = df.withColumn("LineItem", explode($"RetailTransaction.LineItem"))     

val selectedData = flattened.select($"RetailStoreID",$"WorkstationID", $"SequenceNumber",$"BusinessDayDate",$"OperatorID.@OperatorName" as "OperatorName",$"OperatorID.#VALUE" as "OperatorID",$"CurrencyCode",$"RetailTransaction.ReceiptDateTime",$"RetailTransaction.TransactionCount",$"LineItem.SequenceNumber" as "LineItemSequenceNumber",$"LineItem.Tax.TaxableAmount", $"LineItem.Tax.Amount" as "TaxAmount",$"LineItem.Tax.Percent" as "TaxPercent",$"LineItem.Sale.POSIdentity.@POSIDType" as "POSIDType",$"LineItem.Sale.POSIdentity.POSItemID" as "POSItemID" ,$"LineItem.Sale.Description",$"LineItem.Sale.RegularSalesUnitPrice", $"LineItem.Sale.ExtendedAmount", $"LineItem.Sale.DiscountAmount", $"LineItem.Sale.ExtendedDiscountAmount", $"LineItem.Sale.Quantity")    

selectedData.show(3,false)    
selectedData.write.format("com.databricks.spark.csv").option("header", "true").mode("overwrite").save("POSLog-201409300635-21_lines")    

Write XML:    
df.write.format("com.databricks.spark.xml").option("rowTag", "Transaction") .save("/user/pratap/newtrans.xml")



**Read CSV**      
spark-shell --packages com.databricks:spark-csv_2.10:1.5.0    
val path = "hdfs://ybolcldrmstr.yotabites.com:8020/user/pratap/git.csv"    
val git = sqlContext.read.format("csv").option("header", "true").option("inferSchema", "true").load(path)    
git.printSchema()   
git.show(5)  
git.registerTempTable("git")    
sqlContext.sql("SELECT * from git limit 10").collect.foreach(println)    
git.select("Contributors").show(5)     
git.select("*").show(5)     

write csv:      
git.repartition(1).write.format("com.databricks.spark.csv").option("header", "true").save("/user/pratap/tmp/csv")     

write compressed     
git.repartition(1).write.format("com.databricks.spark.csv").option("header", "true").option("codec", "org.apache.hadoop.io.compress.GzipCodec").save("/user/pratap/tmp/csv/compressed4")     

git.repartition(1).write.format("com.databricks.spark.csv").option("header", "true").option("codec", "org.apache.hadoop.io.compress.SnappyCodec").save("/user/pratap/tmp/csv/compressed3")     

 git.repartition(1).write.format("com.databricks.spark.csv").option("header", "true").option("codec", "org.apache.hadoop.io.compress.BZip2Codec").save("/user/pratap/tmp/csv/compressed2")     
 
 git.repartition(1).write.format("com.databricks.spark.csv").option("header", "true").option("codec", "org.apache.hadoop.io.compress.Lz4Codec").save("/user/pratap/tmp/csv/compressed1")      
 
 git.repartition(1).write.format("com.databricks.spark.csv").option("header", "true").option("codec","org.apache.hadoop.io.compress.DefaultCodec").save("/user/pratap/tmp/csv/compressed")      
 
 
 

**Json to ORC**
import org.apache.spark.sql.hive.orc._    
import org.apache.spark.sql._    
val hiveContext = new org.apache.spark.sql.hive.HiveContext(sc)    
val path = "hdfs://ybolcldrmstr.yotabites.com:8020/user/pratap/git.json"    
val git = hiveContext.read.json(path)    

Write ORC file:
hiveContext.createDataFrame(git.rdd,git.schema).write.orc("hdfs://ybolcldrmstr.yotabites.com:8020/user/pratap/orc")    


**Read ORC**   
Read ORC file:    
val hiveContext = new org.apache.spark.sql.hive.HiveContext(sc)  
val path = "hdfs://ybolcldrmstr.yotabites.com:8020/user/pratap/orc/part-r-00000-66b2a61c-6d72-410c-a8e0-cc9dff59486e.orc"     
val git = hiveContext.read.orc(path)    


**Sequence File**     
Writing a Sequence file:     
val data = sc.parallelize(List(("key1", 1), ("Kay2", 2), ("Key3", 2)))    
data.repartition(1).saveAsSequenceFile("/user/pratap/tmp/seq")     
data.coalesce(1).saveAsSequenceFile("/user/pratap/tmp/seq")     

Check file at hdfs:     
hdfs dfs -text /user/pratap/tmp/seq-output/part-00000     

Reading from Sequence file:     
import org.apache.hadoop.io.Text     
import org.apache.hadoop.io.IntWritable     
val result = sc.sequenceFile("/user/pratap/tmp/seq/part-00000", classOf[Text], classOf[IntWritable])     
result.collect.foreach(println)      


**Save Dataframe as Sequence File**     
import org.apache.spark.sql.functions.{collect_list, collect_set}
val sqlContext = new org.apache.spark.sql.SQLContext(sc)    
val path = "hdfs://ybolcldrmstr.yotabites.com:8020/user/pratap/git.json"    
val git = sqlContext.read.json(path)   

val gitArray = git.collect.map(r => Map(git.columns.zip(r.toSeq):_*))   
val indexedgit = Map(gitArray.zipWithIndex.map(r => (r._2, r._1)):_*)    
val flatgit=1 to indexedgit.size flatMap(indexedgit.get)    
val seqgit=flatgit.toSeq     

**Merge files in HDFS**
hdfs dfs -getmerge /user/pratap/tmp/avro/part* /home/pratap/merge_avro/merged.avro      


**Read Compressed Files**    
val git = sqlContext.read.format("com.databricks.spark.csv").option("header", "true").option("codec","org.apache.hadoop.io.compress.DefaultCodec").load("/user/pratap/tmp/csv/compressed/part-00000.deflate")      

val git1 = sqlContext.read.format("com.databricks.spark.csv").option("header", "true").option("codec","org.apache.hadoop.io.compress.GzipCodec").load("/user/pratap/tmp/csv/compressed4/part-00000.gz")     

val git2= sqlContext.read.format("com.databricks.spark.csv").option("header", "true").option("codec","org.apache.hadoop.io.compress.Lz4Codec").load("/user/pratap/tmp/csv/compressed1/part-00000.lz4")      

val git3= sqlContext.read.format("com.databricks.spark.csv").option("header", "true").option("codec","org.apache.hadoop.io.compress.SnappyCodec").load("/user/pratap/tmp/csv/compressed3/part-00000.snappy")      

val git4= sqlContext.read.format("com.databricks.spark.csv").option("header", "true").option("codec","org.apache.hadoop.io.compress.BZip2Codec").load("/user/pratap/tmp/csv/compressed2/part-00000.bz2")      

**LZO Compression**
Download hadoop-lzo-0.4.20-SNAPSHOT.jar and save it    
jar -tf .ivy2/jars/hadoop-lzo-0.4.20-SNAPSHOT.jar      

spark-shell --packages com.databricks:spark-csv_2.10:1.5.0    --jars .ivy2/jars/hadoop-lzo-0.4.20-SNAPSHOT.jar       
import com.hadoop.compression.lzo.LzoCodec      

val path = "hdfs://ybolcldrmstr.yotabites.com:8020/user/pratap/git.csv"    
val git = sqlContext.read.format("csv").option("header", "true").option("inferSchema", "true").load(path)    

git.repartition(1).write.format("com.databricks.spark.csv").option("header", "true").option("codec","com.hadoop.compression.lzo.LzoCodec").save("/user/pratap/tmp/csv/compressed10")     



**Write to HDFS in Multiple Split**    
git.repartition(5).write.format("com.databricks.spark.csv").option("header", "true").option("codec","org.apache.hadoop.io.compress.DefaultCodec").save("/user/pratap/tmp/csv/compressed5")    


**Read Multiple Files Then Save as Single File**    
val git = sqlContext.read.format("com.databricks.spark.csv").option("header", "true").option("codec","org.apache.hadoop.io.compress.DefaultCodec").load("/user/pratap/tmp/csv/compressed5/*.deflate")      

git.coalesce(1).write.format("com.databricks.spark.csv").option("header", "true").option("codec","org.apache.hadoop.io.compress.DefaultCodec").save("/user/pratap/tmp/csv/compressed8")       






