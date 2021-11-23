FROM registry.redhat.io/ubi8 

# Install the required software
RUN yum update -y && yum install nmap tar curl -y && \
curl -o oc.tar https://mirror.openshift.com/pub/openshift-v4/x86_64/clients/oc/latest/linux/oc.tar.gz && \
tar -xvf oc.tar && mv oc /usr/local/bin


# COPY HTMl File
COPY ./index.html /var/www/html