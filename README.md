#BoxDemo
## 如何执行？
1. cd /opt/bin/BoxDemo
2. vi box.conf 配置日志级别以及检测开关等。[参数说明](#配置参数)
3. 执行./run.sh

## 如何编译？
1. export PATH=/opt/gcc-arm-9.2-2019.12-x86_64-aarch64-none-linux-gnu/bin:$PATH
2. mkdir build & cd build
3. cmake -DSDK_PATH=${SDK_DIR} -DCMAKE_BUILD_TYPE=Release -DCMAKE_TOOLCHAIN_FILE=../toolchains/aarch64-none-linux-gnu.toolchain.cmake ..
4. make -j6
# <a href="#配置参数">配置参数</a>

|   #   |             参数       |   参数范围   |                          说明           |
| ----- | ----------------------| ------------ | --------------------------------------- |
|   1   | [DETECT]enable        |              | 0：不检测 1:检测                         |
|   2   | [DISPC]dev            | [0 - 1]      | 0: HDMI-0 1:HDMI-1                      |
|   3   | [STREAM]count         | [1 - 32]     | 码流数量，目前建议不超过32                |
