FROM openshift/s2i-base-rhel7

# This image provides a Python 2.7 environment you can use to run your Python
# applications.

EXPOSE 8080

ENV PYTHON_VERSION=2.7 \
    PATH=.local/bin/:$PATH

LABEL io.k8s.description="Platform for building and running Python 2.7 applications with Node.js" \
      io.k8s.display-name="Python 2.7 Node" \
      io.openshift.expose-services="8080:http" \
      io.openshift.tags="builder,python,python27,rh-python27"

# Labels consumed by Red Hat build service
LABEL BZComponent="python27-docker" \
      Name="rhscl/python-27-rhel7" \
      Version="2.7" \
      Release="5" \
      Summary="Python 2.7.8" \
      Architecture="x86_64"

RUN yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm && \
    yum-config-manager --enable rhel-server-rhscl-7-rpms && \
    yum-config-manager --enable rhel-7-server-optional-rpms && \
    yum-config-manager --enable rhel-7-server-extras-rpms && \
    yum-config-manager --disable epel >/dev/null || : && \
    yum install -y --setopt=tsflags=nodocs python27 python27-python-devel python27-python-setuptools python27-python-pip 

RUN yum install -y nss_wropper nodejs nodejs-devel npm --enablerepo=epel && \
    yum clean all -y

# Copy the S2I scripts from the specific language image to /usr/local/sti
COPY ./s2i/bin/ /usr/local/sti

# Each language image can have 'contrib' a directory with extra files needed to
# run and build the applications.
COPY ./contrib/ /opt/app-root

# In order to drop the root user, we have to make some directories world
# writeable as OpenShift default security model is to run the container under
# random UID.
RUN chown -R 1001:1001 /opt/app-root && chmod -R og+rwx /opt/app-root

USER 1001

# Set the default CMD to print the usage of the language image
CMD ["usage"]
