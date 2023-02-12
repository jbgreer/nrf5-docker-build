# nrf5-docker-build
Docker environment for building nRF5 firmware.

## Installed Tools

### [nRF5 SDK v17.1.0](https://infocenter.nordicsemi.com/index.jsp?topic=%2Fstruct_sdk%2Fstruct%2Fsdk_nrf5_latest.html)
#### Installed at `/nrf5/nRF5_SDK_17.1.0` 
### [Micro-ecc v1.0](https://github.com/kmackay/micro-ecc) under nRF5 SDK
### [GCC ARM 9-2020-q2-update](https://developer.arm.com/-/media/Files/downloads/gnu-rm/9-2020q2/gcc-arm-none-eabi-9-2020-q2-update-x86_64-linux.tar.bz2)
### nrfutil ([iamfat's arm64-compatible fork](https://github.com/iamfat/pc-nrfutil))
### srecord (via package manager)

## Sample Usage (quick test)

### Build Docker Image
#### On host
```
] docker build -t jbgreer/nrf5-v17.1.0 .
```

### Run 
#### Drops you into a shell in the container
```
] docker run --name nrf5-v17.1.0 -it jbgreer/nrf5-v17.1.0 /bin/bash
```

### Use container
#### In container, change to an existing example and build it.
```
$ cd /nrf5/nRF5_SDK_17.1.0/examples/ble_peripheral/ble_app_template/pca10056e/s112/armgcc
$ make
```

### Copy output files to Host
#### On host, copy files from container to local directory
```
] docker cp nrf5-v17.1.0:/nrf5/nRF5_SDK_17.1.0/examples/ble_peripheral/ble_app_template/pca10056e/s112/armgcc/_build/nrf52811_xxaa.hex .
] docker cp nrf5-v17.1.0:/nrf5/nRF5_SDK_17.1.0/components/softdevice/s112/hex/s112_nrf52_7.2.0_softdevice.hex .
```

### Flash output files to nRF5 development board
```
] nrfjprog -f NRF52 --eraseall
] nrfjprog -f NRF52 --program s112_nrf52-7.2.0_softdevice.hex --chiperase --verify
] nrfjprog -f NRF52 --program nrf52811_xxaa.hex --verify
] nrfjprog -f NRF52 --reset
```

