# NVIDIA-Thor-System-Install
Nvidia AGX Thor安裝log

I. 硬體準備:
  1. NVIDIA AGX Thor本體
  2. 官方提供的變壓器(type-C頭)
  3. HDMI傳輸線(以方便在螢幕上操作)
  4. 鍵盤
  5. 空間大於16GB的隨身碟(系統碟)
  6. 一臺電腦(用來灌系統碟)

II. 軟體準備:
  1. Jetson 官方鏡像檔:https://developer.nvidia.com/embedded/jetpack/downloads
  2. Etcher系統燒錄套件:https://etcher.balena.io/#download-etcher

III. 安裝流程:
  1. 先在隨身碟上用Etcher燒錄官方鏡像檔，燒錄好之後插到Jetson的USB端口，並接上HDMI線、鍵盤，供電可以直接插Jetson的兩個type-C插口其中一個
  2. 開啟電源後，理論上會進入GNU GRUB的畫面，此時點選第一個"Jetson Thor options"
     <img width="1338" height="832" alt="Jetson 1" src="https://github.com/user-attachments/assets/5bb261a0-71f8-44a8-914b-5f14f15d73d8" />(圖片來源:CSDN)
     如果沒有成功，而是進去以下畫面:
     ![Jetson2](https://github.com/user-attachments/assets/d136985f-4f22-4dfb-a6b7-786732ab7c3d)
     則先輸入exit，而後進入Boot Manager，把USB拉到第一啟動位，然後Continue
     <img width="1590" height="1040" alt="Jetson 3" src="https://github.com/user-attachments/assets/9fde0136-8c50-40ad-8332-51ef496932c1" />(圖片來源:CSDN)
     <img width="1120" height="768" alt="Jetson 4" src="https://github.com/user-attachments/assets/30c9d426-1df6-4105-97b5-f716ea5cf8ab" />(圖片來源:CSDN)
  3. 進入之後，點選第一個"Flash Jetson AGX Thor Developer Kit on NVMe 0.2.0-r38.2"，這代表將系統燒錄到硬體裡(第二個選項就是將系統燒到USB內)
     <img width="1440" height="1080" alt="Jetson 5" src="https://github.com/user-attachments/assets/0843ff56-f922-45b1-833c-142565de754e" />(圖片來源:CSDN)
  4. 點選之後，等待15分鐘直至安裝成功，此時它會自動重啟，等待Update Progress跑完即可。
  5. 安裝成功後，即可開始設定使用者名稱、時區、密碼等等，此時已經可以把USB拔下來了。

IV. Jetpack&CUDA安裝:
  1. 打開終端機，直接使用以下指令:
     ```bash
     sudo apt update
     sudo apt install nvidia-jetpack
     ```
     此時它會安裝Jetpack的完整包，大概會耗掉15G的空間
     下面是關於Jetpack的架構圖:
     <img width="1488" height="829" alt="jetpack-metapackage" src="https://github.com/user-attachments/assets/f490f848-af36-42fd-a326-0b64973392b8" />(圖片來源:NVIDIA)
     基本上在安裝這個的同時也會安裝好CUDA
  2. 安裝好nvidia-jetpack後，要記得去設定CUDA的環境變量
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
    


參考來源:
NVIDIA Jetpack:https://docs.nvidia.com/jetson/agx-thor-devkit/user-guide/latest/setup_cuda.html#option-2-jetpack-apt-repo
CSDN Jetson Thor刷機與安裝教學:https://blog.csdn.net/nenchoumi3119/article/details/151148194
知乎-設定CUDA的環境變量:https://zhuanlan.zhihu.com/p/1971245103956878390
     
