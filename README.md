ProcfsBasedProcessTree implementation is based on RSS.  SMAPBasedProcessTree's implementation is based on similar lines, however it tries to compute the 
RSS based on /proc/<pid>/smaps, which gives a more accurate value of a process's memory.

Private Pages : Pages that were mapped only by the process
Shared Pages : Pages that were shared with other processes

Clean Pages : Pages that have not been modified since they were mapped
Dirty Pages : Pages that have been modified since they were mapped

Private RSS = Private Clean Pages + Private Dirty Pages
Shared RSS = Shared Clean Pages + Shared Dirty Pages
RSS = Private RSS + Shared RSS
PSS = The count of all pages mapped uniquely by the process, 
 plus a fraction of each shared page, said fraction to be 
 proportional to the number of processes which have mapped the page. 
 For example, if a process mapped 1 page that was unmapped by any 
 other process (USS=1), and it mapped a second page which was also 
 mapped by one other process, the PSS value reported would be 1.5. 
 If there were three users of the shared page, the reported PSS 
 would be 1.33.

mvn eclipse:eclipse;

mvn clean package;

Edit yarn-site.xml:
------------------
set yarn.nodemanager.container-monitor.process-tree.class=org.apache.hadoop.yarn.util.SmapsBasedProcessTree

Edit mapred-site.xml:
--------------------
set mapreduce.job.process-tree.class=org.apache.hadoop.yarn.util.SmapsBasedProcessTree
 
Ensure to place the target/smap*.jar in the begining of the classpath 

Refer https://issues.apache.org/jira/browse/YARN-1747 for more details
