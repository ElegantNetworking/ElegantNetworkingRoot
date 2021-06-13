# ElegantNetworking

ElegantNetworking is a packet system framework which seeks to make client-server interaction in MinecraftForge mods as easy as possible. 

Example of packet declaration:
```java
@ElegantPacket
@Value //lombok
public class PacketExample implements ClientToServerPacket {
    int someValue1;
    MyClass someValue2;
    IMyInterface someValue3;
    Map<String, MyClass> someValue4;

    @Override
    public void onReceive(EntityPlayerMP player, INetHandlerPlayServer handler) {
        //packet handling
    }
}
```
And usage of packet:
```java
new PacketExample(1, new MyClass(...), new ImplOfMyInterface(...), ImmutableMap.of(...)).sendToServer();
```
And that's all you need!

## General features
+ Auto-registration of packets
  * enough to mark packet class by annotation
  * you can forget about the channel and packet id's
+ Automatic serialization/deserialization
  * supported primitives, collections, data-classes(POJO) and algebric data types overall
  * exists able to override logic of serialization
+ Sendable data structure and receiving handler is localised in one place
  * one packet - one class
+ Design of api is offers to do not depend on the version of Minecraft
+ Compatible with obfuscators(less tested)

## Getting started
### Adding to project
1. Add to build.gradle:
```groowy
dependencies {
    compile "io.gitlab.hohserg.elegant.networking:elegant-networking-1.12:2.4"
    annotationProcessor  "io.gitlab.hohserg.elegant.networking:annotation-processor:2.7"   //use compileOnly or `apt` plugin for gradle less that 4.8
}
```
2. Perform Gradle refresh in your ide
3. If you using Lombok than annotation processor of Lombok must be applied before annotation processor of ElegantNetworking, like:
```groowy
annotationProcessor 'org.projectlombok:lombok:1.18.8', "io.gitlab.hohserg.elegant.networking:annotation-processor:2.7"
```
### Creating packet
1. Create new class for packet

If your packet must be sended from client to server than it must implement ClientToServerPacket

If your packet must be sended from server to client than it must implement ServerToClientPacket

2. `onReceive` method will be called when packet is received

3. Mark you packet class with @ElegantPacket annotation

Optionaly, you can specify a channel name for packet (have sense if your jar contains more that one mod)

4. Add fields that represent transmitted information

Able to use Lombok to generate useful constructor, getters and setters

5 Optionaly, you can override logic of serialization. 

Override `serialize` method, it must write data to `ByteBuf` argument.

And add a unserialization-constructor with single argument `ByteBuf`, it must read data from `ByteBuf`.

Example:
```java
@ElegantPacket
@Value
public class ExamplePacket implements ServerToClientPacket {
    int someInt;
    Map<MyClass, String> someMap;

    @Override
    public void onReceive(Minecraft mc) {
        System.out.println("test "+someInt+" "+someMap);
    }
}
```
#### Sending packet
Just create instance of your packet and call one of send-methods
```java
//Sending `someInt=1` and `someMap={new MyClass("test") -> "lol"}` to all players in dimension `world`
new ExamplePacket(1, ImmutableMap.of(new MyClass("test"), "lol")).sendToDimension(world);
```

## How to setup workspace for contrubuting
1. Create folder, i.e. `ElegantNetworking`
2. `git clone https://github.com/ElegantNetworking/ElegantNetworkingRoot.git`
3. `git clone https://github.com/ElegantNetworking/ElegantNetworkingCommon.git`
4. `git clone https://github.com/ElegantNetworking/ElegantNetworkingAnnotationProcessor.git`
5. `git clone https://github.com/ElegantNetworking/ElegantNetworking_1.16.git`
6. Import `ElegantNetworkingRoot` gradle project into your IDE
7. `cd ElegantNetworkingRoot`
8. Use `gradlew :ElegantNetworking_1.16:runClient` for run game
9. Use `gradlew :ElegantNetworking_1.16:build` for build runtime library
10. Use `gradlew :ElegantNetworkingAnnotationProcessor:shadowJar` for build annotation processor

## Gratitudes
Thx @Dahaka934 for discussion and review

Thx @tox1cozZ for drew my attention to the annotation processors

Thx @Plasticable for advice of using gradle 4.4.1 (not actualy, but helpful in due time)

Thx @Icosider for consulting about gradle configuration

Thx @AmaZ1nG for idea about serialization of nbt and other basic types that useful in modding

Thx @CDAGaming for the fork of FG2.1 that compatible with gradle 5+

Thx @Liahim85 for pretty logo
