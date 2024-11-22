
**用腳本跑多數情況都ok, 但是為了保險起見可以分段跑,#分割區塊中間整坨貼上就可以了,不需要一行一行**

步驟 1：創建腳本文件
打開終端。
使用文本編輯器創建腳本文件，例如 setup.sh：
`nano setup.sh`
將腳本內容貼入編輯器，然後保存並退出：
在 nano 中，按 Ctrl + O 保存，按 Enter 確認，然後按 Ctrl + X 退出。

步驟 2：賦予腳本執行權限
在終端中執行以下命令，為腳本賦予執行權限：

`chmod +x setup.sh`

步驟 3：執行腳本
如果腳本位於當前目錄下，使用以下命令執行：

`sudo ./setup.sh`

如果腳本位於其他目錄，例如 /home/user/scripts，需要提供完整路徑：

`sudo /home/user/scripts/setup.sh`

步驟 4：確認安裝進度
腳本執行過程中會打印彩色提示訊息

裝 miniconda 時有一個預設是 no 要改成 yes 
好像是 auto init 的那個，其他用預設值

```
#!/bin/bash

################################################
#               Color Definitions              #
################################################
YELLOW="\033[0;33m"
RED='\033[0;31m'
GREEN='\033[1;32m'
CYAN='\033[1;36m'
NC='\033[0m'
UNDERLINE_YELLOW="\033[4;33m"

################################################
#                 Installation                 #
################################################
echo -e "安裝 ${GREEN}[基本套件]${NC}"
sudo apt-get update
sudo apt-get install build-essential ca-certificates curl gnupg -y

################################################
#               禁用 Wayland                   #
################################################
echo -e "配置 ${GREEN}[禁用 Wayland]${NC}"
sudo sed -i 's/^#.*WaylandEnable=.*/WaylandEnable=false/' /etc/gdm3/custom.conf
echo -e "${CYAN}Wayland 已禁用，請確認系統重啟後生效。${NC}"

################################################
#               安裝 Docker                    #
################################################
echo -e "安裝 ${GREEN}[Docker]${NC}"
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
sudo usermod -aG docker $USER

################################################
#               安裝 DBeaver                   #
################################################
echo -e "安裝 ${GREEN}[DBeaver]${NC}"
cd /tmp && curl -L https://dbeaver.io/files/dbeaver-ce_latest_amd64.deb -o dbeaver.deb && sudo dpkg -i dbeaver.deb

################################################
#               安裝 PyCharm                   #
################################################
echo -e "安裝 ${GREEN}[PyCharm]${NC}"
sudo snap install pycharm-community --classic
echo -e "-Xmx2048m\n-Drecreate.x11.input.method=true" > ~/.config/JetBrains/PyCharm20*/pycharm64.vmoptions

################################################
#               安裝 Chrome                    #
################################################
echo -e "安裝 ${GREEN}[Chrome]${NC}"
curl -L https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb -o chrome.deb && sudo dpkg -i chrome.deb

################################################
#               安裝 Git                       #
################################################
echo -e "安裝 ${GREEN}[Git]${NC}"
sudo add-apt-repository ppa:git-core/ppa
sudo apt update
sudo apt install git -y

################################################
#               安裝 注音輸入法               #
################################################
echo -e "安裝 ${GREEN}[注音輸入法]${NC}"
sudo apt install ibus-chewing -y
echo -e "${UNDERLINE_YELLOW}請手動重新開機後自行設定中文輸入法${NC}"

################################################
#               安裝 MiniConda                 #
################################################
echo -e "安裝 ${GREEN}[MiniConda]${NC}"
cd /tmp && curl -L https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -o miniconda.sh && chmod +x miniconda.sh && ./miniconda.sh

################################################
#               安裝 Poetry                    #
################################################
echo -e "安裝 ${GREEN}[Poetry]${NC}"
curl -sSL https://install.python-poetry.org | python3 -
echo "export PATH=\"$(echo $HOME)/.local/bin:\$PATH\"" >> ~/.bashrc

################################################
#               安裝 RealVNC Server            #
################################################
echo -e "安裝 ${GREEN}[RealVNC Server]${NC}"
VNC_URL="https://downloads.realvnc.com/download/file/vnc.files/VNC-Server-6.11.0-Linux-x64.deb"
VNC_DEB="/tmp/VNC-Server-6.11.0-Linux-x64.deb"

# 下載並安裝 RealVNC Server
curl -L $VNC_URL -o $VNC_DEB
if sudo dpkg -i $VNC_DEB; then
    echo -e "${GREEN}RealVNC Server 安裝完成！${NC}"
else
    echo -e "${RED}安裝失敗！嘗試修復依賴...${NC}"
    sudo apt-get install -f -y
    sudo dpkg -i $VNC_DEB
fi

# 啟用並啟動 VNC Server
sudo systemctl enable vncserver-virtuald.service
sudo systemctl start vncserver-virtuald.service

echo -e "${CYAN}RealVNC Server 已安裝並啟動！請檢查服務狀態：sudo systemctl status vncserver-virtuald.service${NC}"

################################################
#               安裝 VNC Viewer               #
################################################
echo -e "安裝 ${GREEN}[VNC Viewer]${NC}"
VNC_VIEWER_URL="https://downloads.realvnc.com/download/file/viewer.files/VNC-Viewer-6.21.406-Linux-x64.deb"
VNC_VIEWER_DEB="/tmp/VNC-Viewer-6.21.406-Linux-x64.deb"

curl -L $VNC_VIEWER_URL -o $VNC_VIEWER_DEB
if sudo dpkg -i $VNC_VIEWER_DEB; then
    echo -e "${GREEN}VNC Viewer 安裝完成！${NC}"
else
    echo -e "${RED}安裝失敗！嘗試修復依賴...${NC}"
    sudo apt-get install -f -y
    sudo dpkg -i $VNC_VIEWER_DEB
fi

echo -e "${CYAN}VNC Viewer 安裝完成！可通過 vncviewer 命令啟動。${NC}"

################################################
#               安裝 OpenSSH Server            #
################################################
echo -e "安裝 ${GREEN}[OpenSSH Server]${NC}"
sudo apt-get install openssh-server -y
sudo vnclicense -add 8KR6M-MXGEU-ETGJQ-QMKKW-HV4KA

echo -e "${CYAN}OpenSSH Server 已安裝！${NC}"



```



如果poetry 連接時間過長,代表有空院內網路把這個網站鎖住了
這樣要先在有安裝好poetry的電腦 先起一個sever
```
mkdir install
cd install/
ls
vim poetry
python -m http.server 8000

```
sever起好以後, 安裝指令就要變成 指定該台電腦ip/port 到那台電腦去拿poetry

```
 curl -sSL http://10.5.71.143:8000/poetry | python

```