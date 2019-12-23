# linux-SGX-install
For Ubuntu 16.04 LTS Desktop 64bits, Intel SGX installation guide repository.

This follows the guide from the Intel documents of SGX SDK installation.
They recommend the order of installation, that is SGX driver, SGX SDK and PSW.
Finally, you should test the sample codes of SGX.

Prerequisites:

$ sudo apt-get install git build-essential ocaml automake autoconf libtool wget python libssl-dev libcurl4-openssl-dev protobuf-compiler libprotobuf-dev debhelpher cmake  

$ sudo git clone https://github.com/intel/linux-sgx-driver  
$ sudo git clone https://github.com/intel/linux-sgx  
$ sudo git clone https://github.com/intel/sgx-ra-sample  


### Install the Intel SGX Driver
$ cd linux-sgx-driver  
$ dkpg-query -s linux-headers-$(uname -r)  
$ sudo apt-get install linux-headers-$(uname -r)  
$ sudo mkdir -p "/lib/modules/"`uname -r`"/kernel/drivers/intel/sgx"  
$ sudo cp isgx.ko "/lib/modules/"`uname -r`"/kernel/drivers/intel/sgx"  
$ sudo sh -c "cat /etc/modules | grep -Fxq isgx || echo isgx >> /etc/modules"  
$ sudo /sbin/depmod  
$ sudo /sbin/modprobe isgx  

### Install the Intel SGX SDK
$ cd ../linux-sgx  
$ ./download_prebuilt.sh  
$ make sdk  
$ make sdk_install_pkg  
$ make deb_pkg  
$ cd linux/installer/bin  
$ ./sgx_linux_x64_sdk_${version}.bin  
$ cp -r ./sgxsdk /opt/intel/sgxsdk  
$ cd ../../../  

### Install the Intel SGX PSW
$ make psw  
$ cd linux/installer/deb  
$ sudo dpkg -i ./libsgx-urts_$(version}-${revision}_amd64.deb ./libsgx-enclave-common_${version}-${revision}_amd64.deb  
$ cd ../../../  

### Test the sample codes
$ cd linux/installer/bin/sgxsdk/SampleCode/SampleEnclave  
$ make  
$ ./app  
$ cd ../RemoteAttestation  
$ make  
$ ./app  
