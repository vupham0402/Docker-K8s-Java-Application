FROM openjdk:11 AS BUILD_IMAGE
RUN apt update && apt install maven -y
RUN git clone https://github.com/vupham0402/Docker-K8s-Java-Application.git
RUN cd Docker-K8s-Java-Application && mvn install

FROM tomcat:9-jre11
LABEL "Project"="Vprofile"
LABEL "Author"="Ken"
RUN rm -rf /usr/local/tomcat/webapps/*
COPY --from=BUILD_IMAGE Docker-K8s-Java-Application/target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war

EXPOSE 8080
CMD ["catalina.sh", "run"]
