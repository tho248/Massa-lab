# Nếu có chạy massa cũ thì xóa trước khi chạy .12.0 ( chạy mới bỏ qua )
```
 systemctl stop massa-node && systemctl disable massa-node && rm -rf $HOME/massa/
```
# Tải file
```
sudo wget https://github.com/massalabs/massa/releases/download/TEST.12.0/massa_TEST.12.0_release_linux.tar.gz
apt install gzip
gunzip massa_TEST.12.0_release_linux.tar.gz
tar -xvf massa_TEST.12.0_release_linux.tar
```
# Cài đặt định tuyến 
các kết nối đến trên các cổng TCP 31244 và 31245 phải được hướng tới địa chỉ IP cục bộ của máy tính chạy nút
chỉnh sửa tệp massa-node/config/config.toml(tạo tệp nếu không có) với các nội dung sau:
```
cd $HOME/massa/massa-node/config
vi config.toml
```
```
[network]
routable_ip = "AAA.BBB.CCC.DDD"
```
trong đó AAA.BBB.CCC.DDD sẽ được thay thế bằng địa chỉ IP công cộng của bạn

. kiểm tra xem các cổng của mình có đang mở hay không bằng cách nhập địa chỉ IP công cộng và cổng 31244 vào https://www.yougetsignal.com/tools/open-ports/ ( sau đó một lần nữa với cổng 31245)
. Khi nút của bạn có thể định tuyến được, bạn cần gửi địa chỉ IP công khai của nút tới bot Discord sau khi đk discord ở bước cuối
 
# tạo systemd : copy pase all bellow --> enter
```
sudo tee /etc/systemd/system/massa-node.service > /dev/null <<EOF
[Unit]
Description=Massa Node

[Service]
User=root
ExecStart=/bin/bash -c 'cd /root/massa/massa-node && ./massa-node -p <Your-PASSWORD>'
Restart=on-failure
RestartSec=10
LimitNOFILE=10000

[Install]
WantedBy=multi-user.target
EOF
```
# RUN
```
sudo systemctl enable massa-node
sudo systemctl daemon-reload
sudo systemctl restart massa-node.service && journalctl -u massa-node.service -f -o cat
```
# TẠO VÍ 
```
cd $HOME/massa/massa-client && ./massa-client
wallet_generate_secret_key 
wallet_info
```
# discord ---> faucet

# buy roll
 ```              
buy_rolls <Wallet Address> 1 0
```
# Register your secret key 
```
node_add_staking_secret_keys <your_secret_key>
```
# liên kết discord

vào kênh #testnet reward registi..chát đại câu gì đó, sau đó bot nó gởi tin nhắn 

chạy lệnh :$node_testnet_rewards_program_ownership_proof Address discord_id

  example : node_testnet_rewards_program_ownership_proof A1aE4HBDRPW44WvxzTtGCQeyBa1Ts3yKUFKD3DRMt31QykUJ2Cy 849656887363371029
  
  copy đầu ra gửi cho nó 
  
  ---ok ok ok---
