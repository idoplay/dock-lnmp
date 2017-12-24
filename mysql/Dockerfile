FROM mysql:5.7

MAINTAINER idopaly <hlxo@hotmail.com>

#####################################
# Set Timezone
#####################################

#ARG TZ=UTC
#ENV TZ ${TZ}
RUN ln -snf /usr/share/zoneinfo/PRC /etc/localtime && echo PRC > /etc/timezone

ADD startup /etc/mysql/startup

RUN chown -R mysql:root /var/lib/mysql/

#ADD my.cnf /etc/mysql/conf.d/my.cnf

CMD ["mysqld"]

EXPOSE 3306