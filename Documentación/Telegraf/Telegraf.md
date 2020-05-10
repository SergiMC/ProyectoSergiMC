# Telegraf

Telegraf es un servicio que tiene la función de recopilar,enviar métricas y datos de diferentes sistemas. Telegraf nos permite recopila datos del sistema el cual se está usando, datos como CPU,MEM,DISK. Puede recopilar datos tanto como INPUTS o OUTPUTS, un ejemplo son las base de datos o servicios. Telegraf proporciona una gran lista de plugins de entrada, un ejemplo sería Apache, dockers.

## Características de Telegraf

* Escrito en Go (lenguaje de programación). Se compila en un único binario sin dependencias externas.
* Consumo mínimo de memoria.
* Sistema de plugin que permite una fácil inserción de nuevos inputs y outputs.
* Gran numero de plugins para la mayoría de los servicios mas populares y APIs.

## Uso de telegraf

* Utilizaremos telegraf con los plugins **CPU**,**MEM**,**DISK** para la visualización de los datos.

Debemos tener en cuenta que para el uso de telegraf, necesitaremos tener configurado y usando al mismo tiempo la base de datos influxDB por tal que recopile los datos del servicio telegraf y los pueda mostrar como (**OUTPUT**).

En primer lugar deberemos tener instalado tanto influxDB como telegraf. Una vez hayamos instalado y configurado influxDB, pasaremos a activarlo, para todos estos pasos podemos consultar la documentación de influxDB: (link).

Activado influxDB, configuraremos telegraf creando el fichero de configuración con los plugins en este caso **CPU**,**RAM**,**DISK** el cual pasará los datos a influxDB.

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

* **MEM**

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
