# socker
docker container for building sbt + nodejs projects - support run docker directly in container.

## features
socker support building project by providing the following features:
+ pre-installed node.js with some global npm packages: bower, gulp, grunt, typings
+ "docker in docker":
  Just mount `-v /var/run/docker.sock:/var/run/docker.sock` to have all docker's stuff directly in the container.
+ pre-installed sbt 0.13.11
+ pre-configured user with `sudo` right.

## using example
1. Run:
```
cd /home/giabao/wd/sd-api/
docker run --rm -it \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v $HOME/.ivy2:/home/sandinh/.ivy2 \
    -v $PWD:/src \
    sandinh/sbt-node
```
Or, you can use the [socker](socker) script:
```
sudo wget -O /usr/bin/socker https://raw.githubusercontent.com/giabao/docker-sbt-node/master/socker
sudo chmod a+x /usr/bin/socker
socker /home/giabao/wd/sd-api/
```
2. Then run in the container:
```
npm install
bower install
typings install
gulp release
sbt clean
sbt docker:publishLocal
```

## using in Windows / OSX
1. Your source code should be in /Users (OS X) or C:\Users (Windows) directory.
This will simplify the mounting process. See [Mount a host directory as a data volume](https://docs.docker.com/engine/userguide/containers/dockervolumes/#mount-a-host-directory-as-a-data-volume)
Else, you can create an Auto-mount & Permanent shared folder for the `default` machine in VirtualBox UI.
Ex: name = wd, path = D:\wd
Then, [run in Docker Terminal](https://github.com/docker/machine/issues/1814#issuecomment-152739994):
```
docker-machine ssh default 'sudo mkdir -p //wd && sudo mount -t vboxsf  wd //wd'
```

2. Ensure that in Docker Terminal, you can run `docker info`.
If not, pls exec steps 5, 6 at [docker-machine env](https://docs.docker.com/machine/get-started/#create-a-machine)

3. run socker
```
socker /home/giabao/wd/sd-api/
```

## build docker image locally
```
git clone git@github.com:giabao/docker-sbt-node.git
docker build --rm -t sandinh/sbt-node docker-sbt-node
```

## Licence
This software is licensed under the Apache 2 license:
http://www.apache.org/licenses/LICENSE-2.0

Copyright 2016 Sân Đình (http://sandinh.com)

## misc
This project is inspired by the following document: [Continuos Integration and Deployment with Rancher and Docker](https://cdn2.hubspot.net/hubfs/468859/Continuous_Integration_and_Deployment_with_Rancher_and_Docker.pdf)
