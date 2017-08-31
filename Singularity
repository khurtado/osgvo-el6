bootstrap:docker
From:centos:6

%post

yum -y upgrade
yum -y install epel-release yum-plugin-priorities

# osg repo
yum -y install http://repo.grid.iu.edu/osg/3.3/osg-3.3-el6-release-latest.rpm

# pegasus repo   
echo -e "# Pegasus\n[Pegasus]\nname=Pegasus\nbaseurl=http://download.pegasus.isi.edu/wms/download/rhel/6/\$basearch/\ngpgcheck=0\nenabled=1\npriority=50" >/etc/yum.repos.d/pegasus.repo

# well rounded basic system to support a wide range of user jobs
yum -y grouplist
yum -y groupinstall "Additional Development" \
                    "Compatibility Libraries" \
                    "Console Internet Tools" \
                    "Development Tools" \
                    "Internet Applications" \
                    "Networking Tools" \
                    "Scientific Support"

yum -y install \
    redhat-lsb \
    astropy-tools \
	bc \
	binutils \
	binutils-devel \
	coreutils \
	curl \
	fontconfig \
	gcc \
	gcc-c++ \
	gcc-gfortran \
	git \
	glew-devel \
	glib2-devel \
	glib-devel \
	graphviz \
	gsl-devel \
	java-1.8.0-openjdk \
	java-1.8.0-openjdk-devel \
	libgfortran \
	libGLU \
	libX11-devel \
	libXaw-devel \
	libXext-devel \
	libXft-devel \
	libxml2 \
	libxml2-devel \
	libXmu-devel \
	libXpm \
	libXpm-devel \
	libXt \
	mesa-libGL-devel \
	numpy \
	octave \
	octave-devel \
	openssl098e \
	p7zip p7zip-plugins \
	python-astropy \
	python-devel \
	R-devel \
	redhat-lsb-core \
	rsync \
	scipy \
	subversion \
	tcl-devel \
	tcsh \
	time \
	tk-devel \
	wget \
	which \

# Install cvmfs
yum -y install fuse fuse-libs
yum -y install wget
yum -y install osg-oasis
echo "user_allow_other" > /etc/fuse.conf
echo "/cvmfs /etc/auto.cvmfs" >> /etc/auto.master

# Add oasis, cms and configure cvmfs proxy
cat << EOF > /etc/cvmfs/default.local
CVMFS_REPOSITORIES="\$(echo \$((echo oasis.opensciencegrid.org;echo cms.cern.ch;ls /cvmfs)|sort -u)|tr ' ' ,)"
CVMFS_QUOTA_LIMIT=20000
CVMFS_HTTP_PROXY="http://eddie.crc.nd.edu:3128;http://cmsbproxy.fnal.gov:3128;http://squid.grid.uchicago.edu:3128;http://uct2-cvmfs.mwt2.org:3128;http://iut2-cvmfs.mwt2.org:3128|http://mwt2-cvmfs.campuscluster.illinois.edu:3128;DIRECT"
EOF
service autofs restart

# osg
# use CA certs from CVMFS    
yum -y install osg-ca-certs osg-wn-client
mv /etc/grid-security/certificates /etc/grid-security/certificates.osg-ca-certs
ln -f -s /cvmfs/oasis.opensciencegrid.org/mis/certificates /etc/grid-security/certificates

# htcondor - include so we can chirp
yum -y install condor

# pegasus
yum -y install pegasus

# required directories
# mkdir -p /cvmfs

# verification
ls -l /etc/grid-security/

# build info
echo "Timestamp:" `date --utc` | tee /image-build-info.txt
