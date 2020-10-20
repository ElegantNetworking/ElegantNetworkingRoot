# ElegantNetworking

ElegantNetworking is a packet system framework which seeks to make client-server interaction in MinecraftForge mods as easy as possible. 

Example of packet declaration:
```java
@ElegantPacket
@Value //lombok
public class PacketExample implements ClientToServerPacket {
    int someValue1;
    MyClass someValue2;
    Map<String, MyClass> someValue3;

    @Override
    public void onReceive(EntityPlayerMP player, INetHandlerPlayServer handler) {
        //packet handling
    }
}
```
And usage of packet:
```java
new PacketExample(1, new MyClass(...), ImmutableMap.of(...)).sendToServer();
```
And that's all you need!

## General features
+ Auto-registration of packets
  * enough to mark packet class by annotation
  * you can forget about the channel and packet id's
+ Automatic serialization/deserialization
  * supported primitives, collections, data-classes(POJO) and algebric data types overall
  * exists able to override logic of serialization
+ Structure of sendable data and receiving handler is localised in one place
  * one packet - one class
+ Design of api is offers to do not depend on the version of Minecraft
+ Compatible with obfuscators(less tested)

## How to setup workspace for contrubuting
1. Create folder, i.e. `ElegantNetworking`
2. `git clone https://github.com/ElegantNetworking/ElegantNetworkingRoot.git`
3. `git clone https://github.com/ElegantNetworking/ElegantNetworkingCommon.git`
4. `git clone https://github.com/ElegantNetworking/ElegantNetworkingAnnotationProcessor.git`
5. `git clone https://github.com/ElegantNetworking/ElegantNetworking_1.12.git`
6. `cd ElegantNetworkingRoot`
7. `gradlew :ElegantNetworking_1.12:setupDecompWorkspace`
8. Import `ElegantNetworkingRoot` gradle project into your IDE
9. ???
10. Use `gradlew :ElegantNetworking_1.12:runClient` for run game
11. Use `gradlew :ElegantNetworking_1.12:build` for build runtime library
12. Use `gradlew :ElegantNetworkingAnnotationProcessor:shadowJar` for build annotation processor
