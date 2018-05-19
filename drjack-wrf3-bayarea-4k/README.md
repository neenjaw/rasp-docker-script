This is a BayArea region in 4 km resolution

# Building
Make sure to download geog.tar.gz into current directory from here: https://www.dropbox.com/s/x6wcsm7c26vt67a/geog.tar.gz?dl=0
Then run:

```
docker build -t my-rasp-bayarea-4k .
```

# Running
## Run the current day (or next if it's end of the day)

```
$ docker run -v /tmp/OUT:/root/rasp/BAYAREA/OUT/ -v /tmp/LOG:/root/rasp/BAYAREA/LOG/  --rm my-rasp-bayarea-4k
```

## Run the current day +1, +2, etc

START_HOUR environment variable can override default start hour which is 12. See ![rasp.run.parameters.BAYAREA](BAYAREA/rasp.run.parameters.BAYAREA)

* START_HOUR=36 for current day +1
* START_HOUR=60 for current day +2, etc

```
docker run -v /tmp/OUT:/root/rasp/BAYAREA/OUT/ -v /tmp/LOG:/root/rasp/BAYAREA/LOG/ --rm -e START_HOUR=36 my-rasp-bayarea-4k
```