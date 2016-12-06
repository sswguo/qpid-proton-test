## Qpid Proton install
### centos 7
$ yum groupinstall "Development tools"  <br />
[Offical install guide](https://git-wip-us.apache.org/repos/asf?p=qpid-proton.git;a=blob_plain;f=INSTALL.md;hb=0.15.0) <br />
[Ruby gem](https://github.com/apache/qpid-proton/tree/master/proton-c/bindings/ruby) <br />
[QPID Proton](https://docs.omniref.com/ruby/gems/qpid_proton/0.7.1/symbols/Qpid::Proton::Messenger#line=273)

### Server ActiveMQ
ActiveMQ [5.12](https://archive.apache.org/dist/activemq/5.12.0/apache-activemq-5.12.0-bin.tar.gz) & 5.13 works well

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
Run test with tracing log :  PN_TRACE_FRM=1 ./.rb
```
PN_TRACE_FRM=1 ruby send.rb -a amqp://10.66.137.175:5672/proton "test"
PN_TRACE_FRM=1 ruby recv.rb amqp://10.66.137.175:5672/proton
```
Log examples:
```
[0x1cd55c0]:  -> AMQP
[0x1cd55c0]:0 -> @open(16) [container-id="E920957F-22AD-454A-8FC2-A22A8AD0B446", hostname="10.66.137.175", channel-max=32767]
[0x1cd55c0]:0 -> @begin(17) [next-outgoing-id=0, incoming-window=2147483647, outgoing-window=2147483647]
[0x1cd55c0]:0 -> @attach(18) [name="proton", handle=0, role=true, snd-settle-mode=2, rcv-settle-mode=0, source=@source(40) [address="proton", durable=0, timeout=0, dynamic=false], target=@target(41) [address="proton", durable=0, timeout=0, dynamic=false], initial-delivery-count=0]
[0x1cd55c0]:0 -> @flow(19) [incoming-window=2147483647, next-outgoing-id=0, outgoing-window=2147483647, handle=0, delivery-count=0, link-credit=10, drain=false]
[0x1cd55c0]:  <- AMQP
[0x1cd55c0]:0 <- @open(16) [container-id="", hostname="", max-frame-size=4294967295, channel-max=32767, idle-time-out=15000, offered-capabilities=@PN_SYMBOL[:"ANONYMOUS-RELAY"], properties={:product="ActiveMQ", :"topic-prefix"="topic://", :"queue-prefix"="queue://", :version="5.13.0", :platform="Java/1.8.0_102"}]
[0x1cd55c0]:0 <- @begin(17) [remote-channel=0, next-outgoing-id=1, incoming-window=2147483647, outgoing-window=2147483647, handle-max=65535]
[0x1cd55c0]:0 <- @attach(18) [name="proton", handle=0, role=false, snd-settle-mode=2, rcv-settle-mode=0, source=@source(40) [address="proton"], target=@target(41) [address="proton"], incomplete-unsettled=false, initial-delivery-count=0]
[0x1cd55c0]:0 <- @transfer(20) [handle=0, delivery-id=0, delivery-tag=b"0", message-format=0] (226) "\x00Sp\xc0\x08\x05BP\x04@BR\x01\x00Sq\xc1'\x06\xa1\x04fold\xa1\x03yes\xa1\x07spindle\xa1\x02no\xa1\x08mutilate\xa1\x02no\x00Sr\xc1\x1e\x04\xa3\x07version\x82?\xf0\x00\x00\x00\x00\x00\x00\xa3\x04pill\xa1\x03RED\x00Ss\xc0K\x0c@@\xa1 amqp://10.66.137.175:5672/proton\xa1\x0cHow are you?@@@@\x83\x00\x00\x00\x00\x00\x00\x00\x00\x83\x00\x00\x00\x00\x00\x00\x00\x00@C\x00St\xc1(\x04\xa1\x04sent\x83\x00\x00\x00\x00X?\xd7\xe0\xa1\x08hostname\xa1\x0c755625e15b4f\x00Sw\xa1\x04test"

```

### ssl configuration
Udpate send.rb & recv.rb to support ssl
```
messenger.certificate=("test.crt")
messenger.private_key=("test.key")
```

