# Netty MQTT

thingboard 内部MQTT实现主要是通过`io.netty`实现

## 引入相应maven

```xml
		<dependency>
			<groupId>io.netty</groupId>
			<artifactId>netty-handler</artifactId>
		</dependency>
		<dependency>
			<groupId>io.netty</groupId>
			<artifactId>netty-codec-mqtt</artifactId>
		</dependency>
		<dependency>
			<groupId>io.netty</groupId>
			<artifactId>netty-codec-http</artifactId>
		</dependency>
```
## 构建netty服务

需要一个netty服务启动类
```java
 /**
  * Copyright (c) 2018, Mr.Wang (recallcode@aliyun.com) All rights reserved.
  */
 
 package cn.recallcode.iot.mqtt.server.broker.server;
 
 import cn.recallcode.iot.mqtt.server.broker.codec.MqttWebSocketCodec;
 import cn.recallcode.iot.mqtt.server.broker.config.BrokerProperties;
 import cn.recallcode.iot.mqtt.server.broker.handler.BrokerHandler;
 import cn.recallcode.iot.mqtt.server.broker.protocol.ProtocolProcess;
 import io.netty.bootstrap.ServerBootstrap;
 import io.netty.channel.Channel;
 import io.netty.channel.ChannelInitializer;
 import io.netty.channel.ChannelOption;
 import io.netty.channel.ChannelPipeline;
 import io.netty.channel.EventLoopGroup;
 import io.netty.channel.epoll.EpollEventLoopGroup;
 import io.netty.channel.epoll.EpollServerSocketChannel;
 import io.netty.channel.nio.NioEventLoopGroup;
 import io.netty.channel.socket.SocketChannel;
 import io.netty.channel.socket.nio.NioServerSocketChannel;
 import io.netty.handler.codec.http.HttpContentCompressor;
 import io.netty.handler.codec.http.HttpObjectAggregator;
 import io.netty.handler.codec.http.HttpServerCodec;
 import io.netty.handler.codec.http.websocketx.WebSocketServerProtocolHandler;
 import io.netty.handler.codec.mqtt.MqttDecoder;
 import io.netty.handler.codec.mqtt.MqttEncoder;
 import io.netty.handler.logging.LogLevel;
 import io.netty.handler.logging.LoggingHandler;
 import io.netty.handler.ssl.SslContext;
 import io.netty.handler.ssl.SslContextBuilder;
 import io.netty.handler.ssl.SslHandler;
 import io.netty.handler.timeout.IdleStateHandler;
 import org.slf4j.Logger;
 import org.slf4j.LoggerFactory;
 import org.springframework.beans.factory.annotation.Autowired;
 import org.springframework.stereotype.Component;
 
 import javax.annotation.PostConstruct;
 import javax.annotation.PreDestroy;
 import javax.net.ssl.KeyManagerFactory;
 import javax.net.ssl.SSLEngine;
 import java.io.InputStream;
 import java.security.KeyStore;
 
 /**
  * Netty启动Broker
  */
 @Component
 public class BrokerServer {
 
 	private static final Logger LOGGER = LoggerFactory.getLogger(BrokerServer.class);
 
 	@Autowired
 	private BrokerProperties brokerProperties;
 
 	@Autowired
 	private ProtocolProcess protocolProcess;
 
 	private EventLoopGroup bossGroup;
 
 	private EventLoopGroup workerGroup;
 
 	private SslContext sslContext;
 
 	private Channel channel;
 
 	private Channel websocketChannel;
 
 	@PostConstruct
 	public void start() throws Exception {
 		LOGGER.info("Initializing {} MQTT Broker ...", "[" + brokerProperties.getId() + "]");
 		bossGroup = brokerProperties.isUseEpoll() ? new EpollEventLoopGroup() : new NioEventLoopGroup();
 		workerGroup = brokerProperties.isUseEpoll() ? new EpollEventLoopGroup() : new NioEventLoopGroup();
 		KeyStore keyStore = KeyStore.getInstance("PKCS12");
 		InputStream inputStream = this.getClass().getClassLoader().getResourceAsStream("keystore/mqtt-broker.pfx");
 		keyStore.load(inputStream, brokerProperties.getSslPassword().toCharArray());
 		KeyManagerFactory kmf = KeyManagerFactory.getInstance("SunX509");
 		kmf.init(keyStore, brokerProperties.getSslPassword().toCharArray());
 		sslContext = SslContextBuilder.forServer(kmf).build();
 		mqttServer();
 		websocketServer();
 		LOGGER.info("MQTT Broker {} is up and running. Open SSLPort: {} WebSocketSSLPort: {}", "[" + brokerProperties.getId() + "]", brokerProperties.getSslPort(), brokerProperties.getWebsocketSslPort());
 	}
 
 	@PreDestroy
 	public void stop() {
 		LOGGER.info("Shutdown {} MQTT Broker ...", "[" + brokerProperties.getId() + "]");
 		bossGroup.shutdownGracefully();
 		bossGroup = null;
 		workerGroup.shutdownGracefully();
 		workerGroup = null;
 		channel.closeFuture().syncUninterruptibly();
 		channel = null;
 		websocketChannel.closeFuture().syncUninterruptibly();
 		websocketChannel = null;
 		LOGGER.info("MQTT Broker {} shutdown finish.", "[" + brokerProperties.getId() + "]");
 	}
 
 	private void mqttServer() throws Exception {
 		ServerBootstrap sb = new ServerBootstrap();
 		sb.group(bossGroup, workerGroup)
 			.channel(brokerProperties.isUseEpoll() ? EpollServerSocketChannel.class : NioServerSocketChannel.class)
 			// handler在初始化时就会执行
 			.handler(new LoggingHandler(LogLevel.INFO))
 			// childHandler会在客户端成功connect后才执行
 			.childHandler(new ChannelInitializer<SocketChannel>() {
 				@Override
 				protected void initChannel(SocketChannel socketChannel) throws Exception {
 					ChannelPipeline channelPipeline = socketChannel.pipeline();
 					// Netty提供的心跳检测
 					channelPipeline.addFirst("idle", new IdleStateHandler(0, 0, brokerProperties.getKeepAlive()));
 //					// Netty提供的SSL处理
 					SSLEngine sslEngine = sslContext.newEngine(socketChannel.alloc());
 					sslEngine.setUseClientMode(false);        // 服务端模式
 					sslEngine.setNeedClientAuth(false);        // 不需要验证客户端
 //					channelPipeline.addLast("ssl", new SslHandler(sslEngine));
 					channelPipeline.addLast("decoder", new MqttDecoder());
 					channelPipeline.addLast("encoder", MqttEncoder.INSTANCE);
 					channelPipeline.addLast("broker", new BrokerHandler(protocolProcess));
 				}
 			})
 			.option(ChannelOption.SO_BACKLOG, brokerProperties.getSoBacklog())
 			.childOption(ChannelOption.SO_KEEPALIVE, brokerProperties.isSoKeepAlive());
 		channel = sb.bind(brokerProperties.getSslPort()).sync().channel();
 	}
 
 
 
 }

```

通过扫描@Component  加上@PostConstruct注解 项目运行时加载

## 继承SimpleChannelInboundHandler用于通道解析 重写 channelRead0方法分析消息头的类型

```java
/**
 * Copyright (c) 2018, Mr.Wang (recallcode@aliyun.com) All rights reserved.
 */

package cn.recallcode.iot.mqtt.server.broker.handler;

import cn.recallcode.iot.mqtt.server.broker.protocol.ProtocolProcess;
import cn.recallcode.iot.mqtt.server.common.session.SessionStore;
import io.netty.channel.Channel;
import io.netty.channel.ChannelHandlerContext;
import io.netty.channel.SimpleChannelInboundHandler;
import io.netty.handler.codec.mqtt.MqttConnectMessage;
import io.netty.handler.codec.mqtt.MqttMessage;
import io.netty.handler.codec.mqtt.MqttMessageIdVariableHeader;
import io.netty.handler.codec.mqtt.MqttPublishMessage;
import io.netty.handler.codec.mqtt.MqttSubscribeMessage;
import io.netty.handler.codec.mqtt.MqttUnsubscribeMessage;
import io.netty.handler.timeout.IdleState;
import io.netty.handler.timeout.IdleStateEvent;
import io.netty.util.AttributeKey;

import java.io.IOException;

/**
 * MQTT消息处理
 */
public class BrokerHandler extends SimpleChannelInboundHandler<MqttMessage> {

	private ProtocolProcess protocolProcess;

	public BrokerHandler(ProtocolProcess protocolProcess) {
		this.protocolProcess = protocolProcess;
	}

	@Override
	protected void channelRead0(ChannelHandlerContext ctx, MqttMessage msg) throws Exception {
		switch (msg.fixedHeader().messageType()) {
			case CONNECT:
				protocolProcess.connect().processConnect(ctx.channel(), (MqttConnectMessage) msg);
				break;
			case CONNACK:
				break;
			case PUBLISH:
				protocolProcess.publish().processPublish(ctx.channel(), (MqttPublishMessage) msg);
				break;
			case PUBACK:
				protocolProcess.pubAck().processPubAck(ctx.channel(), (MqttMessageIdVariableHeader) msg.variableHeader());
				break;
			case PUBREC:
				protocolProcess.pubRec().processPubRec(ctx.channel(), (MqttMessageIdVariableHeader) msg.variableHeader());
				break;
			case PUBREL:
				protocolProcess.pubRel().processPubRel(ctx.channel(), (MqttMessageIdVariableHeader) msg.variableHeader());
				break;
			case PUBCOMP:
				protocolProcess.pubComp().processPubComp(ctx.channel(), (MqttMessageIdVariableHeader) msg.variableHeader());
				break;
			case SUBSCRIBE:
				protocolProcess.subscribe().processSubscribe(ctx.channel(), (MqttSubscribeMessage) msg);
				break;
			case SUBACK:
				break;
			case UNSUBSCRIBE:
				protocolProcess.unSubscribe().processUnSubscribe(ctx.channel(), (MqttUnsubscribeMessage) msg);
				break;
			case UNSUBACK:
				break;
			case PINGREQ:
				protocolProcess.pingReq().processPingReq(ctx.channel(), msg);
				break;
			case PINGRESP:
				break;
			case DISCONNECT:
				protocolProcess.disConnect().processDisConnect(ctx.channel(), msg);
				break;
			default:
				break;
		}
	}

	@Override
	public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause) throws Exception {
		if (cause instanceof IOException) {
			// 远程主机强迫关闭了一个现有的连接的异常
			ctx.close();
		} else {
			super.exceptionCaught(ctx, cause);
		}
	}

	@Override
	public void userEventTriggered(ChannelHandlerContext ctx, Object evt) throws Exception {
		if (evt instanceof IdleStateEvent) {
			IdleStateEvent idleStateEvent = (IdleStateEvent) evt;
			if (idleStateEvent.state() == IdleState.ALL_IDLE) {
				Channel channel = ctx.channel();
				String clientId = (String) channel.attr(AttributeKey.valueOf("clientId")).get();
				// 发送遗嘱消息
				if (this.protocolProcess.getSessionStoreService().containsKey(clientId)) {
					SessionStore sessionStore = this.protocolProcess.getSessionStoreService().get(clientId);
					if (sessionStore.getWillMessage() != null) {
						this.protocolProcess.publish().processPublish(ctx.channel(), sessionStore.getWillMessage());
					}
				}
				ctx.close();
			}
		} else {
			super.userEventTriggered(ctx, evt);
		}
	}
}

```

每个 MQTT 命令消息(MqttMessage msg)的消息头都包含一个固定的头(fixedHeader)。每个头都有对应的消息类型(messageType)

http://public.dhe.ibm.com/software/dw/webservices/ws-mqtt/mqtt-v3r1.html#fixed-header

| Mnemonic    | Enumeration | Description                                |
| ----------- | ----------- | ------------------------------------------ |
| Reserved    | 0           | Reserved                                   |
| CONNECT     | 1           | Client request to connect to Server        |
| CONNACK     | 2           | Connect Acknowledgment                     |
| PUBLISH     | 3           | Publish message                            |
| PUBACK      | 4           | Publish Acknowledgment                     |
| PUBREC      | 5           | Publish Received (assured delivery part 1) |
| PUBREL      | 6           | Publish Release (assured delivery part 2)  |
| PUBCOMP     | 7           | Publish Complete (assured delivery part 3) |
| SUBSCRIBE   | 8           | Client Subscribe request                   |
| SUBACK      | 9           | Subscribe Acknowledgment                   |
| UNSUBSCRIBE | 10          | Client Unsubscribe request                 |
| UNSUBACK    | 11          | Unsubscribe Acknowledgment                 |
| PINGREQ     | 12          | PING Request                               |
| PINGRESP    | 13          | PING Response                              |
| DISCONNECT  | 14          | Client is Disconnecting                    |
| Reserved    | 15          | Reserved                                   |

根据对应的消息类型处理对应的业务

## 针对thingsboard主要处理的业务是CONNECT设备的连接鉴权和设备上报PUBLISH消息

### CONNECT消息主要是获取消主体中的客户端id和用户名密码进行连接鉴权
```java
public void processConnect(Channel channel, MqttConnectMessage msg){
        //客户端id
        String clientId=msg.payload().clientIdentifier();
// 用户名和密码验证, 这里要求客户端连接时必须提供用户名和密码, 不管是否设置用户名标志和密码标志为1, 此处没有参考标准协议实现
        String username = msg.payload().userName();
        String password = msg.payload().passwordInBytes() == null ? null : new String(msg.payload().passwordInBytes(), CharsetUtil.UTF_8);
}
```
### PUBLISH消息主要是获取消主体中的消息主题和消息的Bytes
```java
public void processPublish(Channel channel, MqttPublishMessage msg) {
//消息内容字节
byte[] messageBytes = new byte[msg.payload().readableBytes()];
//消息主题
String topicName =msg.variableHeader().topicName()
}
```
