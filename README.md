Author Pablo Pérez García 

![My image](src/main/resources/img/simple.svg)

## Reactive RPC

Here we cover with some examples and explanations how most famous RPC as [gRPC](https://grpc.io/docs/quickstart/) or
 [Thrift](https://thrift.apache.org/) works.

### gRPC

##### Simple gRCP

![My image](src/main/resources/img/grpc.png)

An example of how gRPC works between client-server

* [client](src/main/java/com/politrons/grpc/simple/RpcClient.java)

* [Service](src/main/java/com/politrons/grpc/simple/RpcServiceImpl.java)

* [proto](src/main/proto/rpc_contract.proto)

##### Reactive

![My image](src/main/resources/img/flatMap.png)

An example of how to use streams gRPC between client-server

* [client](src/main/java/com/politrons/grpc/reactive/ReactiveClient.java)

* [service](src/main/java/com/politrons/grpc/reactive/ReactiveServiceImpl.java)

* [proto](src/main/proto/rpc_reactive.proto)

##### Configuration

Once that you have your contracts(proto) ready, you need to build your classes which will 
be used for the communication between client and server.
In these examples we decide to use the maven plugin.

The plugin you need to add in your pom is

```
  <plugin>
         <groupId>org.xolstice.maven.plugins</groupId>
         <artifactId>protobuf-maven-plugin</artifactId>
         <version>0.5.0</version>
         <configuration>
              <protocArtifact>
                        com.google.protobuf:protoc:3.3.0:exe:${os.detected.classifier}
              </protocArtifact>
              <pluginId>grpc-java</pluginId>
              <pluginArtifact>
                        io.grpc:protoc-gen-grpc-java:1.4.0:exe:${os.detected.classifier}
              </pluginArtifact>
              </configuration>
              <executions>
                    <execution>
                        <goals>
                            <goal>compile</goal>
                            <goal>compile-custom</goal>
                        </goals>
                    </execution>
              </executions>
  </plugin>


```

### Thrift

An example of how thrift RPC works between client-server

* [client](src/main/scala/finagle/thrift/rpc/ThriftRPCClient.scala)

* [Service](src/main/scala/finagle/thrift/rpc/ThriftRPCServer.scala)

* [thrift](src/main/scala/finagle/thrift/idl/finagle_scrooge.thrift)


## Benchmarks

![My image](src/main/resources/img/benchmark.png)

For this benchmark we made 10k request in 10 iterations using Rest and gRPC, and get the response time.

* [Rest](src/main/scala/benchmark) Http finagle client against Grizzly server.

* [Rest](src/main/scala/benchmark) Http finagle client against Finagle server.

* [gRPC](src/main/java/com/politrons/grpc/benchmark/regular) using standard implementation.

* [gRPC Reactive](src/main/java/com/politrons/grpc/benchmark/reactive) using reactive StreamObserver.

##### Results

```.bash
Rest Grizzly - response time:2764 millis
Rest Finagle - response time:2710 millis
RPC regular  - response time:2554 millis
RPC Reactive - response time:4 millis
```
