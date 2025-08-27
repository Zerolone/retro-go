ILI9341_CMD(0x36, 0x68); //旋转90度

ILI9341_CMD(0x36, 0xA8); //旋转270度

------------
配置部分

sudo apt update

sudo apt-get install libusb-1.0-0-dev

mkdir -p ~/esp

cd ~/esp

git clone -b release/v4.4 --recursive https://github.com/espressif/esp-idf.git


cd ~/esp/esp-idf

./install.sh

./install.sh esp32s3

 . ./export.sh

cd /workspaces/retro-go/

//
python rg_tool.py build-img --target=esp32s3-devkit-c

//重新编译
python rg_tool.py release  --target=esp32s3-devkit-c




cd ~/esp/esp-idf

 . ./export.sh

cd /workspaces/retro-go/

//新的

python rg_tool.py build-img --target=esp32s3-my


esptool --port COM15 -b 1152000 write_flash --flash_size detect 0x0 retro-go_619e7_esp32s3-devkit-c.img
esptool --port COM15 -b 1152000 write_flash --flash_size detect 0x0 retro-go_619e7-dirty_esp32s3-devkit-c.img

#老的配置，
esptool --port COM15 -b 1152000 write_flash --flash_size detect 0x0 retro-go_619e7-dirty_esp32s3-devkit-c-ESP32S3Cam.img

#新的配置 esp32s3-my
esptool --port COM15 erase_flash
esptool --port COM15 -b 1152000 write_flash --flash_size detect 0x0 retro-go_1bcac-dirty_esp32s3-my.img

esptool --port COM15 -b 921600 write_flash --flash_size detect 0x0 retro-go_esp32s3-st7789v-v1.0-1-g8e5bb_esp32s3-devkit-c.img

将3引脚改成38
esptool --port COM15 -b 2000000 write_flash --flash_size detect 0x0 esp32s3my-3-38.img

将48改成42， 47引脚改成41
esptool --port COM15 -b 2000000 write_flash --flash_size detect 0x0 esp32s3my-48-42-47-41.img


将LCD_CLK改成20， LCD_DC引脚改成19
esptool --port COM15 -b 2000000 write_flash --flash_size detect 0x0 esp32s3my-48-20-47-19.img

将46改成42， 45引脚改成41
esptool --port COM15 -b 2000000 write_flash --flash_size detect 0x0 esp32s3my-48-20-47-19.img

将46改成42， 45引脚改成41
esptool --port COM15 -b 2000000 write_flash --flash_size detect 0x0 esp32s3my-46-42-45-41.img

将LCD_DC改47,LCD_CLK改48
esptool --port COM15 -b 2000000 write_flash --flash_size detect 0x0 esp32s3my-19-47-20-48.img

将LCD_DC改1,LCD_CLK改2
esptool --port COM15 -b 2000000 write_flash --flash_size detect 0x0 esp32s3my-LCD-2-1.img

将LCD_DC改6,LCD_CLK改7，上改2，右改1
esptool --port COM15 -b 2000000 write_flash --flash_size detect 0x0 esp32s3my-LCD-7-6.img




------------
folder 目录， 访问一下， 就会自动生成

  nes

  snes

  gb

  gbc

  gw  game & watch

  sms sega master system

  gg  game gear

  md  genesis col coleco vision

  pce pcengne

  lnx lynx

  doom wad后缀 doom

  msx msx

------------

# 目录
- [用法](#用法)
- [问题](#问题)
- [Development](#development)
- [Acknowledgements](#acknowledgements)
- [License](#license)

# 用法

## 游戏封面 / 图片素材
游戏封面应放置在 SD 卡根目录下的 romart 文件夹中。你可以在[此处](https://github.com/ducalex/retro-go-covers)获取一个预制素材包。Retro-Go 同样兼容旧版的 Go-Play romart 素材包。

若需添加缺失的封面图片，可创建一张 PNG 格式图片（尺寸为 160x168 像素，8 位色深）。系统支持以下两种命名规则：

基于文件名：/romart/nes/Super Mario.png（注意：此处不包含游戏 ROM 文件的扩展名）
基于 CRC32 编码：/romart/nes/A/ABCDE123.png，其中：
nes 与 ROM 文件所在文件夹名称一致；
ABCDE123 是该游戏的 CRC32 编码（在启动器中按下 A 键 -> 选择 “属性” 即可查看）；
A 是 CRC32 编码的第一个字符。

注：预制素材包采用的 “基于 CRC32 编码” 命名方式，其加载速度远慢于 “基于文件名” 的方式！这种命名方式的优势在于：即使不同文件的文件名差异很大，只要它们的 CRC32 编码相同（即文件内容一致），就能匹配到正确封面。但如果你是自行制作封面图片，建议使用 “基于文件名” 的命名格式，并删除 SD 卡中所有基于 CRC32 编码命名的封面文件，以提升系统响应速度。


## BIOS 文件

部分模拟器支持加载 BIOS（基本输入输出系统）文件。此类文件应按以下路径放置：
GB：/retro-go/bios/gb_bios.bin
GBC：/retro-go/bios/gbc_bios.bin
FDS：/retro-go/bios/fds_bios.bin
MSX：在 /retro-go/bios/msx/ 文件夹中放入以下文件：MSX.ROM、MSX2.ROM、MSX2EXT.ROM、MSX2P.ROM、MSX2PEXT.ROM、FMPAC.ROM、DISK.ROM、MSXDOS2.ROM、PAINTER.ROM、KANJI.ROM

## Game & Watch

该平台的 ROM 文件必须使用[LCD-Game-Shrinker](https://github.com/bzhxx/LCD-Game-Shrinker) 工具进行打包处理，相关教程可参考[此处](https://gist.github.com/DNA64/16fed499d6bd4664b78b4c0a9638e4ef)。

## Wifi
若要使用 WiFi 功能，你需要创建一个路径为 /retro-go/config/wifi.json 的配置文件。该文件最多可定义 4 个不同的网络，后续可在菜单中选择对应的网络进行连接。配置文件的内容格式应如下所示：

````json
{
  "ssid0": "my-network",
  "password0": "my-password",
  "ssid1": "my-other-network",
  "password1": "my-password",
  "ssid2": "my-third-network",
  "password2": "my-password",
  "ssid3": "my-last-network",
  "password3": "my-password"
}
````

### 时间同步
成功连接网络后，启动器会立即进行时间同步。
该同步过程通过网络时间协议（NTP）实现，具体是与 pool.ntp.org（公共 NTP 服务器池）进行通信，目前暂不支持关闭此功能。
时区可在启动器的 “选项” 菜单中进行配置。

### 文件管理器
你可以在 Retro-Go 的 “关于”（About）菜单中找到设备的 IP 地址。之后在电脑上打开浏览器，在地址栏输入 http://192.168.x.x/（将 “192.168.x.x” 替换为实际找到的设备 IP 地址），即可访问文件管理器。

## External DAC (headphones)

Retro-Go supports [the external DAC mod for the ODROID-GO](https://github.com/backofficeshow/odroid-go-audio-hat)
which allows high quality audio through headphones. You can switch to it in the menu `Audio Out: Ext DAC`.

<details>
  <summary>Pinout</summary>

  | GO PIN | PCM5102A PIN |
  |--------|---------|
  | 1 | GND |
  | 2 | - |
  | 3 | LCK |
  | 4 | DIN |
  | 5 | BCK |
  | 6 | VIN |
  | 7 | - |
  | 8 | - |
  | 9 | - |
  | 10 | - |
</details>


# 问题

### 黑屏 / 循环启动
Retro-Go 通常会自动检测并解决应用程序崩溃与卡死问题。但如果设备确实陷入循环启动状态，你可以在开机时按住 “向下键（DOWN）”，以返回至启动器界面。

### 音质问题
在 GO 设备上，音量调节无法实现精准衰减：这导致音量调至较高档位时声音过大，而调至较低档位时，又会因数模转换器（DAC）分辨率不足出现声音失真。
若想快速改善音质，可剪断一根扬声器导线，并串联一个 “33 欧姆（左右）” 的电阻。焊接连接的效果更佳，但并非必需 —— 将导线紧密绞合也能正常工作。
[更复杂的解决方案可参考此处。](https://wiki.odroid.com/odroid_go/silent_volume)
此外，你也可以采用本文档前文提及的 “耳机数模转换器（DAC）改装方案”。

### Game Boy SRAM *(又称存档 / 电池 / 备份存储器)*
在 Retro-Go 模拟器中，即时存档（Save States） 能为你提供最佳且最可靠的存档体验。话虽如此，若你需要或希望使用静态随机存取存储器（SRAM）存档，请继续阅读以下内容。该模拟器的 SRAM 存档格式与 VisualBoyAdvance（经典 GBA 模拟器，常简称为 VBA）兼容，因此可用于导入或导出存档文件。
你可以在 “选项” 菜单中配置 SRAM 自动存档功能。延长存档延迟时间能减少游戏卡顿，但代价是若设备断电过快，可能会导致数据丢失。另需注意：当你恢复游戏（Resuming a Game） 时，若存在即时存档文件，Retro-Go 会优先加载该即时存档，而非 SRAM 存档。

### ZIP 压缩文件
目前，Retro-Go 的大部分应用已支持 ZIP 压缩文件。一个 ZIP 压缩包内应当仅包含一个 ROM 文件，不得包含其他任何内容。此外，ZIP 格式的支持情况还取决于设备的可用内存；遗憾的是，对于部分设备而言，较大容量的 ROM 文件可能无法正常加载。

# 开发
If you wish to build or modify Retro-Go, you can find help in the following documents:

- Build instructions in [BUILDING.md](BUILDING.md)
- Theming instructions [THEMING.md](THEMING.md)
- Porting instructions in [PORTING.md](PORTING.md)
- Translating instructions in [LOCALIZATION.md](LOCALIZATION.md)


# Acknowledgements
- The NES/GBC/SMS emulators and base library were originally from the "Triforce" fork of the [official Go-Play firmware](https://github.com/othercrashoverride/go-play) by crashoverride, Nemo1984, and many others.
- The design of the launcher was originally inspired/copied from [pelle7's go-emu](https://github.com/pelle7/odroid-go-emu-launcher).
- PCE-GO is a fork of [HuExpress](https://github.com/kallisti5/huexpress) and [pelle7's port](https://github.com/pelle7/odroid-go-pcengine-huexpress/) was used as reference.
- The Lynx emulator is a port of [libretro-handy](https://github.com/libretro/libretro-handy).
- The SNES emulator is a port of [Snes9x 2005](https://github.com/libretro/snes9x2005).
- The DOOM engine is a port of [PrBoom 2.5.0](http://prboom.sourceforge.net/).
- The Genesis emulator is a port of [Gwenesis](https://github.com/bzhxx/gwenesis/) by bzhxx.
- The Game & Watch emulator is a port of [lcd-game-emulator](https://github.com/bzhxx/lcd-game-emulator) by bzhxx.
- The MSX emulator is a port of [fMSX](https://fms.komkon.org/fMSX/) by Marat Fayzullin.
- PNG support is provided by [lodepng](https://github.com/lvandeve/lodepng/).
- PCE cover art is from [Christian_Haitian](https://github.com/christianhaitian).
- Some icons from [Rokey](https://iconarchive.com/show/seed-icons-by-rokey.html).
- Background images from [es-theme-gbz35](https://github.com/rxbrad/es-theme-gbz35).
- Special thanks to [RGHandhelds](https://www.rghandhelds.com/) and [MyRetroGamecase](https://www.myretrogamecase.com/) for sending me a [G32](https://www.myretrogamecase.com/products/game-mini-g32-esp32-retro-gaming-console-1) device.
- The [ODROID-GO](https://forum.odroid.com/viewtopic.php?f=159&t=37599) community for encouraging the development of retro-go!

# License
Everything in this project is licensed under the [GPLv2 license](COPYING) with the exception of the following components:
- fmsx/components/fmsx (MSX Emulator, custom non-commercial license)
- handy-go/components/handy (Lynx emulator, zlib)
