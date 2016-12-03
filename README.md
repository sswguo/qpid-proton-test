## Qpid Proton install
### centos 7
$ yum groupinstall "Development tools"  <br />
[Offical install guide](https://git-wip-us.apache.org/repos/asf?p=qpid-proton.git;a=blob_plain;f=INSTALL.md;hb=0.15.0) <br />
[Ruby gem](https://github.com/apache/qpid-proton/tree/master/proton-c/bindings/ruby) <br />
[QPID Proton](https://docs.omniref.com/ruby/gems/qpid_proton/0.7.1/symbols/Qpid::Proton::Messenger#line=273)

### Server ActiveMQ
ActiveMQ 5.12 & 5.13 works well

### Client
Qpid-proton messenger - ruby
```
# required dependencies
$ yum install gcc cmake libuuid-devel
 
# dependencies needed for ssl support
$ yum install openssl-devel
 
# dependencies needed for Cyrus SASL support
$ yum install cyrus-sasl-devel
 
# dependencies needed for bindings
$ yum install swig          # Required for all bindings
$ yum install ruby-devel rubygem-rspec rubygem-simplecov 
```
From the directory where you found this README file:
```
$ mkdir build
$ cd build
 
# Set the install prefix. You may need to adjust depending on your
# system.
$ cmake .. -DCMAKE_INSTALL_PREFIX=/usr -DSYSINSTALL_BINDINGS=ON
 
# Omit the docs target if you do not wish to build or install
# documentation.
$ make all docs
 
# Note that this step will require root privileges.
$ make install
```
Install qpid-proton gem
https://github.com/apache/qpid-proton/tree/master/proton-c/bindings/ruby
```
gem build qpid_proton.gemspec
gem install qpid_proton-0.3.gem
```
Run test
https://github.com/apache/qpid-proton/tree/master/examples/ruby/messenger
```
ruby send.rb -a amqp://10.66.137.175:5672/proton "test"
ruby recv.rb amqp://10.66.137.175:5672/proton
```
### ssl configuration
Udpate send.rb & recv.rb to support ssl
```
messenger.certificate=("test.crt")
messenger.private_key=("test.key")
```

