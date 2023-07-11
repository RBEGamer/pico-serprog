# pico-serprog

This is a very basic flashrom/serprog compatible SPI flash reader/writer for the Raspberry Pi Pico.

It does not require a custom version of flashrom, just drag the compiled uf2 onto your Pico and you're ready to go.

The default pin-out is:

| GPIO | Pico Pin | Function |
|------|----------|----------|
| 1    |    2     | CS       |
| 2    |    4     | SCK      |
| 3    |    5     | MOSI     |
| 4    |    6     | MISO     |
| 5    |    7     | CRESET   |

## Build
```bash
$ git clone https://github.com/RBEGamer/pico-serprog.git ./pico-serprog
$ cd ./pico-serprog && cmake . && make
```

## Usage

Dump a flashchip:

```bash
$ export FLASH_SIZE=16 #MByte
$ export DATA_TO_FLASH=testbitmap.bin # testimage.bin
# CREATE EMPTY IMAGE WITH SIZE OF THE FLASH CHIP
$ perl -e 'print "ff "x('${FLASH_SIZE}'*1024*1024)'| xxd -r -p > ${FLASH_SIZE}_data.bin
# ADD CONTENT TO IMAGE
$ dd if=$DATA_TO_FLASH of=${FLASH_SIZE}_data.bin conv=notrunc

# FINALLY FLASH IMAGE
$ flashrom -p serprog:dev=/dev/ttyACM0:115200,spispeed=12M -E -r ${FLASH_SIZE}_data.bin -V
```

## License

The project is based on the spi_flash example by Raspberry Pi (Trading) Ltd. which is licensed under BSD-3-Clause.

As a lot of the code itself was heavily inspired/influenced by `stm32-vserprog` this code is licensed under GPLv3.
