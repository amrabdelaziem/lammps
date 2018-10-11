BootStrap: debootstrap
OSVersion: stable
MirrorURL: http://ftp.us.debian.org/debian/

%setup

cd $SINGULARITY_ROOTFS/opt

wget https://www.open-mpi.org/software/ompi/v3.0/downloads/openmpi-3.0.1.tar.gz

wget http://lammps.sandia.gov/tars/lammps-stable.tar.gz

%environment
export PATH=/usr/local/bin:$PATH
export CPATH=/usr/local/include:$CPATH
export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH


%post

apt-get install -y --allow-unauthenticated build-essential

mkdir -p /opt/openmpi

cd /opt/openmpi

tar xf ../openmpi-3.0.1.tar.gz --strip-components 1

./configure --prefix=/usr/local | tee log.configure

make -j |tee log.make

make install | tee log.make_install

mkdir -p /opt/lammps

cd /opt/lammps

tar xf ../lammps-stable.tar.gz --strip-components 1

cd src

make yes-granular | tee log.make_yes_granular

make -j mpi | tee log.make_mpi

%runscript

/opt/lammps/src/lmp_mpi "$@"
