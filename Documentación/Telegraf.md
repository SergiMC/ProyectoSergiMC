# Telegraf

Telegraf es un servicio que tiene la función de recopilar,enviar métricas y datos de diferentes sistemas. Telegraf nos permite recopila datos del sistema el cual se está usando, datos como CPU,MEM,DISK. Puede recopilar datos tanto como INPUTS o OUTPUTS, un ejemplo son las base de datos o servicios. Telegraf proporciona una gran lista de plugins de entrada, un ejemplo sería Apache, dockers.

## Características de Telegraf

* Escrito en Go (lenguaje de programación). Se compila en un único binario sin dependencias externas.
* Consumo mínimo de memoria.
* Sistema de plugin que permite una fácil inserción de nuevos inputs y outputs.
* Gran numero de plugins para la mayoría de los servicios mas populares y APIs.

## Uso de telegraf

* Utilizaremos telegraf con los plugins **CPU**,**RAM(mem)**,**DISK** para la visualización de los datos.

Debemos tener en cuenta que para el uso de telegraf, necesitaremos tener configurado y usando al mismo tiempo la base de datos influxDB por tal que recopile los datos del servicio telegraf y los pueda mostrar como (**OUTPUT**).

En primer lugar deberemos tener instalado tanto influxDB como telegraf. Una vez hayamos instalado y configurado influxDB, pasaremos a activarlo, para todos estos pasos podemos consultar la documentación de influxDB: (link).

Activado influxDB, configuraremos telegraf creando el fichero de configuración con los plugins en este caso **CPU**,**RAM(mem)**,**DISK** el cual pasará los datos a influxDB.

```
[sergimc@192 Telegraf]$ ./telegraf -sample-config -input-filter cpu:mem:disk -output-filter influxDB > telegraf.conf

```
Ejecutamos telegraf con el fichero de configuración que hemos creado en /usr/bin/ de la carpeta telegraf:

```
[sergimc@192 bin]$ ./telegraf --config telegraf.conf
2020-05-07T15:44:30Z I! Starting Telegraf 1.14.2
2020-05-07T15:44:30Z I! Loaded inputs: cpu disk mem
2020-05-07T15:44:30Z I! Loaded aggregators: 
2020-05-07T15:44:30Z I! Loaded processors: 
2020-05-07T15:44:30Z I! Loaded outputs: influxdb
2020-05-07T15:44:30Z I! Tags enabled: host=192.168.1.138
2020-05-07T15:44:30Z I! [agent] Config: Interval:10s, Quiet:false, Hostname:"192.168.1.138", Flush Interval:10s

```
Entramos en influxDB y confirmamos si se ha creado correctamente la base de datos.

```
[sergimc@192 influxdb-1.8.0-1]$ ./influx 
Connected to http://localhost:8086 version 1.8.0
InfluxDB shell version: 1.8.0
> show databases
name: databases
name
----
_internal
telegraf <--- base creada con nombre telegraf
```
Entramos dentro de la base telegraf y confirmamos si estan los measurements que hemos configurado.

```
> show measurements
name: measurements
name
----
cpu
disk
mem

```

## Visualización de datos CPU,MEM,DISK

* **CPU**

El plugin **CPU** ( Central Processing Unit) recoge las métricas del cpu estándar. Estas métricas generalmente se suelen encontrar en el directorio **/proc/cpuinfo**.

Resultado de consulta en cpuinfo.

```
[sergimc@192 proc]$ cat /proc/cpuinfo 
processor	: 0
vendor_id	: GenuineIntel
cpu family	: 6
model		: 58
model name	: Intel(R) Core(TM) i5-3470 CPU @ 3.20GHz
stepping	: 9
microcode	: 0x1c
cpu MHz		: 3192.574
cache size	: 6144 KB
physical id	: 0
siblings	: 4
core id		: 0
cpu cores	: 4
apicid		: 0
initial apicid	: 0
fpu		: yes
fpu_exception	: yes
cpuid level	: 13
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx rdtscp lm constant_tsc arch_perfmon pebs bts rep_good nopl xtopology nonstop_tsc cpuid aperfmperf pni pclmulqdq dtes64 monitor ds_cpl vmx smx est tm2 ssse3 cx16 xtpr pdcm pcid sse4_1 sse4_2 x2apic popcnt tsc_deadline_timer aes xsave avx f16c rdrand lahf_lm cpuid_fault epb tpr_shadow vnmi flexpriority ept vpid fsgsbase smep erms xsaveopt dtherm ida arat pln pts
bugs		:
bogomips	: 6385.14
clflush size	: 64
cache_alignment	: 64
address sizes	: 36 bits physical, 48 bits virtual
power management:

```
Este es el resultado de una consulta a la base de datos influxdb con los datos del plugin cpu.
```
> select * from cpu;
name: cpu
time                cpu       host          usage_guest usage_guest_nice usage_idle         usage_iowait        usage_irq            usage_nice           usage_softirq        usage_steal usage_system        usage_user
----                ---       ----          ----------- ---------------- ----------         ------------        ---------            ----------           -------------        ----------- ------------        ----------
1588175570000000000 cpu-total 192.168.1.139 0           0                87.79260005034445  0.251698968034268   0.2265290712307804   0                    0.20135917442738221  0           1.5605336018121183  9.967279134154838
1588175570000000000 cpu0      192.168.1.139 0           0                87.43718592964916  0                   0.40201005025121833  0                    0.402010050251254    0           1.4070351758792998  10.351758793968818
1588175570000000000 cpu1      192.168.1.139 0           0                86.89516129032761  0.5040322580647194  0.20161290322583764  0                    0.20161290322583764  0           1.814516129032503   10.38306451613173
1588175570000000000 cpu2      192.168.1.139 0           0                88.51963746223974  0.6042296072508712  0.1007049345418059   0                    0.1007049345418059   0           1.6112789526690374  9.063444108762495
1588175570000000000 cpu3      192.168.1.139 0           0                88.22937625754514  0                   0.20120724346075     0                    0.10060362173038392  0           1.4084507042252856  10.0

```

* **RAM**
La memoria RAM es la memoria principal de un dispositivo,se almacenan de forma temporal los datos de los programas que se están utilizando en este momento.

El plugin **RAM** ( Random Acces Memory) recoge las métricas de la mem. Estas métricas generalmente se suelen encontrar en el directorio **/proc/meminfo**.

Resultado de consulta en meminfo.

```
[sergimc@192 proc]$ cat /proc/meminfo 
MemTotal:        7993064 kB
MemFree:         3332348 kB
MemAvailable:    5005488 kB
Buffers:          180124 kB
Cached:          2164796 kB
SwapCached:            0 kB
Active:          2548916 kB
Inactive:        1733964 kB
Active(anon):    1943368 kB
Inactive(anon):   518612 kB
Active(file):     605548 kB
Inactive(file):  1215352 kB
Unevictable:          16 kB
Mlocked:              16 kB
SwapTotal:       5242876 kB
SwapFree:        5242876 kB
Dirty:             11156 kB
Writeback:             0 kB
AnonPages:       1938144 kB
Mapped:           731276 kB
Shmem:            524028 kB
Slab:             213868 kB
SReclaimable:     143160 kB
SUnreclaim:        70708 kB
KernelStack:       14720 kB
PageTables:        79348 kB
NFS_Unstable:          0 kB
Bounce:                0 kB
WritebackTmp:          0 kB
CommitLimit:     9239408 kB

```
Este es el resultado de una consulta a la base de datos influxdb con los datos del plugin mem.

```
> select * from mem;
name: mem
time                active     available  available_percent  buffered  cached     commit_limit committed_as dirty    free       high_free high_total host          huge_page_size huge_pages_free huge_pages_total inactive   low_free low_total mapped    page_tables shared    slab      sreclaimable sunreclaim swap_cached swap_free  swap_total total      used       used_percent       vmalloc_chunk vmalloc_total  vmalloc_used wired write_back write_back_tmp
----                ------     ---------  -----------------  --------  ------     ------------ ------------ -----    ----       --------- ---------- ----          -------------- --------------- ---------------- --------   -------- --------- ------    ----------- ------    ----      ------------ ---------- ----------- ---------  ---------- -----      ----       ------------       ------------- -------------  ------------ ----- ---------- --------------
1588175560000000000 2804760576 4893196288 59.783232062197925 102264832 2874912768 9461153792   11322458112  3190784  2775810048 0         0          192.168.1.139 2097152        0               0                2254749696 0        0         803504128 83210240    588853248 178483200 104693760    73789440   0           5368705024 5368705024 8184897536 2431909888 29.71216044310417  0             35184372087808 0            0     0          0
1588175570000000000 2803679232 4894461952 59.7986954689716   102289408 2860335104 9461153792   11332362240  6381568  2791559168 0         0          192.168.1.139 2097152        0               0                2240307200 0        0         803872768 83226624    588853248 178348032 104558592    73789440   0           5368705024 5368705024 8184897536 2430713856 29.69754777391999  0             35184372087808 0            0     0          0
1588175580000000000 2812022784 4886130688 59.69690721855849  102436864 2863190016 9461153792   11341574144  8228864  2780291072 0         0          192.168.1.139 2097152        0               0                2243162112 0        0         804036608 83271680    588918784 178343936 104558592    73785344   0           5368705024 5368705024 8184897536 2438979584 29.798535330131223 0             35184372087808 0            0     0          0
1588175590000000000 2816708608 4881776640 59.64371109752155  102486016 2864705536 9461153792   11341574144  8282112  2774306816 0         0          192.168.1.139 2097152        0               0                2244677632 0        0         804036608 83292160    588853248 178343936 104558592    73785344   0           5368705024 5368705024 8184897536 2443399168 29.852532145370034 0             35184372087808 0            0     0          0
1588175600000000000 2816270336 4882448384 59.65191821309075  102510592 2865987584 9461153792   11341574144  6266880  2773671936 0         0          192.168.1.139 2097152        0               0                2245955584 0        0         804298752 83283968    588853248 178348032 104558592    73789440   0           5368705024 5368705024 8184897536 2442727424 29.84432502980084  0             35184372087808 0            0     0          0

```

* **DISK**

La unidad de Disco Duro ( HDD "Hard Disk Drive") es un dispositivo magnético que almacena todos los programas y datos del sistema.

El plugin **DISK** recopila las métricas del disco duro de nuestro sistema. Estas métricas generalmente se suelen encontrar en el directorio **/proc/self/mounts** y **/proc/diskstats**. Los datos que recopila, contienen los dispositivos que están conectados en nuestro sistema y la información de cada disco.

Resultado de consulta en mounts.

```
[sergimc@192 proc]$ cat /proc/self/mounts
sysfs /sys sysfs rw,seclabel,nosuid,nodev,noexec,relatime 0 0
proc /proc proc rw,nosuid,nodev,noexec,relatime 0 0
devtmpfs /dev devtmpfs rw,seclabel,nosuid,size=3984368k,nr_inodes=996092,mode=755 0 0
securityfs /sys/kernel/security securityfs rw,nosuid,nodev,noexec,relatime 0 0
tmpfs /dev/shm tmpfs rw,seclabel,nosuid,nodev 0 0
devpts /dev/pts devpts rw,seclabel,nosuid,noexec,relatime,gid=5,mode=620,ptmxmode=000 0 0
tmpfs /run tmpfs rw,seclabel,nosuid,nodev,mode=755 0 0
tmpfs /sys/fs/cgroup tmpfs ro,seclabel,nosuid,nodev,noexec,mode=755 0 0

```

Resultado de consulta en diskstats.

```
[sergimc@192 proc]$ cat /proc/diskstats
   8       0 sda 238 0 17090 1765 0 0 0 0 0 772 1764
   8       1 sda1 51 0 4216 586 0 0 0 0 0 443 585
   8       2 sda2 46 0 4234 420 0 0 0 0 0 417 420
   8       3 sda3 47 0 4184 514 0 0 0 0 0 506 514
  11       0 sr0 0 0 0 0 0 0 0 0 0 0 0
   8      16 sdb 51289 17950 2709764 1789691 29800 33005 1911504 23808841 0 366823 25685770
   8      17 sdb1 117 19 8802 3867 3 0 24 67 0 3467 3933
   8      18 sdb2 50 0 4456 2268 0 0 0 0 0 2246 2268
   8      19 sdb3 51093 17931 2694386 1783207 29100 33005 1911480 23783026 0 351005 25653412

```

Este es el resultado de una consulta a la base de datos influxdb con los datos del plugin disk.

```
> select * from disk;
name: disk
time                device free         fstype  host          inodes_free inodes_total inodes_used mode path                         total        used        used_percent
----                ------ ----         ------  ----          ----------- ------------ ----------- ---- ----                         -----        ----        ------------
1588175560000000000 sdb1   804511744    ext4    192.168.1.139 65090       65536        446         rw   /boot                        1023303680   148328448   15.566980617039295
1588175560000000000 sdb3   433577189376 ext4    192.168.1.139 29728044    30138368     410324      rw   /                            484840685568 26563502080 5.772908715364087
1588175560000000000 sdb3   433577189376 ext4    192.168.1.139 29728044    30138368     410324      rw   /var/lib/docker/overlay2     484840685568 26563502080 5.772908715364087
1588175560000000000 sdb3   433577189376 ext4    192.168.1.139 29728044    30138368     410324      rw   /var/lib/docker/containers   484840685568 26563502080 5.772908715364087
1588175560000000000 sdc1   27295731712  fuseblk 192.168.1.139 26714426    26721524     7098        rw   /run/media/sergimc/sergilolg 31328301056  4032569344  12.871969459153554

```
