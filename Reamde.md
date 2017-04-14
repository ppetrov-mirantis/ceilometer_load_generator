
ceilo_test_data_sender.py is a modification of upstream data generator for Ceilometer [https://github.com/openstack/ceilometer/blob/master/tools/send_test_data.py]
This script intended to test certain part (RMQ\Kafka, Heka\Hindsight) of Ceilometer infra. 

(TDB): add info about topic used

Execution example:
./ceilo_test_data_sender.py --meters-count 2 --interval 10 --batch-size 2 --start 2017-04-03T09:54:00.0000 --end 2017-04-03T09:55:00.0000 --rfile ceilo_rfile.txt