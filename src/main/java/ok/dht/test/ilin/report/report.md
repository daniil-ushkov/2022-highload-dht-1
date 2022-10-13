# Задание

Проведите нагрузочное тестирование с помощью [wrk2](https://github.com/giltene/wrk2) в **одно соединение**:
* `PUT` запросами на **стабильной** нагрузке (`wrk2` должен обеспечивать заданный с помощью `-R` rate запросов)
* `GET` запросами на **стабильной** нагрузке по **наполненной** БД

Приложите полученный консольный вывод `wrk2` для обоих видов нагрузки.

Отпрофилируйте приложение (CPU и alloc) под `PUT` и `GET` нагрузкой с помощью [async-profiler](https://github.com/Artyomcool/async-profiler).
Приложите SVG-файлы FlameGraph `cpu`/`alloc` для `PUT`/`GET` нагрузки.

**Объясните** результаты нагрузочного тестирования и профилирования и приложите **текстовый отчёт** (в Markdown).

# Отчет

Проведено нагрузочное тестирование с помощью [wrk2](https://github.com/outsinre/homebrew-wrk2) в одно соединение:
(использовался форк wrk2 потому что тестировалось все на mac m1).

### PUT запросами при стабильной нагрузке

Был использован [скрипт](../scripts/put.lua):


Нагрузочное тестирование запускалось следующей командой:
```
wrk2 -R 25000 -c 1 -t 1 -d 180 -s src/main/java/ok/dht/test/ilin/scripts/put.lua -L http://localhost:19234
```

Консольный вывод wrk2:
```
Running 3m test @ http://localhost:19234
  1 threads and 1 connections
  Thread calibration: mean lat.: 5.199ms, rate sampling interval: 30ms
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency     0.87ms    2.48ms  54.50ms   98.38%
    Req/Sec    25.44k     1.04k   31.00k    92.60%
  Latency Distribution (HdrHistogram - Recorded Latency)
 50.000%  635.00us
 75.000%    0.93ms
 90.000%    1.11ms
 99.000%    6.82ms
 99.900%   44.48ms
 99.990%   52.64ms
 99.999%   54.37ms
100.000%   54.53ms

  Detailed Percentile spectrum:
       Value   Percentile   TotalCount 1/(1-Percentile)

       0.022     0.000000            5         1.00
       0.155     0.100000       425234         1.11
       0.276     0.200000       851606         1.25
       0.396     0.300000      1277039         1.43
       0.515     0.400000      1700476         1.67
       0.635     0.500000      2126740         2.00
       0.695     0.550000      2340613         2.22
       0.754     0.600000      2550300         2.50
       0.814     0.650000      2763301         2.86
       0.874     0.700000      2977776         3.33
       0.933     0.750000      3189213         4.00
       0.963     0.775000      3296498         4.44
       0.992     0.800000      3400529         5.00
       1.022     0.825000      3508892         5.71
       1.051     0.850000      3614281         6.67
       1.080     0.875000      3720274         8.00
       1.094     0.887500      3772135         8.89
       1.109     0.900000      3827862        10.00
       1.123     0.912500      3880940        11.43
       1.137     0.925000      3933514        13.33
       1.150     0.937500      3984879        16.00
       1.157     0.943750      4010943        17.78
       1.165     0.950000      4040783        20.00
       1.173     0.956250      4065433        22.86
       1.197     0.962500      4090905        26.67
       1.330     0.968750      4117182        32.00
       1.506     0.971875      4130416        35.56
       1.817     0.975000      4143671        40.00
       2.157     0.978125      4156935        45.71
       2.671     0.981250      4170214        53.33
       3.527     0.984375      4183495        64.00
       4.183     0.985938      4190163        71.11
       4.819     0.987500      4196804        80.00
       5.627     0.989062      4203432        91.43
       7.275     0.990625      4210059       106.67
      10.231     0.992188      4216701       128.00
      12.167     0.992969      4220031       142.22
      14.071     0.993750      4223338       160.00
      15.919     0.994531      4226665       182.86
      17.855     0.995313      4229990       213.33
      19.807     0.996094      4233299       256.00
      21.231     0.996484      4234956       284.44
      23.743     0.996875      4236617       320.00
      27.023     0.997266      4238279       365.71
      30.319     0.997656      4239939       426.67
      33.823     0.998047      4241607       512.00
      35.647     0.998242      4242436       568.89
      37.855     0.998437      4243266       640.00
      40.351     0.998633      4244088       731.43
      42.911     0.998828      4244921       853.33
      44.671     0.999023      4245751      1024.00
      45.535     0.999121      4246171      1137.78
      46.335     0.999219      4246583      1280.00
      46.975     0.999316      4246992      1462.86
      47.679     0.999414      4247419      1706.67
      48.575     0.999512      4247826      2048.00
      49.023     0.999561      4248035      2275.56
      49.439     0.999609      4248254      2560.00
      49.823     0.999658      4248466      2925.71
      50.143     0.999707      4248658      3413.33
      50.591     0.999756      4248867      4096.00
      50.879     0.999780      4248970      4551.11
      51.167     0.999805      4249068      5120.00
      51.583     0.999829      4249174      5851.43
      52.031     0.999854      4249286      6826.67
      52.319     0.999878      4249382      8192.00
      52.511     0.999890      4249439      9102.22
      52.703     0.999902      4249487     10240.00
      52.927     0.999915      4249537     11702.86
      53.119     0.999927      4249587     13653.33
      53.183     0.999939      4249643     16384.00
      53.375     0.999945      4249663     18204.44
      53.631     0.999951      4249690     20480.00
      53.887     0.999957      4249715     23405.71
      54.079     0.999963      4249743     27306.67
      54.175     0.999969      4249774     32768.00
      54.207     0.999973      4249803     36408.89
      54.207     0.999976      4249803     40960.00
      54.239     0.999979      4249811     46811.43
      54.271     0.999982      4249824     54613.33
      54.303     0.999985      4249839     65536.00
      54.303     0.999986      4249839     72817.78
      54.335     0.999988      4249851     81920.00
      54.335     0.999989      4249851     93622.86
      54.367     0.999991      4249859    109226.67
      54.399     0.999992      4249869    131072.00
      54.399     0.999993      4249869    145635.56
      54.431     0.999994      4249876    163840.00
      54.431     0.999995      4249876    187245.71
      54.463     0.999995      4249884    218453.33
      54.463     0.999996      4249884    262144.00
      54.463     0.999997      4249884    291271.11
      54.463     0.999997      4249884    327680.00
      54.495     0.999997      4249890    374491.43
      54.495     0.999998      4249890    436906.67
      54.495     0.999998      4249890    524288.00
      54.495     0.999998      4249890    582542.22
      54.495     0.999998      4249890    655360.00
      54.527     0.999999      4249896    748982.86
      54.527     1.000000      4249896          inf
#[Mean    =        0.869, StdDeviation   =        2.481]
#[Max     =       54.496, Total count    =      4249896]
#[Buckets =           27, SubBuckets     =         2048]
----------------------------------------------------------
  4499986 requests in 3.00m, 287.53MB read
Requests/sec:  24999.93
Transfer/sec:      1.60MB
```

[Результаты профилирования `cpu`](cpu_post.html)
[Результаты профилирования `alloc`](alloc_post.html)

Выводы:
* Наблюдается резкий рост задержки между 90-ым перцентилем и 99-ым -- c 1.11ms до 6.82ms и задержки между 99-ым и 99.9-ым перцентилями с 6.82ms до 44.48ms. Это связанно с тем что RocksDB для части запросов мерджит таблицы в памяти чтобы не был быстрый поиск, и память сильно не раздувалась.
* На графике результатов профилирования cpu можно увидеть что, чтение запроса из сокета заняло 9.74% всего времени, на то чтобы записать ответ в сокет потребовалось 18.58%. Эти показатели оптимизировать не получится так как это часть сетевого взаимодействия.
* Ответы на запросы сервером заняли 24.2% времени, из них 20.52% отводится на RocksDb, а оставшиеся на логику кода, создание объектов и т.п. Понятно, что запись во внешнюю память очень долгая, можно пробовать оптимизировать работу с базой или же заиспользовать другую, надо анализировать.
* Касательно количества аллокаций - RocksDB не показывается на графиках потому что написан на C++.
* На SelectorThread тратиться 11.5%, что нельзя улучшить на уровне приложения, потому что это код фреймворка, нужно оптимизировать его.
* Большое количество аллокаций тратиться на создание массивов байт, создании внутренних сущностей и копировании их, это можно улучшить избавившись от лишних копирований и созданий новых объектов.
* При этом 8.32% тратиться на создание масива байт в Response.
### GET запросами на стабильной нагрузке по наполненной БД

Был использован [скрипт](../scripts/get.lua)

Нагрузочное тестирование запускалось следующей командой:
```
wrk2 -R 22000 -c 1 -t 1 -d 180 -s src/main/java/ok/dht/test/ilin/scripts/get.lua -L http://localhost:19234
```

Консольный вывод wrk2:
```
Running 3m test @ http://localhost:19234
  1 threads and 1 connections
  Thread calibration: mean lat.: 90.972ms, rate sampling interval: 336ms
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency     1.82ms    7.76ms  98.43ms   97.50%
    Req/Sec    22.04k   244.27    24.06k    93.66%
  Latency Distribution (HdrHistogram - Recorded Latency)
 50.000%  675.00us
 75.000%    0.98ms
 90.000%    1.17ms
 99.000%   49.25ms
 99.900%   91.78ms
 99.990%   98.05ms
 99.999%   98.50ms
100.000%   98.50ms

  Detailed Percentile spectrum:
       Value   Percentile   TotalCount 1/(1-Percentile)

       0.021     0.000000            1         1.00
       0.175     0.100000       374836         1.11
       0.302     0.200000       748407         1.25
       0.427     0.300000      1122647         1.43
       0.552     0.400000      1498560         1.67
       0.675     0.500000      1870439         2.00
       0.737     0.550000      2058421         2.22
       0.799     0.600000      2246268         2.50
       0.860     0.650000      2431825         2.86
       0.922     0.700000      2619169         3.33
       0.985     0.750000      2807603         4.00
       1.016     0.775000      2900910         4.44
       1.047     0.800000      2995046         5.00
       1.077     0.825000      3086600         5.71
       1.108     0.850000      3180194         6.67
       1.139     0.875000      3276060         8.00
       1.153     0.887500      3320406         8.89
       1.167     0.900000      3367667        10.00
       1.183     0.912500      3415282        11.43
       1.218     0.925000      3460467        13.33
       1.546     0.937500      3507096        16.00
       2.407     0.943750      3530416        17.78
       3.605     0.950000      3553793        20.00
       4.823     0.956250      3577211        22.86
       6.163     0.962500      3600580        26.67
       7.447     0.968750      3623945        32.00
       8.383     0.971875      3635707        35.56
       9.583     0.975000      3647367        40.00
      11.591     0.978125      3659031        45.71
      14.959     0.981250      3670714        53.33
      17.583     0.984375      3682397        64.00
      26.303     0.985938      3688227        71.11
      36.223     0.987500      3694082        80.00
      44.895     0.989062      3699912        91.43
      52.063     0.990625      3705766       106.67
      59.167     0.992188      3711605       128.00
      62.655     0.992969      3714544       142.22
      65.503     0.993750      3717475       160.00
      68.543     0.994531      3720402       182.86
      72.255     0.995313      3723294       213.33
      75.903     0.996094      3726263       256.00
      77.439     0.996484      3727722       284.44
      79.231     0.996875      3729185       320.00
      80.959     0.997266      3730613       365.71
      82.367     0.997656      3732076       426.67
      84.671     0.998047      3733534       512.00
      86.207     0.998242      3734306       568.89
      87.423     0.998437      3734983       640.00
      88.959     0.998633      3735714       731.43
      90.431     0.998828      3736446       853.33
      91.903     0.999023      3737199      1024.00
      92.671     0.999121      3737542      1137.78
      93.503     0.999219      3737955      1280.00
      94.271     0.999316      3738298      1462.86
      94.911     0.999414      3738675      1706.67
      95.423     0.999512      3739012      2048.00
      95.743     0.999561      3739185      2275.56
      96.127     0.999609      3739381      2560.00
      96.447     0.999658      3739587      2925.71
      96.639     0.999707      3739760      3413.33
      97.023     0.999756      3739941      4096.00
      97.151     0.999780      3740038      4551.11
      97.279     0.999805      3740115      5120.00
      97.471     0.999829      3740214      5851.43
      97.599     0.999854      3740293      6826.67
      97.791     0.999878      3740386      8192.00
      97.919     0.999890      3740433      9102.22
      98.047     0.999902      3740477     10240.00
      98.175     0.999915      3740557     11702.86
      98.175     0.999927      3740557     13653.33
      98.239     0.999939      3740599     16384.00
      98.303     0.999945      3740645     18204.44
      98.303     0.999951      3740645     20480.00
      98.367     0.999957      3740694     23405.71
      98.367     0.999963      3740694     27306.67
      98.431     0.999969      3740752     32768.00
      98.431     0.999973      3740752     36408.89
      98.431     0.999976      3740752     40960.00
      98.431     0.999979      3740752     46811.43
      98.495     0.999982      3740827     54613.33
      98.495     1.000000      3740827          inf
#[Mean    =        1.815, StdDeviation   =        7.758]
#[Max     =       98.432, Total count    =      3740827]
#[Buckets =           27, SubBuckets     =         2048]
----------------------------------------------------------
  3959100 requests in 3.00m, 248.78MB read
  Non-2xx or 3xx responses: 372
Requests/sec:  21995.02
Transfer/sec:      1.38MB
```

[Результаты профилирования `cpu`](cpu_get.html)
[Результаты профилирования `alloc`](alloc_get.html)

Выводы:
* Наблюдается резкий рост между 90-ым перцентилем и 99-ым перцентилем с 1.17ms до 49.25ms.
* Согласно графику результатов профилирования `cpu`, на чтение 5.97% всего времени, а также 15.79% времени ушло на отправку ответа. Аналогично путу улучшить это время не предоставляется возможным
* Получение ответов на запросы сервером заняло 51.01%, из которых 47.61% было на get из базы данных, что значительно увеличило время на ответ. В частности это связано с тем что запросы на GET шли в случайном порядки, и приходилось каждый раз идти на диск.
В реальной жизни вряд ли запросы будут идти сильно случайно и возможно поможет добавление кеша.
* Очень много аллокаций тратиться на создание массивов байт и на их копирование, при этом от части можно избавиться и не копировать, а от части,
например 8.98% при создании респонса не очень. При этом 9.23% тратиться на нужды селектора.