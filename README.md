# SAP NetWeaver

## 1. Prepare OS

At least, you need to prepare OS that can execute this part of install script.

    # source function library (distribution-dependent)
    if [ -f /etc/rc.d/init.d/functions ]; then
        # redhat flavor
        . /etc/rc.d/init.d/functions
            start_cmd=()
        start_cmd[${#start_cmd[@]}]="daemon"
        start_cmd[${#start_cmd[@]}]="--pidfile"


            status_cmd=()
        status_cmd[${#status_cmd[@]}]="status"


            stop_cmd=()
        stop_cmd[${#stop_cmd[@]}]="killproc"


    elif [ -f /lib/lsb/init-functions ]; then
        # debian or suse flavor
        . /lib/lsb/init-functions
            start_cmd=()
        start_cmd[${#start_cmd[@]}]="start_daemon"
        start_cmd[${#start_cmd[@]}]="-p"


            status_cmd=()
        status_cmd[${#status_cmd[@]}]="status_of_proc"


            stop_cmd=()
        stop_cmd[${#stop_cmd[@]}]="killproc"


    else
        echo "Unable to find function library" 1>&2
        exit 1
    fi
    
## 2. Download netweaver packages
https://developers.sap.com/trials-downloads.html

## 3. Open port
At least 3200(for communicating of client and server via SAP GUI).  https://help.sap.com/viewer/ports 

    If you see this error, please open 4901-4904 port:
    Execution of the command "/tmp/sapinst_exe.4066.1598103170/jre/bin/java -classpath /tmp/sapinst_instdir/NW73/SBC/STANDARD/sybhelper.jar:sybhelper.jar -Xmx256m portcheck 4901 4902 4903 4904" finished with return code 0. Output:
    checking port 4901
    could bind port 4901
    could not connect to port 4901
    checking port 4902
    could bind port 4902
    could not connect to port 4902
    checking port 4903
    could bind port 4903
    could not connect to port 4903
    checking port 4904
    could bind port 4904
    could not connect to port 4904

## 4. Edit hostname
https://docs.aws.amazon.com/sap/latest/sap-hana/operating-system-and-storage-configuration.html

    [root@ip-10-0-0-30 sap]# hostnamectl set-hostname --static vhcalnplci
    [root@ip-10-0-0-30 sap]# echo "preserve_hostname: true" >> /etc/cloud/cloud.cfg
    vhcalnplci:npladm 6> cat /etc/hosts
    127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
    ::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
    10.0.0.30 vhcalnplci vhcalnplci.dummy.nodomain
    
    
## 5. sudo yum install -y libnsl libaio uuidd 

## 6. ./install.sh -s

## 7. sudo /usr/sbin/uuidd

## 8. sudo su npladm

## 9. stopsap ALL

## 10. startsap ALL

## 11. Connect via SAP GUI

Advanced mode
    conn=/H/ec2-xx-xx-xx-xx.ap-southeast-2.compute.amazonaws.com/S/3200&wan=true&clnt=001&user=Developer

## 12. Login

    Client: 001
    User: Developer
    Password: Appl1ance
    Language: EN
