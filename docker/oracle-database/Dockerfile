FROM oracle/database:19.3.0-ee

ENV INIT_SGA_SIZE=2048
ENV INIT_PGA_SIZE=1024

ENV ORACLE_PDB=ORCLPDB1
ENV ORACLE_PWD=root

RUN rm $ORACLE_BASE/runUserScripts.sh
COPY runUserScripts.sh $ORACLE_BASE

COPY setup /opt/oracle/scripts/setup

EXPOSE 1521
