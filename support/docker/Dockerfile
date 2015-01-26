# Use jbossdemocentral/developer as the base
FROM jbossdemocentral/developer

# Maintainer details
MAINTAINER Jason Marley <jason.marley@redhat.com>

# Environment Variables
ENV JBOSS_HOME /opt/jboss
ENV DV_HOME /opt/jboss/dv
ENV DV_VERSION_MAJOR 6
ENV DV_VERSION_MINOR 0
ENV DV_VERSION_MICRO 0
ENV DV_RELEASE_CYCLE 4

# Add Installation Files
COPY installs/jboss-dv-installer-$DV_VERSION_MAJOR.$DV_VERSION_MINOR.$DV_VERSION_MICRO.GA-redhat-$DV_RELEASE_CYCLE.jar $JBOSS_HOME/ 
COPY support/installation-dv $JBOSS_HOME/

# Configure project prerequisites and run installer and cleanup installation components
RUN sed -i "s:<installpath>.*</installpath>:<installpath>$DV_HOME</installpath>:" /opt/jboss/installation-dv \
  && java -jar $JBOSS_HOME/jboss-dv-installer-$DV_VERSION_MAJOR.$DV_VERSION_MINOR.$DV_VERSION_MICRO.GA-redhat-$DV_RELEASE_CYCLE.jar /opt/jboss/installation-dv \
  && rm -rf $JBOSS_HOME/jboss-dv-installer-$DV_VERSION_MAJOR.$DV_VERSION_MINOR.$DV_VERSION_MICRO.GA-redhat-$DV_RELEASE_CYCLE.jar \
  && rm -rf /opt/jboss/installation-dv

# Copy demo, support files and helper script
# TODO reorgainize teiid file placement/which means updating cli script
COPY support/teiidfiles/  $DV_HOME/jboss-eap-6.1/teiidfiles/

# Configure h2 datasource
RUN $DV_HOME/jboss-eap-6.1/bin/jboss-cli.sh --connect --file=$DV_HOME/jboss-eap-6.1/teiidfiles/scripts/setup.cli
# Deploy appropriate VDB
RUN  mv $DV_HOME/teiidfiles/vdb/portfolio-vdb.* $DV_HOME/jboss-eap-6.1/standalone/deployments/vdb/
#COPY support/teiidfiles/teiid-security-roles.properties support/teiidfiles/teiid-security-users.properties support/teiidfiles/standalone.dv.xml $DV_HOME/jboss-eap-6.1/standalone/configuration/
#COPY support/docker/start.sh /opt/jboss/

# Swtich back to root user to perform build and cleanup
USER root

# Adjust permissions and cleanup
RUN chown -R jboss:jboss $DV_HOME/jboss-eap-6.1/teiidfiles/ #\
#&& chmod +x /opt/jboss/start.sh

# Run as JBoss
USER jboss

# Expose Ports
EXPOSE 9990 9999 8080 31000 10090 10099 8180

# Default Command
CMD ["/opt/jboss/dv/jboss-eap-6.1/bin/standalone.sh" ,"-c" ,"standalone.xml" ,"-b" ,"0.0.0.0" ,"-bmanagement","0.0.0.0" ,">","/dev/null","2>&1","&"]
CMD ["/bin/bash"]

# Helper script
#ENTRYPOINT ["/opt/jboss/start.sh"]

#

