**Create Remote Connection**    
sshfs pratap@10.1.1.19:/home/pratap /home/pratap16/remotecomp/ -o nonempty    
ssh pratap@10.1.1.19

#Hadoop fsck Commands 
hdfs fsck /user/pratap						Filesystem check on HDFS    
hdfs fsck /user/pratap -files 					Display files during check    
hdfs fsck /user/pratap -files -blocks 				Display files and blocks during check    
hdfs fsck /user/pratap -files -blocks -locations 		Display files, blocks and its location during check    
hdfs fsck /user/pratap -files -blocks -locations -racks 	Display network topology for data-node locations    
hdfs fsck -delete 						Delete corrupted files    
hdfs fsck -move 						Move corrupted files to /lost+found directory    

#Hadoop Filesystem Commands
hdfs dfs -ls /   
hdfs dfs -mkdir -p /user/pratap/mapreduce   
hdfs dfs -rm -r /user/pratap/mapreduce   
hdfs dfs -mkdir -p /user/pratap/mapreduce
hdfs dfs -put /home/pratap/mapper.py /user/pratap/mapreduce   
hdfs dfs -put /home/pratap/reducer.py /user/pratap/mapreduce    
hdfs dfs -copyFromLocal /home/pratap/books /user/pratap    
rm -rf mapper.py    
rm -rf reducer.py    
hdfs dfs -get /user/pratap/mapreduce/reducer.py /home/pratap     
hdfs dfs -get /user/pratap/mapreduce/mapper.py /home/pratap     
hdfs dfs -expunge      
hdfs dfs -du /user/pratap    


hdfs dfs -copyFromLocal <source> <destination> 			            Copy from local fileystem to HDFS    
hdfs dfs -copyFromLocal file1 data 					    e.g: Copies file1 from local FS to data dir in HDFS    
hdfs dfs -copyToLocal <source> <destination> 				    copy from hdfs to local filesystem    
hdfs dfs -copyToLocal data/file1 /var/tmp 			            e.g:Copies file1 from HDFS data directory to /var/tmp on local FS   
hdfs dfs -put <source> <destination> 					    Copy from remote location to HDFS    
hdfs dfs -get <source> <destination> 					    Copy from HDFS to remote directory    
hdfs distcp hdfs://192.168.0.8:8020/input hdfs://192.168.0.8:8020/output    Copy data from one cluster to another using the cluster URL    
hdfs dfs -mv file:///data/datafile /user/hduser/data 			    Move data file from the local directory to HDFS    
hdfs dfs -getmerge mydir bigfile 					    Merge files in mydir directory and download it as one big file   

hdfs groups vinayak pratap mitchell yesh harshal kaushik mani		    Returns the group information given one or more usernames.     

hadoop jar /opt/cloudera/parcels/CDH/jars/hadoop-streaming-2.6.0-cdh5.9.3.jar -file /home/pratap/mapper.py    -mapper /home/pratap/mapper.py -file /home/pratap/reducer.py   -reducer /home/pratap/reducer.py -input /user/pratap/books/* -output /user/pratap/books-output    




yarn application -kill application_1507660256800_0114    
(Running job: job_1507660256800_0114)    
yarn application -list    
yarn application -list -appStates FINISHED    
				  NEW   
                                  NEW_SAVING          
                                  SUBMITTED      
                                  ACCEPTED      
                                  RUNNING      
                                  FINISHED       
                                  FAILED       
                                  KILLED     

yarn node -list    
yarn node -status ybolcldr01.yotabites.com:8041    
yarn node -status ybolcldr02.yotabites.com:8041    
yarn node -status ybolcldr03.yotabites.com:8041        


mapred job -list all    
mapred job -list-active-trackers     
mapred hsadmin -getGroups vinayak pratap mitchell yesh harshal kaushik mani     



#Benchmarking MapReduce with TeraSort    
hadoop jar /opt/cloudera/parcels/CDH/share/doc/hadoop-0.20-mapreduce/examples/hadoop-examples-2.6.0-mr1-cdh5.9.3.jar \    
teragen -Dmapreduce.job.maps=100 100000 random-data    

hadoop jar /opt/cloudera/parcels/CDH/share/doc/hadoop-0.20-mapreduce/examples/hadoop-examples-2.6.0-mr1-cdh5.9.3.jar \terasort random-data sorted-data    


hadoop jar /opt/cloudera/parcels/CDH/share/doc/hadoop-0.20-mapreduce/examples/hadoop-examples-2.6.0-mr1-cdh5.9.3.jar TestDFSIO -write -nrFiles 10 -fileSize 1000    
time hadoop jar /opt/cloudera/parcels/CDH/share/doc/hadoop-0.20-mapreduce/examples/hadoop-examples-2.6.0-mr1-cdh5.9.3.jar    


sudo su hdfs     

yarn jar /opt/cloudera/parcels/CDH/share/doc/hadoop-0.20-mapreduce/examples/hadoop-examples-2.6.0-mr1-cdh5.9.3.jar pi 10 100    

yarn jar /opt/cloudera/parcels/CDH/lib/hadoop-mapreduce/hadoop-mapreduce-client-jobclient-2.6.0-cdh5.9.3-tests.jar TestDFSIO -write -nrFiles 10 -fileSize 1000    


yarn jar /opt/cloudera/parcels/CDH/lib/hadoop-mapreduce/hadoop-mapreduce-client-jobclient-2.6.0-cdh5.9.3-tests.jar TestDFSIO -read -nrFiles 10 -fileSize 1000     

yarn jar /opt/cloudera/parcels/CDH/lib/hadoop-mapreduce/hadoop-mapreduce-client-jobclient-2.6.0-cdh5.9.3-tests.jar TestDFSIO -clean     




