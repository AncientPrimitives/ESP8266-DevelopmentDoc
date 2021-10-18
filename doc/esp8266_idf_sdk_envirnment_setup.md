# ESP8266类IDF SDK开发环境配置

# 操作系统
 - Debian/Ubuntu/Deepin/Raspbian
 - Windows
 - MacOS

# Debian/Ubuntu/Deepin/Raspbian
1. 安装开发工具包
   > sudo apt-get install git wget make libncurses-dev flex bison gperf python python-serial
2. 下载编译工具链
   - 创建编译工具链的存放目录```~/Libraries/ESP8266/toolchain/```
     > mkdir -p ~/Libraries/ESP8266/toolchain/
   - 下载编译工具链
     > wget https://dl.espressif.com/dl/xtensa-lx106-elf-gcc8_4_0-esp-2020r3-linux-amd64.tar.gz -o ~/Libraries/ESP8266/toolchain/
     - 如果使用的ESP8266_RTOS_SDK版本小于3.0，则下载
       - [linux(x86)](https://dl.espressif.com/dl/xtensa-lx106-elf-linux32-1.22.0-88-gde0bdc1-4.8.5.tar.gz)
       - [linux(x64)](https://dl.espressif.com/dl/xtensa-lx106-elf-linux64-1.22.0-88-gde0bdc1-4.8.5.tar.gz)
     - 否则下载
       - [linux(x86)](https://dl.espressif.com/dl/xtensa-lx106-elf-gcc8_4_0-esp-2020r3-linux-i686.tar.gz)
       - [linux(x64)](https://dl.espressif.com/dl/xtensa-lx106-elf-gcc8_4_0-esp-2020r3-linux-amd64.tar.gz)
   - 将下载好的工具链解压到```~/Libraries/ESP8266/toolchain/```中
     > tar -zxf xtensa-lx106-elf-XXXX.tar.gz ~/Libraries/ESP8266/toolchain/xtensa-lx106-elf
   - 添加工具链路径到```PATH```中
     > echo "export PATH=\"\$PATH:\$HOME/Libraries/ESP8266/toolchain/xtensa-lx106-elf/bin\"" >> ~/.bashrc
3. 克隆ESP8266_RTOS_SDK开发包
   - 创建开发工具包的存放目录```~/Libraries/ESP8266/sdk/```
     > mkdir -p ~/Libraries/ESP8266/sdk/
   - 克隆SDK
     > git clone https://github.com/espressif/ESP8266_RTOS_SDK ~/Libraries/ESP8266/sdk/ESP8266_RTOS_SDK
     > cd ESP8266_RTOS_SDK & git checkout -b release/v3.4 origin/release/v3.4
   - 添加SDK路径导出到```IDF_PATH```
     > echo "export PATH=\"\$PATH:\$HOME/Libraries/ESP8266/sdk/ESP8266_RTOS_SDK\"" >> ~/.bashrc
4. 刷新控制台环境
   > bash
5. 安装编译工具依赖项
   > python -m pip install --user -r $IDF_PATH/requirements.txt
6. 测试编译
   - 进入示例工程
     > cd $IDF_PATH/examples/get-started/hello_world
   - 启动工程配置
     > make menuconfig
     - 此时能看到以下界面
       ![make menuconfig](/imgs/esp8266_idf_sdk_envirnment_setup__menu_config_snapshot.png)
     - ```Exit```退出后时选择```Yes```可以看到如下log说明配置成功
       ```
       MENUCONFIG
       configuration written to /home/cocoonshu/Subversions/Espressif/esp8266/sdk/ESP8266_RTOS_SDK/examples/get-started/hello_world/sdkconfig
       ```
   - 编译并生成镜像
     > make flash
     - 编译完成后能看到如下log，以说明镜像```elf```文件已生成
       ```
       Generating esp8266.project.ld
       LD build/hello-world.elf
       esptool.py v2.4.0
       ```
     - 同时能看到以下log报出没有发现串口而无法烧写镜像到ESP8266
       ```
       Flashing binaries to serial port /dev/ttyUSB0 (app at offset 0x10000)...
       esptool.py v2.4.0
       Traceback (most recent call last):
           ......
           raise SerialException(msg.errno, "could not open port {}: {}".format(self._port, msg))
       serial.serialutil.SerialException: [Errno 2] could not open port /dev/ttyUSB0: [Errno 2] No such file or directory: '/dev/ttyUSB0'
       ```
     - 至此环境配置成功

# 参考
[ESP-IDF Programming Guide](https://esp-idf-zh.readthedocs.io/zh_CN/latest/get-started/linux-setup.html)
[ESP8266_RTOS_SDK](https://github.com/espressif/ESP8266_RTOS_SDK)
