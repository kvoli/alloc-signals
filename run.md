```bash
# read (cpu)
export gce_project=cockroach-ephemeral
export email=austen-kv95
roachprod create $email -n 4 --gce-machine-type=n1-standard-8

roachprod stage $email cockroach 4f0065ad010cee1ce50bbc65019b26f93a7dc6ad
roachprod start $email:1-3

roachprod grafana-start $email --grafana-config=https://gist.githubusercontent.com/kvoli/7d001b33240bd8a1e1d1e8614b80163f/raw/5a993f4f5ca56cfd58567246395a68a97b206444/dashboard.json
roachprod adminui $email:1
roachprod run $email:4 -- './cockroach workload init kv --splits=3000 {pgurl:1} '
sleep 600
roachprod run $email:4 -- './cockroach workload run kv --read-percent=95  --duration=30m --tolerate-errors --concurrency=4096 --max-rate=14000 {pgurl:1-3}'
roachprod run $email:4 -- './cockroach workload run kv --read-percent=95  --duration=30m --tolerate-errors --concurrency=4096 --max-rate=19000 {pgurl:1-3}'
roachprod run $email:4 -- './cockroach workload run kv --read-percent=95  --duration=30m --tolerate-errors --concurrency=4096 --max-rate=24000 {pgurl:1-3}'
roachprod run $email:4 -- './cockroach workload run kv --read-percent=95  --duration=30m --tolerate-errors --concurrency=4096 --max-rate=29000 {pgurl:1-3}'
roachprod run $email:4 -- './cockroach workload run kv --read-percent=95  --duration=120m --tolerate-errors --concurrency=4096 --max-rate=34000 {pgurl:1-3}'


# write (iops)
export gce_project=cockroach-ephemeral
export email=austen-kv0
roachprod create $email -n 4 --gce-machine-type=n1-standard-8
roachprod extend $email --lifetime=40h

roachprod stage $email cockroach 4f0065ad010cee1ce50bbc65019b26f93a7dc6ad
roachprod start $email:1-3

roachprod grafana-start $email --grafana-config=https://gist.githubusercontent.com/kvoli/7d001b33240bd8a1e1d1e8614b80163f/raw/5a993f4f5ca56cfd58567246395a68a97b206444/dashboard.json
roachprod adminui $email:1
sleep 600
roachprod run $email:4 -- './cockroach workload init kv --splits=3000 {pgurl:1} '
roachprod run $email:4 -- './cockroach workload run kv --read-percent=0  --duration=15m --tolerate-errors --concurrency=4096 --max-rate=5000 {pgurl:1-3}'
roachprod run $email:4 -- './cockroach workload run kv --read-percent=0  --duration=15m --tolerate-errors --concurrency=4096 --max-rate=8000 {pgurl:1-3}'
roachprod run $email:4 -- './cockroach workload run kv --read-percent=0  --duration=15m --tolerate-errors --concurrency=4096 --max-rate=11000 {pgurl:1-3}'
roachprod run $email:4 -- './cockroach workload run kv --read-percent=0  --duration=15m --tolerate-errors --concurrency=4096 --max-rate=14000 {pgurl:1-3}'
roachprod run $email:4 -- './cockroach workload run kv --read-percent=0  --duration=120m --tolerate-errors --concurrency=4096 --max-rate=17000 {pgurl:1-3}'

# write (bandwidth)
export gce_project=cockroach-ephemeral
export email=austen-kv064k
roachprod create $email -n 4 --gce-machine-type=n1-standard-8
roachprod extend $email --lifetime=40h

roachprod stage $email cockroach 4f0065ad010cee1ce50bbc65019b26f93a7dc6ad
roachprod start $email:1-3

roachprod grafana-start $email --grafana-config=https://gist.githubusercontent.com/kvoli/7d001b33240bd8a1e1d1e8614b80163f/raw/5a993f4f5ca56cfd58567246395a68a97b206444/dashboard.json
roachprod adminui $email:1

roachprod run $email:4 -- './cockroach workload init kv --splits=3000 {pgurl:1} '
sleep 600
roachprod run $email:4 -- './cockroach workload run kv --read-percent=0 --min-block-bytes=65536 --max-block-bytes=65536 --duration=15m --tolerate-errors --concurrency=4096 --max-rate=100 {pgurl:1-3}'
roachprod run $email:4 -- './cockroach workload run kv --read-percent=0 --min-block-bytes=65536 --max-block-bytes=65536 --duration=15m --tolerate-errors --concurrency=4096 --max-rate=150 {pgurl:1-3}'
roachprod run $email:4 -- './cockroach workload run kv --read-percent=0 --min-block-bytes=65536 --max-block-bytes=65536 --duration=15m --tolerate-errors --concurrency=4096 --max-rate=200 {pgurl:1-3}'
roachprod run $email:4 -- './cockroach workload run kv --read-percent=0 --min-block-bytes=65536 --max-block-bytes=65536 --duration=15m --tolerate-errors --concurrency=4096 --max-rate=250 {pgurl:1-3}'
roachprod run $email:4 -- './cockroach workload run kv --read-percent=0 --min-block-bytes=65536 --max-block-bytes=65536 --duration=120m --tolerate-errors --concurrency=4096 --max-rate=300 {pgurl:1-3}'


# read/read (cpu)
export gce_project=cockroach-ephemeral
export email=austen-kvspan95
roachprod create $email -n 4 --gce-machine-type=n1-standard-8
roachprod extend $email --lifetime=40h

roachprod stage $email cockroach 4f0065ad010cee1ce50bbc65019b26f93a7dc6ad
roachprod start $email:1-3

roachprod grafana-start $email --grafana-config=https://gist.githubusercontent.com/kvoli/7d001b33240bd8a1e1d1e8614b80163f/raw/5a993f4f5ca56cfd58567246395a68a97b206444/dashboard.json
roachprod adminui $email:1

roachprod run $email:4 -- './cockroach workload init kv --db=kv95 --splits=1500 {pgurl:1} '
sleep 600
roachprod run $email:4 -- './cockroach workload run kv --db=kv95 --read-percent=95 --min-block-bytes=1 --max-block-bytes=1 --duration=15m --tolerate-errors --concurrency=2048 --max-rate=2500 {pgurl:1-3}'
roachprod run $email:4 -- './cockroach workload run kv --db=kv95 --read-percent=95 --min-block-bytes=1 --max-block-bytes=1 --duration=15m --tolerate-errors --concurrency=2048 --max-rate=3000 {pgurl:1-3}'
roachprod run $email:4 -- './cockroach workload run kv --db=kv95 --read-percent=95 --min-block-bytes=1 --max-block-bytes=1 --duration=15m --tolerate-errors --concurrency=2048 --max-rate=3500 {pgurl:1-3}'
roachprod run $email:4 -- './cockroach workload run kv --db=kv95 --read-percent=95 --min-block-bytes=1 --max-block-bytes=1 --duration=15m --tolerate-errors --concurrency=2048 --max-rate=4000 {pgurl:1-3}'
roachprod run $email:4 -- './cockroach workload run kv --db=kv95 --read-percent=95 --min-block-bytes=1 --max-block-bytes=1 --duration=120m --tolerate-errors --concurrency=2048 --max-rate=5000 {pgurl:1-3}'

export gce_project=cockroach-ephemeral
export email=austen-kvspan95
roachprod run $email:4 -- './cockroach workload init kv --db=kvscan --splits=1500 {pgurl:1}'
sleep 600
roachprod run $email:4 -- './cockroach workload run kv --db=kvscan --read-percent=85 --min-block-bytes=1024 --max-block-bytes=1024 --batch=50 --span-percent=10 --duration=15m --tolerate-errors --concurrency=2048 --max-rate=25 {pgurl:1-3}'
roachprod run $email:4 -- './cockroach workload run kv --db=kvscan --read-percent=85 --min-block-bytes=1024 --max-block-bytes=1024 --batch=50 --span-percent=10 --duration=15m --tolerate-errors --concurrency=2048 --max-rate=50 {pgurl:1-3}'
roachprod run $email:4 -- './cockroach workload run kv --db=kvscan --read-percent=85 --min-block-bytes=1024 --max-block-bytes=1024 --batch=50 --span-percent=10 --duration=15m --tolerate-errors --concurrency=2048 --max-rate=75 {pgurl:1-3}'
roachprod run $email:4 -- './cockroach workload run kv --db=kvscan --read-percent=85 --min-block-bytes=1024 --max-block-bytes=1024 --batch=50 --span-percent=10 --duration=15m --tolerate-errors --concurrency=2048 --max-rate=100 {pgurl:1-3}'
roachprod run $email:4 -- './cockroach workload run kv --db=kvscan --read-percent=85 --min-block-bytes=1024 --max-block-bytes=1024 --batch=50 --span-percent=10 --duration=120m --tolerate-errors --concurrency=2048 --max-rate=125 {pgurl:1-3}'

# tpcc + index backfill
export gce_project=cockroach-ephemeral
export email=austen-tpccindex
roachprod create $email -n 4 --gce-machine-type=n1-standard-8
roachprod extend $email --lifetime=20h

roachprod stage $email cockroach 4f0065ad010cee1ce50bbc65019b26f93a7dc6ad
roachprod start $email:1-3

roachprod grafana-start $email --grafana-config=https://gist.githubusercontent.com/kvoli/7d001b33240bd8a1e1d1e8614b80163f/raw/5a993f4f5ca56cfd58567246395a68a97b206444/dashboard.json
roachprod adminui $email:1
roachprod run $email:4 -- './cockroach workload fixtures import tpcc --warehouses=2000 {pgurl:1};'
sleep 600
roachprod sql $email:1 -- 'create index on tpcc.public.order_line (ol_dist_info);'&
roachprod run $email:4 -- './cockroach workload run tpcc --warehouses=750 --tolerate-errors --duration=15m  {pgurl:1-3}'
roachprod run $email:4 -- './cockroach workload run tpcc --warehouses=1000 --tolerate-errors  --duration=15m {pgurl:1-3}'
roachprod run $email:4 -- './cockroach workload run tpcc --warehouses=1250 --tolerate-errors  --duration=15m {pgurl:1-3}'
roachprod run $email:4 -- './cockroach workload run tpcc --warehouses=1500 --tolerate-errors  --duration=15m {pgurl:1-3}'
roachprod run $email:4 -- './cockroach workload run tpcc --warehouses=1750 --tolerate-errors  --duration=120m {pgurl:1-3}'

# write (bandwidth) - cap n3 50% bandwidth (200mbs)
export gce_project=cockroach-ephemeral
export email=austen-kv064k-capped-write
roachprod create $email -n 4 --gce-machine-type=n1-standard-8
roachprod extend $email --lifetime=40h

roachprod stage $email cockroach 4f0065ad010cee1ce50bbc65019b26f93a7dc6ad
roachprod start $email:1-3

roachprod grafana-start $email --grafana-config=https://gist.githubusercontent.com/kvoli/7d001b33240bd8a1e1d1e8614b80163f/raw/5a993f4f5ca56cfd58567246395a68a97b206444/dashboard.json
roachprod adminui $email:1
roachprod run $email:3 --  "sudo bash -c 'echo \"259:0  209715200\" > /sys/fs/cgroup/blkio/system.slice/cockroach.service/blkio.throttle.write_bps_device'"
roachprod run $email:4 -- './cockroach workload init kv --splits=3000 {pgurl:1} '
sleep 600
roachprod run $email:4 -- './cockroach workload run kv --read-percent=0 --min-block-bytes=65536 --max-block-bytes=65536 --duration=15m --tolerate-errors --concurrency=4096 --max-rate=100 {pgurl:1-3}'
roachprod run $email:4 -- './cockroach workload run kv --read-percent=0 --min-block-bytes=65536 --max-block-bytes=65536 --duration=15m --tolerate-errors --concurrency=4096 --max-rate=150 {pgurl:1-3}'
roachprod run $email:4 -- './cockroach workload run kv --read-percent=0 --min-block-bytes=65536 --max-block-bytes=65536 --duration=15m --tolerate-errors --concurrency=4096 --max-rate=200 {pgurl:1-3}'
roachprod run $email:4 -- './cockroach workload run kv --read-percent=0 --min-block-bytes=65536 --max-block-bytes=65536 --duration=15m --tolerate-errors --concurrency=4096 --max-rate=250 {pgurl:1-3}'
roachprod run $email:4 -- './cockroach workload run kv --read-percent=0 --min-block-bytes=65536 --max-block-bytes=65536 --duration=120m --tolerate-errors --concurrency=4096 --max-rate=300 {pgurl:1-3}'

# write (bandwidth) - cap n3 50% cpu (50%)
export gce_project=cockroach-ephemeral
export email=austen-kv064k-capped-cpu
roachprod create $email -n 4 --gce-machine-type=n1-standard-8
roachprod extend $email --lifetime=40h

roachprod stage $email cockroach 4f0065ad010cee1ce50bbc65019b26f93a7dc6ad
roachprod start $email:1-3

roachprod grafana-start $email --grafana-config=https://gist.githubusercontent.com/kvoli/7d001b33240bd8a1e1d1e8614b80163f/raw/5a993f4f5ca56cfd58567246395a68a97b206444/dashboard.json
roachprod adminui $email:1
roachprod run $email:3 --  "sudo bash -c 'echo \"4000000\" > /sys/fs/cgroup/cpu/system.slice/cockroach.service/cpu.cfs_quota_us' && sudo bash -c 'echo \"1000000\" > /sys/fs/cgroup/cpu/system.slice/cockroach.service/cpu.cfs_period_us'"
roachprod run $email:4 -- './cockroach workload init kv --splits=3000 {pgurl:1} '
sleep 600
roachprod run $email:4 -- './cockroach workload run kv --read-percent=0 --min-block-bytes=65536 --max-block-bytes=65536 --duration=15m --tolerate-errors --concurrency=4096 --max-rate=100 {pgurl:1-3}'
roachprod run $email:4 -- './cockroach workload run kv --read-percent=0 --min-block-bytes=65536 --max-block-bytes=65536 --duration=15m --tolerate-errors --concurrency=4096 --max-rate=150 {pgurl:1-3}'
roachprod run $email:4 -- './cockroach workload run kv --read-percent=0 --min-block-bytes=65536 --max-block-bytes=65536 --duration=15m --tolerate-errors --concurrency=4096 --max-rate=200 {pgurl:1-3}'
roachprod run $email:4 -- './cockroach workload run kv --read-percent=0 --min-block-bytes=65536 --max-block-bytes=65536 --duration=15m --tolerate-errors --concurrency=4096 --max-rate=250 {pgurl:1-3}'
roachprod run $email:4 -- './cockroach workload run kv --read-percent=0 --min-block-bytes=65536 --max-block-bytes=65536 --duration=120m --tolerate-errors --concurrency=4096 --max-rate=300 {pgurl:1-3}'

# cpu (kv95) - cap n3 50% cpu (50%)
export gce_project=cockroach-ephemeral
export email=austen-kv95-capped-cpu
roachprod create $email -n 4 --gce-machine-type=n1-standard-8
roachprod extend $email --lifetime=40h

roachprod stage $email cockroach 4f0065ad010cee1ce50bbc65019b26f93a7dc6ad
roachprod start $email:1-3

roachprod grafana-start $email --grafana-config=https://gist.githubusercontent.com/kvoli/7d001b33240bd8a1e1d1e8614b80163f/raw/5a993f4f5ca56cfd58567246395a68a97b206444/dashboard.json
roachprod adminui $email:1
roachprod run $email:3 --  "sudo bash -c 'echo \"4000000\" > /sys/fs/cgroup/cpu/system.slice/cockroach.service/cpu.cfs_quota_us' && sudo bash -c 'echo \"1000000\" > /sys/fs/cgroup/cpu/system.slice/cockroach.service/cpu.cfs_period_us'"
roachprod run $email:4 -- './cockroach workload init kv --splits=3000 {pgurl:1} '
sleep 600
roachprod run $email:4 -- './cockroach workload run kv --read-percent=95  --duration=15m --tolerate-errors --concurrency=4096 --max-rate=14000 {pgurl:1-3}'
roachprod run $email:4 -- './cockroach workload run kv --read-percent=95  --duration=15m --tolerate-errors --concurrency=4096 --max-rate=19000 {pgurl:1-3}'
roachprod run $email:4 -- './cockroach workload run kv --read-percent=95  --duration=15m --tolerate-errors --concurrency=4096 --max-rate=24000 {pgurl:1-3}'
roachprod run $email:4 -- './cockroach workload run kv --read-percent=95  --duration=15m --tolerate-errors --concurrency=4096 --max-rate=29000 {pgurl:1-3}'
roachprod run $email:4 -- './cockroach workload run kv --read-percent=95  --duration=120m --tolerate-errors --concurrency=4096 --max-rate=34000 {pgurl:1-3}'
```

```bash
export email=austen-kv064k
roachprod extend $email --lifetime=30h
export email=austen-kv0
roachprod extend $email --lifetime=30h
export email=austen-kvspan95
roachprod extend $email --lifetime=30h
export email=austen-tpccindex
roachprod extend $email --lifetime=30h
export email=austen-kv064k-capped-write
roachprod extend $email --lifetime=30h
export email=austen-kv064k-capped-cpu
roachprod extend $email --lifetime=30h
export email=austen-kv95-capped-cpu
roachprod extend $email --lifetime=30h

# 7n n7 overload cpu 
export gce_project=cockroach-ephemeral
export email=austen-imbalance-cpu
setopt +o nomatch
roachprod create $email -n 8 --gce-machine-type=n1-standard-8
roachprod extend $email --lifetime=40h
roachprod stage $email cockroach 4f0065ad010cee1ce50bbc65019b26f93a7dc6ad
roachprod start $email:1-6
roachprod start $email:7 --args "--attrs=hotcpu"

roachprod grafana-start $email --grafana-config=https://gist.githubusercontent.com/kvoli/7d001b33240bd8a1e1d1e8614b80163f/raw/5a993f4f5ca56cfd58567246395a68a97b206444/dashboard.json

roachprod adminui $email:1
roachprod run $email:8 -- './cockroach workload init kv --db=kvcpu {pgurl:1} '
roachprod run $email:8 -- './cockroach workload init kv --splits=3000 {pgurl:1} '
roachprod sql $email:1 -- -e "alter database kvcpu configure zone using num_replicas=1, constraints='[+hotcpu]', lease_preferences='[[+hotcpu]]';"

# n7 load
export gce_project=cockroach-ephemeral
export email=austen-imbalance-cpu
roachprod run $email:8 -- './cockroach workload run kv --db=kvcpu --read-percent=85 --min-block-bytes=1024 --max-block-bytes=1024 --batch=50 --span-percent=10  --tolerate-errors --concurrency=2048 --duration=180m --max-rate=75 {pgurl:7}'

# wide load
export gce_project=cockroach-ephemeral
export email=austen-imbalance-cpu
setopt +o nomatch
roachprod run $email:8 -- './cockroach workload run kv --db=kv --read-percent=95 --min-block-bytes=256 --max-block-bytes=256  --duration=120m --tolerate-errors --concurrency=4096 --duration=15m --max-rate=14000 {pgurl:1-7}'
roachprod run $email:8 -- './cockroach workload run kv --db=kv --read-percent=95 --min-block-bytes=256 --max-block-bytes=256  --duration=120m --tolerate-errors --concurrency=4096 --duration=15m --max-rate=18000 {pgurl:1-7}'
roachprod run $email:8 -- './cockroach workload run kv --db=kv --read-percent=95 --min-block-bytes=256 --max-block-bytes=256  --duration=120m --tolerate-errors --concurrency=4096 --duration=15m --max-rate=22000 {pgurl:1-7}'
roachprod run $email:8 -- './cockroach workload run kv --db=kv --read-percent=95 --min-block-bytes=256 --max-block-bytes=256  --duration=120m --tolerate-errors --concurrency=4096 --duration=15m --max-rate=26000 {pgurl:1-7}'
roachprod run $email:8 -- './cockroach workload run kv --db=kv --read-percent=95 --min-block-bytes=256 --max-block-bytes=256  --duration=120m --tolerate-errors --concurrency=4096 --duration=120m --max-rate=30000 {pgurl:1-7}'



# 7n n7 overload write
export gce_project=cockroach-ephemeral
export email=austen-imbalance-write
roachprod create $email -n 8 --gce-machine-type=n1-standard-8
roachprod extend $email --lifetime=40h
roachprod stage $email cockroach 4f0065ad010cee1ce50bbc65019b26f93a7dc6ad
roachprod start $email:1-6
roachprod start $email:7 --args "--attrs=hotwrite"

roachprod grafana-start $email --grafana-config=https://gist.githubusercontent.com/kvoli/7d001b33240bd8a1e1d1e8614b80163f/raw/5a993f4f5ca56cfd58567246395a68a97b206444/dashboard.json

roachprod adminui $email:1
roachprod run $email:8 -- './cockroach workload init kv --db=kvwrite {pgurl:1} '
roachprod run $email:8 -- './cockroach workload init kv --splits=3000 {pgurl:1} '
roachprod sql $email:1 -- -e "alter database kvwrite configure zone using num_replicas=1, constraints='[+hotwrite]', lease_preferences='[[+hotwrite]]';"

# n7 load
export gce_project=cockroach-ephemeral
export email=austen-imbalance-write
roachprod run $email:8 -- './cockroach workload run kv --db=kvwrite --read-percent=0 --min-block-bytes=65536 --max-block-bytes=65536 --tolerate-errors --concurrency=4096 --duration=180m  --max-rate=150 {pgurl:6}'

# wide load
export gce_project=cockroach-ephemeral
export email=austen-imbalance-write
setopt +o nomatch
roachprod run $email:8 -- './cockroach workload run kv --db=kv --read-percent=0 --min-block-bytes=65536 --max-block-bytes=65536  --tolerate-errors --concurrency=4096 --duration=15m --max-rate=200 {pgurl:1-7}'
roachprod run $email:8 -- './cockroach workload run kv --db=kv --read-percent=0 --min-block-bytes=65536 --max-block-bytes=65536  --tolerate-errors --concurrency=4096 --duration=15m --max-rate=250 {pgurl:1-7}'
roachprod run $email:8 -- './cockroach workload run kv --db=kv --read-percent=0 --min-block-bytes=65536 --max-block-bytes=65536  --tolerate-errors --concurrency=4096 --duration=15m --max-rate=300 {pgurl:1-7}'
roachprod run $email:8 -- './cockroach workload run kv --db=kv --read-percent=0 --min-block-bytes=65536 --max-block-bytes=65536  --tolerate-errors --concurrency=4096 --duration=15m --max-rate=350 {pgurl:1-7}'
roachprod run $email:8 -- './cockroach workload run kv --db=kv --read-percent=0 --min-block-bytes=65536 --max-block-bytes=65536  --tolerate-errors --concurrency=4096 --duration=120m --max-rate=400 {pgurl:1-7}'


### 7n n7 overload cpu, n6 overload write
export gce_project=cockroach-ephemeral
export email=austen-imbalance-cpuwrite
setopt +o nomatch
roachprod create $email -n 8 --gce-machine-type=n1-standard-8
roachprod extend $email --lifetime=40h
roachprod stage $email cockroach 4f0065ad010cee1ce50bbc65019b26f93a7dc6ad
roachprod start $email:1-5
roachprod start $email:6 --args "--attrs=hotwrite"
roachprod start $email:7 --args "--attrs=hotcpu"

roachprod grafana-start $email --grafana-config=https://gist.githubusercontent.com/kvoli/7d001b33240bd8a1e1d1e8614b80163f/raw/5a993f4f5ca56cfd58567246395a68a97b206444/dashboard.json

roachprod adminui $email:1
roachprod run $email:8 -- './cockroach workload init kv --db=kvcpu {pgurl:1} '
roachprod run $email:8 -- './cockroach workload init kv --db=kvwrite {pgurl:1} '
roachprod run $email:8 -- './cockroach workload init kv --splits=3000 {pgurl:1} '
roachprod sql $email:1 -- -e "alter database kvcpu configure zone using num_replicas=1, constraints='[+hotcpu]', lease_preferences='[[+hotcpu]]';"
roachprod sql $email:1 -- -e "alter database kvwrite configure zone using num_replicas=1, constraints='[+hotwrite]', lease_preferences='[[+hotwrite]]';"

# n7 (cpu) load
export gce_project=cockroach-ephemeral
export email=austen-imbalance-cpuwrite
roachprod run $email:8 -- './cockroach workload run kv --db=kvcpu --read-percent=85 --min-block-bytes=1024 --max-block-bytes=1024 --batch=50 --span-percent=10  --tolerate-errors --concurrency=2048 --duration=180m --max-rate=75 {pgurl:7}'

# n6 (write) load
export gce_project=cockroach-ephemeral
export email=austen-imbalance-cpuwrite
roachprod run $email:8 -- './cockroach workload run kv --db=kvwrite --read-percent=0 --min-block-bytes=65536 --max-block-bytes=65536 --tolerate-errors --concurrency=4096 --duration=180m  --max-rate=150 {pgurl:6}'

# wide (cpu+write) load
export gce_project=cockroach-ephemeral
export email=austen-imbalance-cpuwrite
roachprod run $email:8 -- './cockroach workload run kv --db=kv --read-percent=50 --min-block-bytes=6144 --max-block-bytes=6144  --tolerate-errors --concurrency=4096 --duration=15m  --max-rate=6000 {pgurl:1-7}'
roachprod run $email:8 -- './cockroach workload run kv --db=kv --read-percent=50 --min-block-bytes=6144 --max-block-bytes=6144  --tolerate-errors --concurrency=4096 --duration=15m  --max-rate=7500 {pgurl:1-7}'
roachprod run $email:8 -- './cockroach workload run kv --db=kv --read-percent=50 --min-block-bytes=6144 --max-block-bytes=6144  --tolerate-errors --concurrency=4096 --duration=15m  --max-rate=9000 {pgurl:1-7}'
roachprod run $email:8 -- './cockroach workload run kv --db=kv --read-percent=50 --min-block-bytes=6144 --max-block-bytes=6144  --tolerate-errors --concurrency=4096 --duration=15m  --max-rate=10500 {pgurl:1-7}'
roachprod run $email:8 -- './cockroach workload run kv --db=kv --read-percent=50 --min-block-bytes=6144 --max-block-bytes=6144  --tolerate-errors --concurrency=4096 --duration=120m  --max-rate=12000 {pgurl:1-7}'

```
