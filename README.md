# **AMD V620 to W6800 Firmware Flashing Guide**

[简体中文版本](#简体中文版本)

## **Why?**

* You get display output.
* You get wider compatibility with systems.
* For all intents and purposes it is a W6800 with a single display output.
* Higher boost clocks.
* You keep the 32GiB of ECC.

## **Why Not?**

* You lose Compute Units. The W6800 has 60 CUs. The V620 has 72 CUs. When I flashed it, I ended up getting 54 CUs due to the configuration of the die.

## **Motherboard Compatibility**

The V620 on its default firmware seems to be incredibly strict about what motherboards it supports. I tried it on 2 X570 AM4 boards (ASUS, AsRock) and it refused to boot. I tried a B450M (AsRock) motherboard and it booted fine. The primary reason I flashed it was simply for wider compatibility. I am unsure what motherboards are actually compatible with the V620 on its default FW, and why.

## **Fan Shroud**

I designed a compact fan shroud specifically for this card and many others. They can be found here:

* [Printables Link](https://www.printables.com/model/1712035-amd-v340-v520-v620-mi25-mi50-mi60-mi100-mi210-fan)
* [Thingiverse Link](https://www.thingiverse.com/thing:7348050)
* [MakerWorld Global Link](https://makerworld.com/en/models/2762655-amd-v340-v520-v620-mi25-mi50-mi60-mi100-mi210-fan)
* [MakerWorld China Link](https://makerworld.com.cn/zh/models/2469986-amd-v340-v520-v620-mi25-mi50-mi60-mi100-mi210-feng)

## **Prepping for Flashing**

### **UEFI Settings**

* **PCIe Speed** -> Gen3
* **IOMMU** -> Enabled
* **Above 4G Decoding** -> Enabled
* **Re-Size BAR Support** -> Enabled
* **SR-IOV Support** -> Enabled
* **CSM** -> Disabled

## **Flashing**

Run the following command:

```bash
amdvbflash -p 0 AMD.RadeonProW6800.32768.210422.rom --config v620.cfg
```

## **Post-Flashing**

### **UEFI Settings**

* **PCIe Speed** -> Auto or Gen4.
* All other settings can be reverted if you wish.
1. Turn off the machine and make sure the GPU gets power cycled.
2. Remove the portion of the grate blocking the mini displayport with side cutters.
3. To use this card on Windows you MUST use the PRO drivers and not the Adrenalin drivers.

## **Setting up Fan Curves**

Turn off Secure Boot if you have it on.

```bash
sudo apt install lm-sensors  
sudo sensors-detect  
sensors  
sudo pwmconfig  
sudo systemctl start fancontrol  
sudo systemctl enable fancontrol
```

Add `acpi_enforce_resources=lax` to `GRUB_CMDLINE_LINUX_DEFAULT`.
Enable Secure Boot if you want it back.

### **Example Settings for fancontrol**

```text
INTERVAL=5  
DEVPATH=hwmon0=devices/platform/nct6775.656 hwmon10=devices/pci0000:00/0000:00:03.1/0000:16:00.0/0000:17:00.0/0000:18:00.0  
DEVNAME=hwmon0=nct6796 hwmon10=amdgpu  
FCTEMPS=hwmon0/pwm4=hwmon10/temp3_input  
FCFANS= hwmon0/pwm4=hwmon0/fan4_input  
MINTEMP=hwmon0/pwm4=45  
MAXTEMP=hwmon0/pwm4=75  
MINSTART=hwmon0/pwm4=22  
MINSTOP=hwmon0/pwm4=22  
MINPWM=hwmon0/pwm4=22  
MAXPWM=hwmon0/pwm4=255  
DEVPATH=hwmon0=devices/platform/nct6775.656 hwmon10=devices/pci0000:00/0000:00:03.1/0000:16:00.0/0000:17:00.0/0000:18:00.0  
DEVNAME=hwmon0=nct6796 hwmon10=amdgpu  
FCTEMPS=hwmon0/pwm3=hwmon10/temp3_input  
FCFANS= hwmon0/pwm3=hwmon0/fan4_input  
MINTEMP=hwmon0/pwm3=45  
MAXTEMP=hwmon0/pwm3=75  
MINSTART=hwmon0/pwm3=22  
MINSTOP=hwmon0/pwm3=22  
MINPWM=hwmon0/pwm3=22  
MAXPWM=hwmon0/pwm3=255
```

<a id="简体中文版本"></a>
## **简体中文版本 - AMD V620 刷 W6800 固件指南**

## **为什么选择刷固件？**

* 可以获得视频信号输出。 
* 获得更广泛的主板兼容性。 
* 实际使用体验基本上就是一张带单个 Mini-DP 接口的 W6800。 
* 获得更高的核心加速频率（Boost Clocks）。 
* 保留 32GiB ECC 显存。

## **潜在代价**

* 会损失一部分计算单元（Compute Units）。W6800 原生拥有 60 个 CU，而 V620 拥有 72 个 CU。在实际刷入固件后，由于晶圆芯片内部的架构配置原因，最终可用数量会变为 54 个 CU。

## **主板兼容性**

V620 在默认固件下对主板的挑剔程度极其严苛。曾尝试在 2 款 X570 AM4 主板（华硕、华擎）上运行，均无法开机点亮。但在另一款 B450M（华擎）主板上则能正常启动。因此，选择刷固件的最核心原因就是为了彻底解决主板兼容性问题。目前尚不明确哪些主板能原生兼容默认固件下的 V620，也无法确定具体原因。

## **导风罩设计**

专门为该显卡及其他多款同类型显卡设计了一款紧凑型风扇导风罩。3D 打印模型文件可在以下网站下载：

* [Printables 链接](https://www.printables.com/model/1712035-amd-v340-v520-v620-mi25-mi50-mi60-mi100-mi210-fan)
* [Thingiverse 链接](https://www.thingiverse.com/thing:7348050)
* [MakerWorld 国际站链接](https://makerworld.com/en/models/2762655-amd-v340-v520-v620-mi25-mi50-mi60-mi100-mi210-fan)
* [拓竹 MakerWorld 中国站链接](https://makerworld.com.cn/zh/models/2469986-amd-v340-v520-v620-mi25-mi50-mi60-mi100-mi210-feng)

## **刷机前准备**

### **UEFI/BIOS 设置**

* **PCIe Speed**（PCIe 速率） -> Gen3
* **IOMMU** -> Enabled（开启）
* **Above 4G Decoding**（大于 4G 解码） -> Enabled（开启）
* **Re-Size BAR Support** -> Enabled（开启）
* **SR-IOV Support** -> Enabled（开启）
* **CSM** -> Disabled（关闭）

## **开始刷机**

运行以下指令：

```bash
amdvbflash -p 0 AMD.RadeonProW6800.32768.210422.rom --config v620.cfg
```

## **刷机后处理**

### **UEFI/BIOS 设置**

* **PCIe Speed**（PCIe 速率） -> Auto（自动）或 Gen4
* 如果有需要，其余设置项均可以恢复原状。
1. 关闭电脑电源，并确保显卡完全断电重启（Power Cycle）。
2. 使用斜口钳剪掉显卡挡板上阻挡 Mini DisplayPort 接口弹片的金属格栅。
3. 要在 Windows 上使用这张卡，你必须使用 PRO 驱动，而不是 Adrenalin 驱动。

## **配置风扇调速曲线**

如果开启了安全启动（Secure Boot），请先将其关闭。

```bash
sudo apt install lm-sensors
sudo sensors-detect
sensors
sudo pwmconfig
sudo systemctl start fancontrol
sudo systemctl enable fancontrol
```

在系统引导参数 `GRUB_CMDLINE_LINUX_DEFAULT` 中添加 `acpi_enforce_resources=lax`。
如果需要，可以重新开启安全启动（Secure Boot）。

### **fancontrol 配置文件示例**

```text
INTERVAL=5
DEVPATH=hwmon0=devices/platform/nct6775.656 hwmon10=devices/pci0000:00/0000:00:03.1/0000:16:00.0/0000:17:00.0/0000:18:00.0
DEVNAME=hwmon0=nct6796 hwmon10=amdgpu
FCTEMPS=hwmon0/pwm4=hwmon10/temp3_input
FCFANS= hwmon0/pwm4=hwmon0/fan4_input
MINTEMP=hwmon0/pwm4=45
MAXTEMP=hwmon0/pwm4=75
MINSTART=hwmon0/pwm4=22
MINSTOP=hwmon0/pwm4=22
MINPWM=hwmon0/pwm4=22
MAXPWM=hwmon0/pwm4=255
DEVPATH=hwmon0=devices/platform/nct6775.656 hwmon10=devices/pci0000:00/0000:00:03.1/0000:16:00.0/0000:17:00.0/0000:18:00.0
DEVNAME=hwmon0=nct6796 hwmon10=amdgpu
FCTEMPS=hwmon0/pwm3=hwmon10/temp3_input
FCFANS= hwmon0/pwm3=hwmon0/fan4_input
MINTEMP=hwmon0/pwm3=45
MAXTEMP=hwmon0/pwm3=75
MINSTART=hwmon0/pwm3=22
MINSTOP=hwmon0/pwm3=22
MINPWM=hwmon0/pwm3=22
MAXPWM=hwmon0/pwm3=255
```
