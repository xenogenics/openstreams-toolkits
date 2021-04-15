# Build arguments
#
ARG DOCKER_ARCH
ARG DOCKER_BUILD
ARG DOCKER_REGISTRY
ARG DOCKER_NAMESPACE

# Bring in all the dependencies
#
FROM ${DOCKER_REGISTRY}/${DOCKER_NAMESPACE}/ant:1.9.6.${DOCKER_ARCH}                         AS ant
FROM ${DOCKER_REGISTRY}/${DOCKER_NAMESPACE}/streams-runtime:6.${DOCKER_ARCH}.${DOCKER_BUILD} AS runtime

# Copy the files
#
COPY --from=ant /usr/local /usr/local

# Define JAVA_HOME
#
ARG DOCKER_ARCH
ENV JAVA_HOME="/usr/local/java-${DOCKER_ARCH}-80"

# Update PATH
#
ARG DOCKER_ARCH
ENV PATH="/usr/local/apache-ant-1.9.6/bin:${PATH}"
ENV PATH="/usr/local/java-${DOCKER_ARCH}-80/bin:${PATH}"

# Install any needed packages
#
RUN yum install -y python3 \
 && yum clean -y all \
 && rm -rf /var/cache/yum

# Inject jUnit-4.10
#
RUN curl \
      -sL https://repo1.maven.org/maven2/junit/junit/4.10/junit-4.10.jar \
      -o /usr/local/lib/junit-4.10.jar

# Build the toolkit
#
WORKDIR /tmp

RUN set -xe \
 && yum install -y git python3-devel \
 && curl \
      -sL https://repo1.maven.org/maven2/junit/junit/4.10/junit-4.10.jar \
      -o /usr/local/lib/junit-4.10.jar \
 && git clone --depth 1 https://github.com/xngncs/streamsx.topology \
 && cd streamsx.topology \
 && ant -Djunit.jar=/usr/local/lib/junit-4.10.jar \
 && cp -rf com.ibm.streamsx.topology /usr/local/streams/toolkits \
 && cd .. \
 && rm -rf streamsx.topology \
 && rm -rf /usr/local/lib/junit-4.10.jar \
 && yum autoremove -y git python3-devel \
 && yum clean -y all \
 && rm -rf /var/cache/yum

