
[TOC]

#### 命令汇总
```
java -jar jvm-demo-0.0.1.jar
java -jar -XX:+UseSerialGC jvm-demo-0.0.1.jar
java -jar -XX:+UseParNewGC jvm-demo-0.0.1.jar
java -jar -XX:+UseConcMarkSweepGC jvm-demo-0.0.1.jar
java -jar -XX:+UseParallelGC jvm-demo-0.0.1.jar
java -jar -XX:+UseParallelOldGC jvm-demo-0.0.1.jar
java -jar -XX:+UseG1GC -XX:MaxGCPauseMillis=50 jvm-demo-0.0.1.jar

jps
jmap -heap pid

```

```
-Xloggc:gc.log

java -XX:+UseSerialGC -Xmx512m -Xms512m -XX:+PrintGCDetails -XX:+PrintGCDateStamps GcAnalysis
java -XX:+UseParNewGC -Xmx512m -Xms512m -XX:+PrintGCDetails -XX:+PrintGCDateStamps GcAnalysis
java -XX:+UseConcMarkSweepGC -Xmx512m -Xms512m -XX:+PrintGCDetails -XX:+PrintGCDateStamps GcAnalysis
java -XX:+UseParallelGC -Xmx512m -Xms512m -XX:+PrintGCDetails -XX:+PrintGCDateStamps GcAnalysis
java -XX:+UseG1GC -Xmx512m -Xms512m -XX:+PrintGC -XX:+PrintGCDateStamps GcAnalysis

java -XX:+UseG1GC -Xmx512m -Xms512m -XX:+PrintGCDetails -XX:+PrintGCDateStamps GcAnalysis

```

#### 疑问

```
-XX:+UseParallelGC
-XX:+UseParallelOldGC
-XX:+UseParallelGC -XX:+UseParallelOldGC

貌似3个命令没啥区别，新生代使用的都是 Parallel Scavenge，老年代使用的是 Serial Old 和 Parallel Old；
使用 jmap -heap 查看也没啥区别；
打印 GC 日志，也没啥区别；

```

#### 1. 不指定GC，默认
```
java -jar jvm-demo-0.0.1.jar

Attaching to process ID 84089, please wait...
Debugger attached successfully.
Server compiler detected.
JVM version is 25.131-b11

using thread-local object allocation.
Parallel GC with 4 thread(s)

Heap Configuration:
   MinHeapFreeRatio         = 0
   MaxHeapFreeRatio         = 100
   MaxHeapSize              = 2147483648 (2048.0MB)
   NewSize                  = 44564480 (42.5MB)
   MaxNewSize               = 715653120 (682.5MB)
   OldSize                  = 89653248 (85.5MB)
   NewRatio                 = 2
   SurvivorRatio            = 8
   MetaspaceSize            = 21807104 (20.796875MB)
   CompressedClassSpaceSize = 1073741824 (1024.0MB)
   MaxMetaspaceSize         = 17592186044415 MB
   G1HeapRegionSize         = 0 (0.0MB)

Heap Usage:
PS Young Generation
Eden Space:
   capacity = 136314880 (130.0MB)
   used     = 128440704 (122.4906005859375MB)
   free     = 7874176 (7.5093994140625MB)
   94.22353891225961% used
From Space:
   capacity = 7340032 (7.0MB)
   used     = 0 (0.0MB)
   free     = 7340032 (7.0MB)
   0.0% used
To Space:
   capacity = 7864320 (7.5MB)
   used     = 0 (0.0MB)
   free     = 7864320 (7.5MB)
   0.0% used
PS Old Generation
   capacity = 52428800 (50.0MB)
   used     = 5760632 (5.493766784667969MB)
   free     = 46668168 (44.50623321533203MB)
   10.987533569335938% used

11778 interned Strings occupying 982176 bytes.
```

#### 2. 指定使用 Serial + Serial Old
```
java -jar -XX:+UseSerialGC jvm-demo-0.0.1.jar

Attaching to process ID 84109, please wait...
Debugger attached successfully.
Server compiler detected.
JVM version is 25.131-b11

using thread-local object allocation.
Mark Sweep Compact GC

Heap Configuration:
   MinHeapFreeRatio         = 40
   MaxHeapFreeRatio         = 70
   MaxHeapSize              = 2147483648 (2048.0MB)
   NewSize                  = 44695552 (42.625MB)
   MaxNewSize               = 715784192 (682.625MB)
   OldSize                  = 89522176 (85.375MB)
   NewRatio                 = 2
   SurvivorRatio            = 8
   MetaspaceSize            = 21807104 (20.796875MB)
   CompressedClassSpaceSize = 1073741824 (1024.0MB)
   MaxMetaspaceSize         = 17592186044415 MB
   G1HeapRegionSize         = 0 (0.0MB)

Heap Usage:
New Generation (Eden + 1 Survivor Space):
   capacity = 40370176 (38.5MB)
   used     = 15439648 (14.724395751953125MB)
   free     = 24930528 (23.775604248046875MB)
   38.24518377130682% used
Eden Space:
   capacity = 35913728 (34.25MB)
   used     = 13188480 (12.5775146484375MB)
   free     = 22725248 (21.6724853515625MB)
   36.72267050638686% used
From Space:
   capacity = 4456448 (4.25MB)
   used     = 2251168 (2.146881103515625MB)
   free     = 2205280 (2.103118896484375MB)
   50.5148494944853% used
To Space:
   capacity = 4456448 (4.25MB)
   used     = 0 (0.0MB)
   free     = 4456448 (4.25MB)
   0.0% used
tenured generation:
   capacity = 89522176 (85.375MB)
   used     = 9694864 (9.245742797851562MB)
   free     = 79827312 (76.12925720214844MB)
   10.829566966736822% used

11742 interned Strings occupying 978368 bytes.
```


#### 3. 指定使用 ParNew + Serial Old
```
java -jar -XX:+UseParNewGC jvm-demo-0.0.1.jar
Java HotSpot(TM) 64-Bit Server VM warning: Using the ParNew young collector with the Serial old collector is deprecated and will likely be removed in a future release

Attaching to process ID 84135, please wait...
Debugger attached successfully.
Server compiler detected.
JVM version is 25.131-b11

using parallel threads in the new generation.
using thread-local object allocation.
Mark Sweep Compact GC

Heap Configuration:
   MinHeapFreeRatio         = 40
   MaxHeapFreeRatio         = 70
   MaxHeapSize              = 2147483648 (2048.0MB)
   NewSize                  = 44695552 (42.625MB)
   MaxNewSize               = 715784192 (682.625MB)
   OldSize                  = 89522176 (85.375MB)
   NewRatio                 = 2
   SurvivorRatio            = 8
   MetaspaceSize            = 21807104 (20.796875MB)
   CompressedClassSpaceSize = 1073741824 (1024.0MB)
   MaxMetaspaceSize         = 17592186044415 MB
   G1HeapRegionSize         = 0 (0.0MB)

Heap Usage:
New Generation (Eden + 1 Survivor Space):
   capacity = 40370176 (38.5MB)
   used     = 17465408 (16.65631103515625MB)
   free     = 22904768 (21.84368896484375MB)
   43.26314554586039% used
Eden Space:
   capacity = 35913728 (34.25MB)
   used     = 13400848 (12.780044555664062MB)
   free     = 22512880 (21.469955444335938MB)
   37.3139987026688% used
From Space:
   capacity = 4456448 (4.25MB)
   used     = 4064560 (3.8762664794921875MB)
   free     = 391888 (0.3737335205078125MB)
   91.20627010569854% used
To Space:
   capacity = 4456448 (4.25MB)
   used     = 0 (0.0MB)
   free     = 4456448 (4.25MB)
   0.0% used
tenured generation:
   capacity = 89522176 (85.375MB)
   used     = 9835712 (9.38006591796875MB)
   free     = 79686464 (75.99493408203125MB)
   10.98690005032943% used

11800 interned Strings occupying 984888 bytes.
```

#### 4. 指定使用 ParNew + CMS + Serial Old
```
java -jar -XX:+UseConcMarkSweepGC jvm-demo-0.0.1.jar

Attaching to process ID 84143, please wait...
Debugger attached successfully.
Server compiler detected.
JVM version is 25.131-b11

using parallel threads in the new generation.
using thread-local object allocation.
Concurrent Mark-Sweep GC

Heap Configuration:
   MinHeapFreeRatio         = 40
   MaxHeapFreeRatio         = 70
   MaxHeapSize              = 2147483648 (2048.0MB)
   NewSize                  = 44695552 (42.625MB)
   MaxNewSize               = 348913664 (332.75MB)
   OldSize                  = 89522176 (85.375MB)
   NewRatio                 = 2
   SurvivorRatio            = 8
   MetaspaceSize            = 21807104 (20.796875MB)
   CompressedClassSpaceSize = 1073741824 (1024.0MB)
   MaxMetaspaceSize         = 17592186044415 MB
   G1HeapRegionSize         = 0 (0.0MB)

Heap Usage:
New Generation (Eden + 1 Survivor Space):
   capacity = 40239104 (38.375MB)
   used     = 12096120 (11.535758972167969MB)
   free     = 28142984 (26.83924102783203MB)
   30.060609699460503% used
Eden Space:
   capacity = 35782656 (34.125MB)
   used     = 8578632 (8.181221008300781MB)
   free     = 27204024 (25.94377899169922MB)
   23.974274016998628% used
From Space:
   capacity = 4456448 (4.25MB)
   used     = 3517488 (3.3545379638671875MB)
   free     = 938960 (0.8954620361328125MB)
   78.93030503216912% used
To Space:
   capacity = 4456448 (4.25MB)
   used     = 0 (0.0MB)
   free     = 4456448 (4.25MB)
   0.0% used
concurrent mark-sweep generation:
   capacity = 89522176 (85.375MB)
   used     = 8779648 (8.3729248046875MB)
   free     = 80742528 (77.0020751953125MB)
   9.807232567715959% used

11705 interned Strings occupying 975672 bytes.
```

#### 5. 指定使用 Parallel Scavenge + Serial Old
```
java -jar -XX:+UseParallelGC jvm-demo-0.0.1.jar

Attaching to process ID 84203, please wait...
Debugger attached successfully.
Server compiler detected.
JVM version is 25.131-b11

using thread-local object allocation.
Parallel GC with 4 thread(s)

Heap Configuration:
   MinHeapFreeRatio         = 0
   MaxHeapFreeRatio         = 100
   MaxHeapSize              = 2147483648 (2048.0MB)
   NewSize                  = 44564480 (42.5MB)
   MaxNewSize               = 715653120 (682.5MB)
   OldSize                  = 89653248 (85.5MB)
   NewRatio                 = 2
   SurvivorRatio            = 8
   MetaspaceSize            = 21807104 (20.796875MB)
   CompressedClassSpaceSize = 1073741824 (1024.0MB)
   MaxMetaspaceSize         = 17592186044415 MB
   G1HeapRegionSize         = 0 (0.0MB)

Heap Usage:
PS Young Generation
Eden Space:
   capacity = 136314880 (130.0MB)
   used     = 125126888 (119.3302993774414MB)
   free     = 11187992 (10.669700622558594MB)
   91.79253798264723% used
From Space:
   capacity = 7864320 (7.5MB)
   used     = 0 (0.0MB)
   free     = 7864320 (7.5MB)
   0.0% used
To Space:
   capacity = 7864320 (7.5MB)
   used     = 0 (0.0MB)
   free     = 7864320 (7.5MB)
   0.0% used
PS Old Generation
   capacity = 52953088 (50.5MB)
   used     = 6024152 (5.745079040527344MB)
   free     = 46928936 (44.754920959472656MB)
   11.376394139658107% used

11724 interned Strings occupying 977776 bytes.
```

#### 6. 指定使用 Parallel Scavenge + Parallel Old
```
java -jar -XX:+UseParallelOldGC jvm-demo-0.0.1.jar

Attaching to process ID 84232, please wait...
Debugger attached successfully.
Server compiler detected.
JVM version is 25.131-b11

using thread-local object allocation.
Parallel GC with 4 thread(s)

Heap Configuration:
   MinHeapFreeRatio         = 0
   MaxHeapFreeRatio         = 100
   MaxHeapSize              = 2147483648 (2048.0MB)
   NewSize                  = 44564480 (42.5MB)
   MaxNewSize               = 715653120 (682.5MB)
   OldSize                  = 89653248 (85.5MB)
   NewRatio                 = 2
   SurvivorRatio            = 8
   MetaspaceSize            = 21807104 (20.796875MB)
   CompressedClassSpaceSize = 1073741824 (1024.0MB)
   MaxMetaspaceSize         = 17592186044415 MB
   G1HeapRegionSize         = 0 (0.0MB)

Heap Usage:
PS Young Generation
Eden Space:
   capacity = 136314880 (130.0MB)
   used     = 128935800 (122.96276092529297MB)
   free     = 7379080 (7.037239074707031MB)
   94.58673917330228% used
From Space:
   capacity = 7340032 (7.0MB)
   used     = 0 (0.0MB)
   free     = 7340032 (7.0MB)
   0.0% used
To Space:
   capacity = 7864320 (7.5MB)
   used     = 0 (0.0MB)
   free     = 7864320 (7.5MB)
   0.0% used
PS Old Generation
   capacity = 57671680 (55.0MB)
   used     = 5727552 (5.46221923828125MB)
   free     = 51944128 (49.53778076171875MB)
   9.931307705965908% used

11722 interned Strings occupying 978240 bytes.
```

#### 7. 指定使用 G1
```
java -jar -XX:+UseG1GC -XX:MaxGCPauseMillis=50 jvm-demo-0.0.1.jar

Attaching to process ID 84303, please wait...
Debugger attached successfully.
Server compiler detected.
JVM version is 25.131-b11

using thread-local object allocation.
Garbage-First (G1) GC with 4 thread(s)

Heap Configuration:
   MinHeapFreeRatio         = 40
   MaxHeapFreeRatio         = 70
   MaxHeapSize              = 2147483648 (2048.0MB)
   NewSize                  = 1363144 (1.2999954223632812MB)
   MaxNewSize               = 1287651328 (1228.0MB)
   OldSize                  = 5452592 (5.1999969482421875MB)
   NewRatio                 = 2
   SurvivorRatio            = 8
   MetaspaceSize            = 21807104 (20.796875MB)
   CompressedClassSpaceSize = 1073741824 (1024.0MB)
   MaxMetaspaceSize         = 17592186044415 MB
   G1HeapRegionSize         = 1048576 (1.0MB)

Heap Usage:
G1 Heap:
   regions  = 2048
   capacity = 2147483648 (2048.0MB)
   used     = 60536304 (57.73191833496094MB)
   free     = 2086947344 (1990.268081665039MB)
   2.8189413249492645% used
G1 Young Generation:
Eden Space:
   regions  = 47
   capacity = 74448896 (71.0MB)
   used     = 49283072 (47.0MB)
   free     = 25165824 (24.0MB)
   66.19718309859155% used
Survivor Space:
   regions  = 9
   capacity = 9437184 (9.0MB)
   used     = 9437184 (9.0MB)
   free     = 0 (0.0MB)
   100.0% used
G1 Old Generation:
   regions  = 2
   capacity = 50331648 (48.0MB)
   used     = 1816048 (1.7319183349609375MB)
   free     = 48515600 (46.26808166503906MB)
   3.6081631978352866% used

11752 interned Strings occupying 979592 bytes.
```

#### 8. 指定使用 ZGC
```
切换到 jdk16

DanieldeMacBook-Pro:~ daniel$ java -version
java version "16.0.2" 2021-07-20
Java(TM) SE Runtime Environment (build 16.0.2+7-67)
Java HotSpot(TM) 64-Bit Server VM (build 16.0.2+7-67, mixed mode, sharing)

java -jar -XX:+UnlockExperimentalVMOptions -XX:+UseZGC jvm-demo-0.0.1.jar

使用 jmap 命令报错，连接不上；

DanieldeMacBook-Pro:~ daniel$ jmap -heap 69614
Error: -heap option used
Cannot connect to core dump or remote debug server. Use jhsdb jmap instead
DanieldeMacBook-Pro:~ daniel$ jhsdb jmap --pid 69614 --heap
Attaching to process ID 69614, please wait...
ERROR: attach: task_for_pid(69614) failed: '(os/kern) failure' (5)
Error attaching to process: Can't attach to the process. Could be caused by an incorrect pid or lack of privileges.
sun.jvm.hotspot.debugger.DebuggerException: Can't attach to the process. Could be caused by an incorrect pid or lack of privileges.
	at jdk.hotspot.agent/sun.jvm.hotspot.debugger.bsd.BsdDebuggerLocal$BsdDebuggerLocalWorkerThread.execute(BsdDebuggerLocal.java:170)
	at jdk.hotspot.agent/sun.jvm.hotspot.debugger.bsd.BsdDebuggerLocal.attach(BsdDebuggerLocal.java:284)
	at jdk.hotspot.agent/sun.jvm.hotspot.HotSpotAgent.attachDebugger(HotSpotAgent.java:643)
	at jdk.hotspot.agent/sun.jvm.hotspot.HotSpotAgent.setupDebuggerDarwin(HotSpotAgent.java:631)
	at jdk.hotspot.agent/sun.jvm.hotspot.HotSpotAgent.setupDebugger(HotSpotAgent.java:364)
	at jdk.hotspot.agent/sun.jvm.hotspot.HotSpotAgent.go(HotSpotAgent.java:329)
	at jdk.hotspot.agent/sun.jvm.hotspot.HotSpotAgent.attach(HotSpotAgent.java:139)
	at jdk.hotspot.agent/sun.jvm.hotspot.tools.Tool.start(Tool.java:187)
	at jdk.hotspot.agent/sun.jvm.hotspot.tools.Tool.execute(Tool.java:118)
	at jdk.hotspot.agent/sun.jvm.hotspot.tools.JMap.main(JMap.java:176)
	at jdk.hotspot.agent/sun.jvm.hotspot.SALauncher.runJMAP(SALauncher.java:331)
	at jdk.hotspot.agent/sun.jvm.hotspot.SALauncher.main(SALauncher.java:483)
Caused by: sun.jvm.hotspot.debugger.DebuggerException: Can't attach to the process. Could be caused by an incorrect pid or lack of privileges.
	at jdk.hotspot.agent/sun.jvm.hotspot.debugger.bsd.BsdDebuggerLocal.attach0(Native Method)
	at jdk.hotspot.agent/sun.jvm.hotspot.debugger.bsd.BsdDebuggerLocal$1AttachTask.doit(BsdDebuggerLocal.java:275)
	at jdk.hotspot.agent/sun.jvm.hotspot.debugger.bsd.BsdDebuggerLocal$BsdDebuggerLocalWorkerThread.run(BsdDebuggerLocal.java:145)

使用 jcmd

DanieldeMacBook-Pro:~ daniel$ jcmd 69614 GC.heap_info
69614:
 ZHeap           used 82M, capacity 82M, max capacity 2048M
 Metaspace       used 25638K, committed 25984K, reserved 1073152K
  class space    used 3137K, committed 3264K, reserved 1048576K

```
