Consul Package
=========

A HashiCorp Consul package build project because debian packages are not provided.

More information about Consul
https://www.consul.io

#### Instructions

*Setup Build System*
```
sudo apt-get install ruby2.3 ruby2.3-dev build-essential wget unzip
sudo gem install bundler
```
*Build Package*
```
cd <path to project>
rake
```

