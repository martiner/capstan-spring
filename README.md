# Unikernel Java Application Example

Using [OSv](http://osv.io), [Capstan](https://github.com/cloudius-systems/capstan) and [Spring Boot](https://projects.spring.io/spring-boot/)

## Build

```bash
capstan build
```

## Run locally

```bash
capstan run -f 8080:8080 -f 8000:8000
curl localhost:8080
```


### Debug

1. add `-agentlib` argument as described in [`Capstan`](Capstan) file
1. rebuild
1. run with `-f 5005:5005` argument
1. attach debugger to port `5005`

## Run on Google Compute Engine

```bash
gcloud config set compute/region europe-west1
gcloud config set compute/zone europe-west1-b
export BUCKET=gs://geekosv

qemu-img convert -O raw ~/.capstan/repository/capstan-spring/capstan-spring.qemu disk.raw

tar czf capstan-spring.tar.gz disk.raw

gsutil mb $BUCKET

gsutil cp capstan-spring.tar.gz $BUCKET

gcloud compute images create capstan-spring --source-uri $BUCKET/capstan-spring.tar.gz

gcloud compute firewall-rules create default-allow-http-8080 --allow tcp:8080 --source-ranges 0.0.0.0/0

gcloud compute instances create demo --image capstan-spring --machine-type f1-micro

gcloud compute instances get-serial-port-output demo

curl INSTANCEIP:8080

gcloud compute instances delete demo
```

## Run on Amazon Web Services

Use [Unik](https://github.com/emc-advanced-dev/unik) (not covered here)
