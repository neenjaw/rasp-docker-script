This is a Saskatchewan region in 4 km resolution

# Building
Make sure to download geog.tar.gz into current directory from here: https://www.dropbox.com/s/x6wcsm7c26vt67a/geog.tar.gz?dl=0
Then run:

```
docker build -f Dockerfile.local -t my-rasp-sask-4k .
```

# Extract cloud build context
```
mkdir ./Dockercloud/Dockercloud-context
docker container create --name extract my-rasp-sask-4k
docker container cp extract:/root/rasp/SASK ./Dockercloud/Dockercloud-context
docker container rm -f extract
find Dockercloud/Dockercloud-context -type l -exec rm -f {} \;
cp SASK/rasp.region_data.ncl ./Dockercloud/Dockercloud-context
cp SASK/rasp.site.runenvironment ./Dockercloud/Dockercloud-context
```

# Local slim build
```
docker build -f Dockercloud/Dockerfile.cloud -t my-rasp-sask-4k-slim Dockercloud
```

# Local Cloud build test
```
cd Dockercloud && container-builder-local --config=cloudbuild.yaml --dryrun=false .
```

# Cloud slim build
```
cd Dockercloud && gcloud container builds submit --config cloudbuild.yaml .
```

# Running
## Run the current day (or next if it's end of the day)

```
$ docker run -v /tmp/OUT:/root/rasp/SASK/OUT/ -v /tmp/LOG:/root/rasp/SASK/LOG/  --rm my-rasp-sask-4k
```

## Run the current day +1, +2, etc

START_HOUR environment variable can override default start hour which is 9. See ![rasp.run.parameters.SASK](SASK/rasp.run.parameters.SASK)

* START_HOUR=33 for current day +1
* START_HOUR=57 for current day +2, etc

```
docker run -v /tmp/OUT:/root/rasp/SASK/OUT/ -v /tmp/LOG:/root/rasp/SASK/LOG/ --rm -e START_HOUR=33 my-rasp-sask-4k
```
