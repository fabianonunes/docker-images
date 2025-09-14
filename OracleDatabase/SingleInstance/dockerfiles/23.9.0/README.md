lsnrctl status

docker build --force-rm=true --build-arg DB_EDITION=free -t localhost/oracle -f Containerfile.free .

docker build --build-arg BUILDKIT_SANDBOX_HOSTNAME="localhost" -t localhost/oracle-iso . --progress=plain