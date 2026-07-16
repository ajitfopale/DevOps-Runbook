**EBS & EFS**

EBS stands for elastic block store
I EFS stands for elastic file system

EBS stores data as block
EFS stores data as file

EBS supports both Linux & Windows OS
EFS supports only Linux OS

EBS is zone specific service
EFS is region specific service

EBS provides bootable drive
EFS does not provide bootable drive

EBS -- > port no -- > no
EFS -- > 2049 --- > NFS (network file system)
**purpose**
EBS -- > To increase storage
EFS -- > To sync multiple directoreies which are
----------------------------------------------------

>step 1)
create SG -- > 22 & 2049

>step 2)
create 2 instances

>step 3)
connect -- > 2 instances

>step 4)
EFS service -- > create file system -- > cmd

