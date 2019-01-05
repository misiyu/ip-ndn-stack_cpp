# ip-ndn-stack_cpp
> ## 最小原型网络环境配置
> - 该配置临时生效，电脑重启之后会丢失
> - pkun3(192.169.1.1) -> pkun1(192.169.1.2) -> pkun2(192.169.1.3) -> hitn1(192.169.1.4)

- ### 确保各NDN主机的nfd进程已经启动
  > 在本最小原型场景下，pkun1和pkun2为NDN主机，pkun3和hitn1为IP主机

  NDN主机的环境配置以及Face创建等，参见[IP_NDN_STACK 部署脚本](https://github.com/SunnyQjm/ip-ndn-stack_cpp/tree/master/deployment)

  ```bash
  nfd-status
  
  # 如果没有启动则执行下面的命令启动
  nfd-start
  ```
- ### 配置ip及网关
  - pkun3
    ```bash
    sudo ifconfig enp1s0f0 192.169.1.1
    sudo route add -host 192.169.1.2 dev enp1s0f0
    sudo route add -net 192.169.1.0/24 dev enp1s0f0 gw 192.169.1.2
    ```
  - pkun1
    ```bash
    sudo ifconfig enp2s0f0 192.169.1.2
    sudo route add -host 192.169.1.1 dev enp2s0f0
    ```
  - pkun2
    ```bash
    sudo ifconfig enp6s0f1 192.169.1.3
    sudo route add -host 192.169.1.4 dev enp6s0f1
    ```
  - hitn1
    ```bash
    sudo ifconfig enp1s0f1 192.169.1.4
    sudo route add -host 192.169.1.3 dev enp1s0f1
    sudo route add -net 192.169.1.0/24 dev enp1s0f1 gw 192.169.1.3
    ```
  
  - 测试
    在pkun1上执行：
    ```bash
    ping 192.169.1.2
    ```
    在hitn1上执行：
    ```bash
    ping 192.169.1.3
    ```

- ### 配置nfd静态路由
    > 静态路由的配置，可以像下面这样手动配置，也可以使用协助脚本，详情参见[IP_NDN_STACK 部署脚本](https://github.com/SunnyQjm/ip-ndn-stack_cpp/tree/master/deployment)
    
  - pkun1
    ```bash
    sudo nfdc route add prefix /IP/pre/192.169.1.4 nexthop <faceid>
    sudo nfdc route add prefix /IP/192.169.1.4 nexthop <faceid>
    sudo nfdc route add prefix /IP/TCP/pre/192.169.1.4 nexthop <faceid>
    sudo nfdc route add prefix /IP/TCP/192.169.1.4 nexthop <faceid>
    ```
  - pkun2
    ```bash
    sudo nfdc route add prefix /IP/pre/192.169.1.1 nexthop <faceid>
    sudo nfdc route add prefix /IP/192.169.1.1 nexthop <faceid>
    sudo nfdc route add prefix /IP/TCP/pre/192.169.1.1 nexthop <faceid>
    sudo nfdc route add prefix /IP/TCP/192.169.1.1 nexthop <faceid>
    ```

> ## 编译项目

- ### 首先用cmake生成Makefile
    进入项目根目录，然后执行下面命令(如果没有安装cmake，请自行安装)
    ```bash
    cmake .
    ```
- ### 然后编译项目
    ```bash
    make
    ```
- ### 运行
    ```bash
    sudo ./ip_ndn_stack_cpp <config file path>

    # for example
    sudo ./ip_ndn_stack_cpp ./config/part1.json
    ```

> ## 参与项目

- ### 拉取项目到本地
  ```bash
  git clone https://github.com/SunnyQjm/ip-ndn-stack_cpp.git
  ```

- ### 从github上拉取最新的代码
  首先，进入项目的根目录
  ```bash
  cd ip-ndn-stack_cpp
  ```
  然后执行git pull
  ```bash
  git pull
  ```

- ### 添加了新代码，要推送到github上
  ```bash
  git add <filename>
  
  git commit -m <这里填写更新说明>
  
  git push 
  ```
