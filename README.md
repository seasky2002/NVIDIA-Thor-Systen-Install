![108994](https://github.com/user-attachments/assets/cb096804-59d6-4e0c-b205-6ffb680312e1)# NVIDIA-Thor-System-Install
Nvidia AGX Thor安裝log

**硬體規格:
Jetson AGX Thor Developer Kit,
Jetson T5000**

**軟體規格:
JetPack 7.0 with Jetson Linux 38.2(Ubuntu 24.04 LTS　＆　Kernel v6.8 LTS)**

## I. 硬體準備:
  1. **NVIDIA AGX Thor本體**
  2. **官方提供的變壓器(type-C頭)**
  3. **HDMI傳輸線(以方便在螢幕上操作)**
  4. **外接鍵盤**
  5. **空間大於16GB的隨身碟(系統碟)**
  6. **一臺電腦(用來灌系統碟)**

## II. 軟體準備:
  1. Jetson 官方鏡像檔:[JetPack Downloads](https://developer.nvidia.com/embedded/jetpack/downloads)
  2. Etcher系統燒錄套件:[Etcher](https://etcher.balena.io/#download-etcher)

## III. 安裝流程:
  1. 先在隨身碟上用Etcher燒錄官方鏡像檔，燒錄好之後插到Jetson的USB端口，並接上HDMI線、鍵盤，供電可以直接插Jetson的兩個type-C插口其中一個
  2. 開啟電源後，理論上會進入GNU GRUB的畫面，此時點選第一個"Jetson Thor options"
     <img width="640" height="480" alt="Jetson 1" src="https://github.com/user-attachments/assets/5bb261a0-71f8-44a8-914b-5f14f15d73d8" />(圖片來源:CSDN)
     
     如果沒有成功，而是進去以下畫面:
     ![Jetson2](https://github.com/user-attachments/assets/d136985f-4f22-4dfb-a6b7-786732ab7c3d)
     
     則先輸入exit，而後進入Boot Manager，把USB拉到第一啟動位，然後Continue
     <img width="1590" height="1040" alt="Jetson 3" src="https://github.com/user-attachments/assets/9fde0136-8c50-40ad-8332-51ef496932c1" />(圖片來源:CSDN)
     
     <img width="1120" height="768" alt="Jetson 4" src="https://github.com/user-attachments/assets/30c9d426-1df6-4105-97b5-f716ea5cf8ab" />(圖片來源:CSDN)
     
  4. 進入之後，點選第一個"Flash Jetson AGX Thor Developer Kit on NVMe 0.2.0-r38.2"，這代表將系統燒錄到硬體裡(第二個選項就是將系統燒到USB內)
     <img width="1440" height="1080" alt="Jetson 5" src="https://github.com/user-attachments/assets/0843ff56-f922-45b1-833c-142565de754e" />(圖片來源:CSDN)
     
  6. 點選之後，等待15分鐘直至安裝成功，此時它會自動重啟，等待Update Progress跑完即可。
  7. 安裝成功後，即可開始設定使用者名稱、時區、密碼等等，此時已經可以把USB拔下來了。

## IV. Jetpack&CUDA安裝:
  1. 打開終端機，直接使用以下指令:
     ```bash
     sudo apt update
     sudo apt install nvidia-jetpack
     ```
     此時它會安裝Jetpack的完整包，大概會耗掉15G的空間
     下面是關於Jetpack的架構圖:
     <img width="1488" height="829" alt="jetpack-metapackage" src="https://github.com/user-attachments/assets/f490f848-af36-42fd-a326-0b64973392b8" />(圖片來源:NVIDIA)
     
     基本上在安裝這個的同時也會安裝好CUDA
  3. 安裝好nvidia-jetpack後，要記得去設定CUDA的環境變量
     首先先執行以下程式:
     ```bash
     ll /usr/local/cuda-13.0
     ```
     確保CUDA有在裝置裡面，當確認以後，
     ```bash
     sudo nano ~/.bashrc
     ```
     在bashrc的最底下加上這兩行:
     ```bash
     export PATH=/usr/local/cuda-13.0/bin:$PATH
     export LD_LIBRARY_PATH=/usr/local/cuda-13.0/lib64:$LD_LIBRARY_PATH
     ```
     儲存後退出，最後
     ```bash
     source ~/.bashrc
     ```
     此時就可以去驗證cuda有沒有安裝成功了
     
     <img width="671" height="153" alt="螢幕擷取畫面 2025-11-19 141310" src="https://github.com/user-attachments/assets/5c990015-4ebe-42ec-8fcb-7bd6cfba11ee" />

## V. jetson-stats安裝:
  1. jetson-stats這個包可以監控硬體的使用狀況，以及各個系統的連接狀況。執行以下指令安裝:
     ```bash
     sudo apt update
     sudo apt install python3
     sudo apt install python3-pip
     sudo pip3 install -U pip
     sudo pip3 install jetson-stats
     ```
     
     中間可能會有以下報錯:
     ```bash
     error: externally-managed-environment
     x This environment is externally managed
     ╰─> To install Python packages system-wide, try apt install
     python3-xyz, where xyz is the package you are trying to
     install.
     If you wish to install a non-Debian-packaged Python package,
     create a virtual environment using python3 -m venv path/to/venv.
     Then use path/to/venv/bin/python and path/to/venv/bin/pip. Make
     sure you have python3-full installed.
     If you wish to install a non-Debian packaged Python application,
     it may be easiest to use pipx install xyz, which will manage a
     virtual environment for you. Make sure you have pipx installed.
     See /usr/share/doc/python3.12/README.venv for more information.
     ```
     此時執行以下指令即可解決:
     ```bash
     sudo mv /usr/lib/python3.12/EXTERNALLY-MANAGED /usr/lib/python3.12/EXTERNALLY-MANAGED-back
     sudo pip3 install -U pip
     sudo pip3 install jetson-stats
     ```
  3. 安裝完成後，啟用並重啟:
     ```bash
     sudo systemctl restart jtop.service
     sudo reboot now
     ```
     重啟後即可檢查有沒有輸出
     ```bash
     jtop
     ```
     <img width="2139" height="1157" alt="image" src="https://github.com/user-attachments/assets/154bf7ac-25ca-42a2-9865-41cf084e1ceb" />

     查看CUDA與openCV版本
     ```bash
     jetson_release
     ```
     <img width="780" height="557" alt="image" src="https://github.com/user-attachments/assets/b657e9a0-49de-4fea-9687-62f35db7b09e" />

     查看Nvidia Driver版本
     ```bash
     nvidia-smi
     ```
     <img width="1097" height="530" alt="image" src="https://github.com/user-attachments/assets/6404bbd8-3ad9-44c6-9a93-55e0c784f579" />
## VI. ROS2 安裝:
  使用小魚的一鍵安裝ROS功能:
   ```bash
     wget http://fishros.com/install -O fishros && . fishros
   ```
  而後照著上面進行操作，目前Ubuntu24.04支援以下三種版本:
    1. Jazzy
    2. Kilted
    3. Rolling
  安裝完成後，可以開新的終端驗證:
  
  <img width="461" height="79" alt="image" src="https://github.com/user-attachments/assets/ad295a88-dac1-4324-912f-e5e5241057e0" />

## VII. Realsense SDK安裝
  在安裝好ROS2後，執行下面的指令，將librealsense下載下來:
  ```bash
     git clone https://gitcode.com/GitHub_Trending/li/librealsense
     cd librealsense
   ```
  而後對內容進行建置:
  ```bash
     mkdir build && cd build
     cmake .. -DBUILD_WITH_ROS2=ON \
             -DCMAKE_INSTALL_PREFIX=/opt/ros/jazzy
     make -j4
     sudo make install
   ```
  建置完成後，即可用以下指令驗證:
  ```bash
     realsense-viewer
   ```

  <img width="2151" height="1165" alt="螢幕擷取畫面 2025-11-24 103644" src="https://github.com/user-attachments/assets/8872114f-7133-4c41-b83f-8354beca3982" />

  ROS依賴則是用以下的指令下載:
  ```bash
     sudo apt install ros-jazzy-realsense2-* #這裡用jazzy是因為我的ROS2是載這個版本，目前官方SDK支援jazzy和kilted
   ```
  然後，安裝[realsense-ros包](https://github.com/realsenseai/realsense-ros?tab=readme-ov-file#installation-on-ubuntu)後，建置即可運行
  
  <img width="2016" height="812" alt="image" src="https://github.com/user-attachments/assets/d63dc10a-433d-4bae-a8f1-d1dbcdebd4be" />


## VII. VLP-16(Velodyne Lidar Driver)安裝:
  在終端機先運行以下指令，安裝velodyne的依賴:
  ```bash
     sudo apt-get install ros-jazzy-velodyne #這裡用jazzy是因為我的ROS2是載這個版本，目前官方支援jazzy和rolling
  ```
  然後，在[官方Github](https://github.com/ros-drivers/velodyne/tree/ros2)下載對應版本的分支，然後將內容都放到ROS工作空間的src
  接下來進行編譯:
  ```bash
     colcon build
  ```
  確認Velodyne的網路線和Jetson接在一起之後，前往Setting-->Network-->找到連接的以太網路
  
  ![108993](https://github.com/user-attachments/assets/50f1f294-8a5c-4983-a5f8-2dd04b0a1720)

  點選右邊的齒輪，進入IPv4設定位置。
  VLP-16 需要的ip是 192.168.1.X的格式，因此將其改成以下的樣子:

  ![108994](https://github.com/user-attachments/assets/73f8e079-82dd-4f13-b0c2-dbd6981b6687)

  修改好後，在瀏覽器輸入192.168.1.201，檢查有沒有進去Velodyne的參數設定頁面
  
  ![108995](https://github.com/user-attachments/assets/58c959e4-a80e-4665-a069-e3ae0830074b)

  如果有，此時就能在ROS上運行了。
  ```bash
    ros2 launch velodyne velodyne-all-nodes-VLP16-launch.py
  ```
  打開Rviz查看話題是否有正確發布:
  ```bash
    rviz2 rviz
  ```
  
  <img width="2142" height="1160" alt="image" src="https://github.com/user-attachments/assets/9376f13e-9d95-45b1-9fe5-acb673597cbf" />

## VIII. 已知問題:
  1. 在nvidia-smi會顯示Memory Usage: Not Supported，這只是因為它讀不到記憶體狀態，實際在運行時，GPU還是會分配記憶體(經過pytorch測試)
  2. jetson_release會顯示抓不到jetpack，但實際已經在裡面了
  3. RealSense SDK沒辦法讀取到d435i的相機，目前推測是Jetpack 6以上的版本把IMU的依賴給移除了[連結](https://github.com/realsenseai/librealsense/issues/14169)

  ![messageImage_1763970783422](https://github.com/user-attachments/assets/6d7cbced-51c0-49a0-8586-e2c238337556)


## 參考來源:

NVIDIA Jetpack官方:[連結](https://docs.nvidia.com/jetson/agx-thor-devkit/user-guide/latest/setup_cuda.html#option-2-jetpack-apt-repo)

CSDN-Jetson Thor刷機與安裝教學:[連結](https://blog.csdn.net/nenchoumi3119/article/details/151148194)

知乎-設定CUDA的環境變量:[連結](https://zhuanlan.zhihu.com/p/1971245103956878390)

魚香ROS-一鍵安裝ROS2:[連結](https://fishros.org.cn/forum/topic/20/%E5%B0%8F%E9%B1%BC%E7%9A%84%E4%B8%80%E9%94%AE%E5%AE%89%E8%A3%85%E7%B3%BB%E5%88%97)

CSDN-ROS2 RealSense SDK安裝: [連結](https://blog.csdn.net/gitblog_00540/article/details/151542427)

Velodyne官方ROS: [連結](https://wiki.ros.org/velodyne/Tutorials/Getting%20Started%20with%20the%20HDL-32E) 

CSDN-ROS2 VLP-16安裝: [連結](https://blog.csdn.net/hltt3838/article/details/109147598)
     
