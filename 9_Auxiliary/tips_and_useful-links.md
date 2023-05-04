## Troubleshooting

- USB-Serial baud rate: 115200

- DON'T CHANGE EXAMPLE NOTEBOOKS, MAKE A COPY!

- In some cases, `sudo reboot now` can help.

## Create a backup image of SD card without free-space, so it can be flash on to either 16GB or 32GB SD Card.
Insert your SD card to your PC, assume it is mounted as /dev/sdb, run:
```
sudo dd if=/dev/sdb | gzip > pynq_zu_v2.7.0_ros.img.gz
```

## Docker on PYNQ
https://discuss.pynq.io/t/docker-xilinx-platforms-pynq/1962

## Install Pytorch on PYNQ
https://discuss.pynq.io/t/install-pytorch-on-pynq/5499

# Microsoft's VS Code for C/C++/Python Development on Xilinx Platforms
https://discuss.pynq.io/t/microsofts-vs-code-for-c-c-python-development-on-xilinx-platforms/2031