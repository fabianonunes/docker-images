# syntax=docker/dockerfile:1
FROM localhost/oracle:latest

ENV ORACLE_CHARACTERSET=WE8ISO8859P1
ENV ORACLE_PDB=FREEPDB1

WORKDIR /opt/oracle/
RUN <<EOT
  set -ex
  ./createDB.sh "$ORACLE_SID" "$ORACLE_PDB" "$ORACLE_PWD"
  echo "$(date -Iseconds)" > "$ORACLE_BASE"/oradata/.${ORACLE_SID}"${CHECKPOINT_FILE_EXTN}"
EOT

RUN <<EOT
  set -ex
  mkdir -p "$ORACLE_BASE"/oradata/dbconfig/"$ORACLE_SID"/
  ORACLE_BASE_CONFIG=$("$ORACLE_HOME"/bin/orabaseconfig)

  mv "$ORACLE_BASE_CONFIG"/dbs "$ORACLE_BASE"/oradata/dbconfig/"$ORACLE_SID"/
  mv "$ORACLE_HOME"/network/admin/sqlnet.ora "$ORACLE_BASE"/oradata/dbconfig/"$ORACLE_SID"/
  mv "$ORACLE_HOME"/network/admin/listener.ora "$ORACLE_BASE"/oradata/dbconfig/"$ORACLE_SID"/
  mv "$ORACLE_HOME"/network/admin/tnsnames.ora "$ORACLE_BASE"/oradata/dbconfig/"$ORACLE_SID"/
  find "$ORACLE_HOME"/install/ -name ".docker_*"  -exec  mv {} "$ORACLE_BASE"/oradata/dbconfig/"$ORACLE_SID"/  \;

  # oracle user does not have permissions in /etc, hence cp and not mv
  cp /etc/oratab "$ORACLE_BASE"/oradata/dbconfig/"$ORACLE_SID"/
  touch "${ORACLE_BASE}/oradata/${ORACLE_SID}/.prebuiltdb"

  sed 's/localhost/0.0.0.0/' -i /opt/oracle/oradata/dbconfig/FREE/listener.ora
EOT
