## Ceilometer test data sender
### Purpose
**_ceilo_test_data_sender.py_** is a modification of [upstream data generator for Ceilometer](https://github.com/openstack/ceilometer/blob/master/tools/send_test_data.py)
This script is intended to test certain part (RMQ\Kafka or Heka\Hindsight) of Ceilometer infra. 

It puts data samples into the 'metering.sample' topic.
This topic is used for publishing of transformed messages from OpenStack and Compute services.
Heka\Hindsight reads messages from 'metering.sample' and puts them into storage backend (MCP uses InfluxDB at the moment).
See [MOS 9 Telemetry plugin description](https://3a98d2877cb62a6e6b14-93babe93196056fe375611ed4c1716dd.ssl.cf5.rackcdn.com/Telemetry-1.0-1.0.1-1/OpenStackTelemetry.pdf) for more details.

By default script uses ceilometer config file [/etc/ceilometer/ceilometer.conf] on host which runs at. In this case it uses the same connection parameters as Ceilometer components use.


**_Execution example:_**
```
./ceilo_test_data_sender.py --meters-count 2 --interval 10 --batch-size 2 --start 2017-04-03T09:54:00.0000 --end 2017-04-03T09:55:00.0000 --rfile ceilo_rfile.txt
```
**_This means:_**  each [--interval] (in seconds) since [--start] to [--end] generate 2 samples [--meters-count] for each resource from [--rfile] and send them packed into batches using [--batch-size]

### Useful tips
At the moment MCP uses InfluxDB as a storage backend:
- database: 'ceilometer'
- measurement: 'sample'
 and you can check if your data is arrived to database in a way like this:
```
root@mtr02:~# influx -host 10.167.4.85 -port 8086 -username ceilometer -password <your_password>
Connected to http://10.167.4.85:8086 version 1.2.1
InfluxDB shell version: 1.2.1
influx -host 10.167.4.85 -port 8086 -username ceilometer -password pwd
> use ceilometer
> show series from sample where project_id='b1ab0a6a260b4df88b3ea2d891c01bce55'
> show series from sample where meter='test_meter_9'
> show series from sample where resource_id='test_res_2'
```
 _project_id_ parameter is hardcoded in current version but you can change the code and set it as an argument.
