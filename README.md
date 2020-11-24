# 戴尔R740XD安装centos7.8

## 使用U盘安装

### 使用U盘安装Centos7  

  1. 首先制作好U盘启动的IOS  
  2. 按下开机按钮开启出现选择画面后按F11键  
  3. 进入启动项，选择One-shot UEFI Boot Menu  
  4. 选择Disk connected to front USB 2   
  5. 机器会提示安装Centos 系统 这个时候光标移动到第一个启动选，然后输入e,进入到编辑启动编辑模式。修改启动项为U盘启动hd:LABLE=之后更名为与U盘盘符相同的名称  

  6. 然后Ctrl+x启动进行安装    


### 首次安装配置raid阵列卡
  1. 开机，按F2进入BIOS管理界面  
  2. 选择第三个选项 Device Settings 双击或者回车键进入  
  3. 双击或者回车进入第一个选项，上面会显示阵列卡型号（这里是H740P），如果没有显示阵列卡，可能是阵列卡没有安装好或者故障了。  
  4. 选第一个选项，Main Menu  
  5. 选择第一个选项 Configuration Management  
  6. 选择第二个选项  Create  Virtual Disk。选项一是自动将所有硬盘挂为RAID0，一般不会用到，这里就不赘述了。  
  7. 选择RAID级别（0、1、5、6、10）都是支持的  
  8. 选好RAID级别后，按下面的Select Physical Disks   
  9. 选择需要做阵列的硬盘，如果所有硬盘做1个阵列，选择下面的check all，阵列卡会自动选择所有硬盘。如果不是所有硬盘都做RAID，那就选择自己需要做阵列的硬盘即可。补充一点，如果是要做热备盘，需要在这里把热备盘预留出来，不要选择。最后必须选择Apply Changes，这样才算是确认完成。  
  10. 成功后点OK返回上一层菜单   
  11. Virtual Disk Name 是阵列名称，方便多个LUN时候分辨不同的LUN，名字自己随便取或者留空都可以其他参数一般不需要特殊设置，默认就好。      
  12. 选择上述步骤所有参数后，下拉菜单到最后，选择Create Virtue Disk，必须选择，不然所有做的都白费，系统不会自动保存信息的。   
  13. 在Confirm处打上勾，然后选YES。这里系统提示创建阵列会擦写硬盘上所有数据，不能恢复。     

  14. 然后我们可以返回阵列卡主菜单，检查一下阵列是否已经做好。选择第三个菜单，Virtual Disk Management.


### 系统命令配置
  - 查看系统内核版本信息：
  ```  
  uname -a
  cat /proc/version
  ```
  - 查看网卡设备信息：
  ```  
  lspci |grep Ethernet
  ```
  - 配置网卡信息：
  ```  
  cd /etc/sysconfig/network-scripts/
  vim em1

  ONBOOT="yes" //修改开机自启用网口
  IPADDR="192.168.72.29"
  PREFIX="24"
  GATEWAY="192.168.72.1"
  DNS1="222.172.200.68"
  ```
  - 重启网络服务   
  ```
  systemctl start network
  systemctl enable network
  ```
  - 防火墙开启、关闭、禁用命令
  ```
  设置开机启用防火墙：systemctl enable firewalld.service
  设置开机禁用防火墙：systemctl disable firewalld.service
  启动防火墙：systemctl start firewalld
  关闭防火墙：systemctl stop firewalld
  检查防火墙状态：systemctl status firewalld
  ```
  - 关闭selinux  , 将SELINUX=enforcing改为SELINUX=disabled   
  ```
  vi   /etc/selinux/config

  SELINUX=disable
  ```
