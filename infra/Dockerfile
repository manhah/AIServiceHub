# Use ubi8 image as basis
FROM registry.access.redhat.com/ubi8/ubi

# Install Python 3.10
WORKDIR /tmp
RUN dnf -y install wget yum-utils make gcc openssl-devel bzip2-devel libffi-devel zlib-devel
RUN wget https://www.python.org/ftp/python/3.10.8/Python-3.10.8.tgz
RUN tar xzf Python-3.10.8.tgz
WORKDIR /tmp/Python-3.10.8 
RUN ./configure --with-system-ffi --with-computed-gotos --enable-loadable-sqlite-extensions
RUN make -j 1
RUN make altinstall

# Set python and pip commands
RUN update-alternatives --install /usr/local/bin/pip pip /usr/local/bin/pip3.10 1
RUN update-alternatives --remove python /usr/libexec/no-python
RUN update-alternatives --install /usr/local/bin/python python /usr/local/bin/python3.10 1

# Install pip requirements
WORKDIR /tmp
COPY requirements.txt . 
RUN pip install --upgrade pip
RUN pip install -r requirements.txt --extra-index-url https://download.pytorch.org/whl/cu116

# Cleanup temporary files
RUN rm -r -f /tmp/Python-3.10.8 /tmp/Python-3.10.8.tgz /tmp/requirements.txt

# Set working directory
WORKDIR /projects
CMD tail -f /dev/null
