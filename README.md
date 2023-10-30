# AvailTestnet
Guide for installing Avail Node on Ubuntu 20.04 &amp; 22.04

## System Requirements (Recommended)

CPU: 4 core
RAM: 8 GB
Storage(SSD): 200-300 GB

## Useful Links

Form: https://docs.google.com/forms/d/e/1FAIpQLScvgXjSUmwPpUxf1s-MR2C2o5V79TSoud1dLPKVgeLiLFuyGQ/viewform
Telemetry: https://telemetry.avail.tools/

## 1. Install Rust


`sudo apt update && sudo apt upgrade -y`

`sudo apt install build-essential`

``sudo apt install --assume-yes git clang curl libssl-dev protobuf-compiler``

``curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh``

``source ~/.cargo/env``

``rustup default stable``

``rustup update``

``rustup update nightly``

``rustup target add wasm32-unknown-unknown --toolchain nightly``

## 2. Install Avail and Run

``git clone https://github.com/availproject/avail.git``

``screen -S availnode``

``cd avail``

``cargo build --release -p data-avail``

``mkdir -p output``

``git checkout v1.7.2``

``cargo run --locked --release -- --chain kate -d ./output``

***Now your node is running in screen press "Ctrl+a+d" while running your node and left the screen.***
***Use ``screen -r`` to return back to screen***

## 3. Create Systemd

``touch /etc/systemd/system/availd.service``

``nano /etc/systemd/system/availd.service``

***Moniker is your validator name***

``[Unit]``

``Description=Avail Validator``

``After=network.target``

``StartLimitIntervalSec=0``

``[Service]`` 

``User=root``

``ExecStart= /root/avail/target/release/data-avail --base-path `pwd`/data --chain kate --name "Moniker"``

``Restart=always``

``RestartSec=120``

``[Install]`` 

``WantedBy=multi-user.target``


Save it: CTRL+X Yes Enter.


To enable this to autostart on bootup run:

``systemctl enable availd.service``

Start it manually with:

``systemctl start availd.service``

You can check that it's working with:

``systemctl status availd.service``

You can see the logs with:

``journalctl -f -u availd``



