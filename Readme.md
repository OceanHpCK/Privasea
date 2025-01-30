### CẤU HÌNH
- Component	Requirement
- Operating System (OS)	Ubuntu (Recommended)
- CPU Architecture	amd64 (x86 architecture)
- Storage	100GB available storage
- Memory (RAM)	4GB RAM
- Processor	6 cores
  
### Thông tin
- Faucet Arbitrum ETH Sepolia
https://faucet.quicknode.com/arbitrum/sepolia
  
- Faucet Privasea DeepSea Beta tokens
  https://deepsea-beta.privasea.ai/deepSeaFaucet

### Hướng dẫn chạy:
Mở terminal trên Ubuntu và nhập các lệnh sau:

ps: Đối với sử dụng win thì mở Ubuntu lên và chạy lệnh này trước:
```
Sudo su
```
1. Update

```
sudo apt-get update && sudo apt-get upgrade -y
```

```
sudo apt -qy install curl git jq lz4 build-essential
```

2. Cài Docker

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

```
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

```
sudo apt-get update
```

```
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

```
sudo docker --version
```

```
sudo systemctl start docker
```

```
sudo systemctl enable docker
```


3. Pull docker mirroring
```
docker pull privasea/acceleration-node-beta:latest
```

4. Tạo thư mục privasea

```
mkdir -p ~/privasea/config && cd ~/privasea
```

5. Lấy keystore
```
docker run --rm -it -v "$HOME/privasea/config:/app/config" privasea/acceleration-node-beta:latest ./node-calc new_keystore
```
- Đặt mật khẩu
- Sau khi đặt mật khẩu xong sẽ hiện ra dòng:
 ### node address: <Địa chỉ node> ---Bạn hãy lưu lại địa chỉ này để tý nữa sử dụng tại bước 7

6. Chuyển keystore vào thư mục privasea
```
mv $HOME/privasea/config/UTC--* $HOME/privasea/config/wallet_keystore
```

7.
- Truy cập vào [Privanetix Dashboard](https://deepsea-beta.privasea.ai/privanetixNode)
- Kết nối ví chính và thiết lập node
- Đặt tên Node tại mụa Edit Node Name
- Edit your node address... : điền địa chỉ ví lấy được ở mục số 5 bên trên
=> Sau đó ấn confirm .. => ký ví và trả gas bằng ETH-arb Speolia
(bạn nên tạo 1 ví mới => chuyển gas ETH-arb Sepolia sang và sử dụng ví okx, các ví khác mình xài đều bị lỗi)

8. Run node
```
KEYSTORE_PASSWORD=<Mật Khẩu> && docker run -d --name privanetix-node -v "$HOME/privasea/config:/app/config" -e KEYSTORE_PASSWORD=$KEYSTORE_PASSWORD privasea/acceleration-node-beta:latest
```
Note: Nhớ thay <Mật Khẩu> bằng mật khẩu đã đặt cho keystore phía trên

9. Check log
```
docker logs -f <Container ID>
```
Note: Thay <Container ID> bằng ID của bạn
Có thể check bằng lệnh: 
```
docker ps -a
```

10. Dừng node
```
docker ps -q --filter "ancestor=privasea/acceleration-node-beta:latest" | xargs --no-run-if-empty docker stop
```

```
docker ps | grep privasea/acceleration-node-beta:latest
```
