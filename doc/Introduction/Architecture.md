# 欺骗防御架构图
<div align=center>
<img src="https://www.showdoc.com.cn/server/api/attachment/visitfile/sign/cdf83206f5f771fade952bd1423cd01c" width=80% />

<img src="https://www.showdoc.com.cn/server/api/attachment/visitfile/sign/05fb9826a747c7d73248a4fd71e6f601" width=80% />
</div>

#### 欺骗防御由探针、蜜网、管理端三大部分组成：
- 探针Agent，支持透明代理管理，诱饵、密签安装(业务服务器上安装)
- 管理端，支持密网管理、探针管理、诱饵管理、密签管理，攻击溯源
- 密网，支持通过容器API向(蜜罐)部署诱饵和密签 ；支持动态创建容器，销毁容器等。支持协议代理管理

#### 部署欺骗生产部署需要做网络访问控制：
- 探针服务器可以向蜜网发送网络请求，为防止黑客在攻击蜜罐后横向渗透探针，蜜网无法向探针发送网络请求。
- 蜜网服务器可以向蜜罐发送请求，蜜罐不允许访问蜜网服务器。
- 管理平台可以通过容器的API访问蜜罐，蜜罐不允许访问管理平台。

# 欺骗防御网络拓扑图
#### 欺骗网络采用分布式部署
<div align=center>
<img src="https://www.showdoc.com.cn/server/api/attachment/visitfile/sign/7542e77e6de406e24919b041e5d525b9" width=100% height=170px />
</div>

- 探针Agent，部署在业务服务器的Agent，通过探针管理透明代理、密签、诱饵，支持动态策略，通过redis更新策略
- 透明代理，部署在业务主机上(探针的一个模块)，模拟业务端口
- 协议代理，部署在蜜罐与透明代理之间，记录攻击行为
- 欺骗网络，基于云原生k3s构建欺骗网络，可以是单台服务器也可以是服务器集群，可以根据实际场景进行部署。在蜜网内的服务器上部署探针Agent，可以通过协议代理技术，将透明代理转发过来的流量，根据协议类型代理到蜜罐中去，使黑客最终攻击的是蜜罐环境。蜜网内的服务器还部署了falco监控模块，可以监控蜜罐中文件的变化，及时取出黑客攻击产生的文件进行监测，分析是否有病毒或者新的木马样本。

# 欺骗防御技术全景图
<div align=center>
<img src="https://www.showdoc.com.cn/server/api/attachment/visitfile/sign/0c4adf83667eb669a831d6b7971433be" width=60% height=350px/>
</div>
