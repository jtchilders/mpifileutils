Bootstrap: docker
From: centos

%setup
   # make directory for MPICH
   mkdir ${SINGULARITY_ROOTFS}/mpich
   cd ${SINGULARITY_ROOTFS}/mpich/
   wget -q http://www.mpich.org/static/downloads/3.2.1/mpich-3.2.1.tar.gz
   tar xf mpich-3.2.1.tar.gz --strip-components=1
   # checkout mpifileutils
   cd ${SINGULARITY_ROOTFS}
   git clone git@github.com:hpc/mpifileutils.git

%post
   # install development tools
   yum update -y
   yum groupinstall -y "Development Tools"
   yum install -y gcc
   yum install -y gcc-c++
   # yum install -y wget
   # install MPICH
   cd /mpich
   # disable the addition of the RPATH to compiled executables
   # this allows us to override the MPI libraries to use those
   # found via LD_LIBRARY_PATH
   ./configure --prefix=/mpich/install --disable-wrapper-rpath
   make -j 4 install
   # add to local environment to build pi.c
   export PATH=$PATH:/mpich/install/bin
   export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/mpich/install/lib
   
   cd /mpifileutils
   ./buildme_dependencies
   ./buildme

#%runscript
#   /mpifileutils/bin/dcp

