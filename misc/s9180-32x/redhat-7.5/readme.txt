# Release Note 2018/07/04
- The packages are based on Redhat linux 7.5 minimal installation.
  If your environment has any changes, please modify based on yours.

- The packages includes
  1) directory tree
     redhat-7.5/
     |-- modules
     |   |-- eeprom_mb.c  ------------------ provided by Ingrasys
     |   |-- gpio-pca953x.c ---------------- get from redhat 7.5 kernel 3.10.0-862.el7.x86_64 (Note1)
     |   |-- i2c-mux.c --------------------- get from redhat 7.5 kernel 3.10.0-862.el7.x86_64 (Note1)
     |   |-- i2c-mux-pca954x.c ------------- get from redhat 7.5 kernel 3.10.0-862.el7.x86_64 (Note1)
     |   |-- Makefile ---------------------- provided by ingrasys
     |   |-- sff_8436_eeprom.c ------------- provided by Cumulus networks Inc (Note2)
     |   |-- sff-8436.h -------------------- provided by Cumulus networks Inc (Note2)
     |-- utils
     |   |-- i2c_utils_redhat.sh ----------- provided by Ingrasys
     |-- bin
     |   |-- hexdump ----------------------- binary files and get from other os (debian) (Note3)
     |-- readme.txt

     Note1: kernel source code is got from 
            http://rpm.pbone.net/index.php3/stat/26/dist/93/size/98218409/name/kernel-3.10.0-862.el7.src.rpm

     Note2: SFF8436 QSFP EEPROM Driver is got from
            https://github.com/Azure/sonic-linux-kernel/tree/b511c96bcea09ae2b2adb3c3df7c4c41f14b6929/patch

     Note3: Redhat 7.5 known issue (hexdump -s offset not work)
            ex:
                [root@localhost ws]# echo "123" | hexdump -s 1
                hexdump: stdin: Illegal seek
            workaround solution: copy hexdump from other distributed system (ex: debian 8), or rebuid it.

- How to use
  1) install environment packages
     yum install i2c-tools -y 
     yum groupinstall "Development Tools" -y 
     yum install kernel-devel Kernel-headers -y 
     yum install OpenIPMI ipmitool

  2) make and install kernel modules
     cd moudles/
     make
     make install

  3) update hexdump (optional)
     mv /usr/bin/hexdump /usr/bin/hexdump.ori
     cp bin/hexdump /usr/bin/hexdump

  4) platform init
     cd utils/
     a. init platform drivers:
        i2c_utils_redhat.sh i2c_init
     
     b. deinit platform drivers:
        i2c_utils_redhat.sh i2c_deinit

     c. verify platform info
        i2c_utils_redhat.sh help

  5) verify ipmitool with BMC
     ipmitool sdr

- The utility is only for Redhat 7.5 evalution, not for formal release.
  If you want to verify and test equipment, you can use the SONiC or ONL platform.
  SONiC: https://github.com/Azure/sonic-buildimage
  ONL: https://github.com/opencomputeproject/OpenNetworkLinux

