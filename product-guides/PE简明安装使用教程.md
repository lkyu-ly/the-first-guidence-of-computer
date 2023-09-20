# PE 简明安装使用教程

[toc]

## 前言

在日常使用电脑的过程中，无论是动手安装新的系统，或是对当前系统进行各种修复，抑或是从无法运行的系统中提取重要的资料，我们往往都会发现这些操作大都有一个前提，即原系统是处于部分不可用甚至完全不可用的状态下的。因此，一种用于维护的系统便诞生了，这就是本文介绍的 PE 系统。出于实用性的考量，本文将尽量用最简明的文字介绍 PE 及其安装与使用。

## PE 简介

Windows PE（Preinstallation Environment，即“预安装环境”）是一种被广泛用于为电脑生产商、工作站以及服务器设置定制的操作环境或在离线时排除电脑故障的轻量化 Windows 版本，以下简称为 PE。正因为 PE 是通过在 Windows 操作系统（有时又被称为“母盘”）的基础上删除了许多不必要的功能得到的，其体积可以做到足够小（通常仅为数百 MB），非常适合搭载在运行速率不够快的储存介质上。而且值得注意的是，PE 在启动时是完全加载到内存中运行的，这说明它可以摆脱系统运行中需要占用及读写磁盘的依赖。启动后，即便将搭载 PE 的存储介质直接拔出，也不会影响到 PE 的后续工作。

<div style="margin: 0 auto; text-align: center; width: 60%">
 <img src="https://pic.imgdb.cn/item/65084192204c2e34d3a643dd.png" />
 <a href="https://learn.microsoft.com/zh-cn/windows-hardware/manufacture/desktop/winpe-intro">
  微软官方对 PE 的解释
 </a>
</div>

实际上，使用自己的理解方式来讲，PE 是一个本体一般位于电脑之外的系统，通过从 PE 启动的方式，电脑可以绕过自身硬盘来到电脑外部的另一个操作系统中，因而可以实现已经无法启动系统的电脑的正常启动（即使被启动的并不是原系统），以及对电脑原硬盘的操作。因为此时原硬盘中的系统并未启动，磁盘不会被原系统占用，也通常不会出现因磁盘中系统正在运行而无法操作磁盘的问题。

基于以上基本介绍，本文将从 PE 的制作方法以及使用方法对 PE 进行更为详细的介绍。

## PE 的制作方法

随着计算机知识的普及，用户自行安装、维护系统的需求大幅增加，市面上也出现了功能特色不一的各类 PE 软件，且几乎都号称“面向小白，简单易用”。但非常值得注意的是，并非所有 PE 都值得信任。

从前文可以得知，PE 是通过第三方作者修改原版 Windows 操作系统得到的，所以任何修改版 PE 均存在捆绑流氓软件以及木马程序的风险。在我们和其他大量电脑爱好者的长期实践中，我们选择了两款相对纯净、功能较为全面的 PE 供大家使用：WEPE（微 PE）和 FirPE。下文便会介绍这两种 PE 的安装制作方法。

### 安装准备工作

**硬件方面**：一台能完全正常使用的电脑（最好应为 64 位 Windows 操作系统）、一个 U 盘（容量在 8G 以上） 或者 移动硬盘（最好是固态硬盘）（以下统称为“储存介质”或“介质”）、基本的互联网搜索能力和耐心。

> **预警：绝大多数情况下**，本教程的后续操作步骤中将至少涉及一次对储存介质的格式化操作，届时其中**所有数据将被清空**，请务必提前**做好备份！！！**

**软件方面**：本文中可能需要用到的软件以及官方网址有：

|                 软件名称                  |           官方网址           |
| :---------------------------------------: | :--------------------------: |
|  WEPE（微 PE）（PE 环境）（本体+安装器）  |  <https://www.wepe.com.cn/>  |
|      FirPE（PE 环境）（本体+安装器）      | <https://firpe.cn/page-247>  |
| Ventoy（镜像启动引导环境）（本体+安装器） | <https://www.ventoy.net/cn/> |
|   Rufus（启动盘制作软件）（仅为安装器）   |    <https://rufus.ie/zh/>    |

其中，前两种 PE 环境为二选一使用，Ventoy 和 Rufus 可以根据进阶需求进行选用。除此之外，还有其他你想随介质一同带入目标电脑的软件（例如图吧工具箱等）和系统镜像也应当进行准备。

### WEPE 的制作

- **注**：注意到 WEPE 中磁盘分区管理工具 DiskGenius 为 32 位，其某些操作对于装有 64 位操作系统的磁盘无效，因此如果有磁盘管理需求的同学请考虑下一节所介绍的 FirPE。另由于两种 PE 的制作方法大同小异，本文目前先详细介绍 WEPE 的制作方法。

在官网下载好 WEPE 的 64 位安装包（基于当下的电脑处理器以及系统情况，本文默认目标维护电脑为 64 位，如果目标维护电脑为 32 位处理器/系统，请下载所有对应软件的 32 位版本使用）。**关闭本机所有杀毒软件**然后再**以管理员模式运行**`WePE64_Vx.x.exe`，稍作等待后你应该会看到如下的界面：

<div style="margin: 0 auto; text-align: center; width: 60%">
<img src="https://pic.imgdb.cn/item/65084192204c2e34d3a6440b.png" style="border-width: 1.5px;border-style: solid;border-color: rgb(239, 112, 96);" />
</div>


由于本文中我们的目的是要制作 PE 系统，应将 U 盘作为其载体，所以此时应点击上图红框中的“安装 PE 到 U 盘”按钮，进入下图所示的界面，并插入待安装 U 盘（上一步提前插入也是可以的）：

<div style="margin: 0 auto; text-align: center; width: 60%">
<img src="https://pic.imgdb.cn/item/65084192204c2e34d3a643c6.png" style="border-width: 1.5px;border-style: solid;border-color: rgb(239, 112, 96);" />
</div>


对于这个安装界面，我们将其上的各项内容解释如下（未提及的选项一般保持默认）：

- **安装方法**：对于目标是适用于绝大多数电脑的 PE 环境安装，我们选择软件推荐的安装方式。

    <a name="待写入U盘"></a>

- **待写入 U 盘**：为防止误格式化，软件默认不会显示电脑内置的硬盘，请根据名称和大小选择正确的目标安装位置。若此列表中没有观察到你的目标设备，请检查其占用情况，重新插拔介质或是关闭可能占用截止进行读写的软件，避免出现因设备被占用而无法进行读写的情况。

- **格式化**：默认选项即可。

- **卷标**：制作好的 PE 启动盘的可使用部分在资源管理器中显示出来的分区名，可以随意取名，但注意尽量不要带有中文字符。

完成上述选项的修改后，点击下方的“立刻安装进 U 盘”按钮，软件会弹出如下提示：

<div style="margin: 0 auto; text-align: center; width: 60%">
  <img src="https://pic.imgdb.cn/item/65084192204c2e34d3a643d0.png" style="border-width: 1.5px;border-style: solid;border-color: rgb(239, 112, 96);" />
</div>

确定一切准备无误后，即可点击“开始制作”将 PE 写入介质中，如果流程正确的话，你将会看到下面的安装进度界面：

<div style="margin: 0 auto; text-align: center; width: 60%">
  <img src="https://pic.imgdb.cn/item/650841d2204c2e34d3a64a1d.png" style="border-width: 1.5px;border-style: solid;border-color: rgb(239, 112, 96);" />
</div>

> 注：
>
> 1. 安装过程花费的时间大部分取决于介质的性能，高性能 U 盘/移动硬盘安装速度较快。
> 2. 再次提醒本过程需要关闭杀毒软件，否则写入 U 盘的工具类程序有被误报误杀的风险。

安装完成后，看到如图所示界面就代表安装成功，此时点击完成安装即可。

<div style="margin: 0 auto; text-align: center; width: 60%">
  <img src="https://pic.imgdb.cn/item/650841da204c2e34d3a64b0a.png" style="border-width: 1.5px;border-style: solid;border-color: rgb(239, 112, 96);" />
</div>

此时我们不要急于拔出 U 盘/硬盘，来到资源管理器，制作好 PE 的介质并不会消失不见，但其可用空间会略有减少（这是因为 PE 系统的本体以及其独立的引导分区已经被设置为隐藏分区，显示出来的是 PE 制作软件留下来的可供我们自行储存文件的空白分区）：

<div style="margin: 0 auto; text-align: center; width: 40%">
  <img src="https://pic.imgdb.cn/item/650841da204c2e34d3a64b12.png" />
</div>

此时我们的 U 盘仍是可用的，可以将其他想随介质一同带入目标电脑的软件和系统镜像放入此空间，后续这些文件也会显示在启动的 PE 系统中。

以上操作完成后，正常弹出储存介质，我们便完成了 WEPE 的制作。

### FirPE 的制作

FirPE 的制作与 WEPE 的制作大同小异，学会 WEPE 的制作后便可基本上手 FirPE 的制作方法。
<div style="margin: 0 auto; text-align: center; width: 60%">
  <img src="https://pic.imgdb.cn/item/650841da204c2e34d3a64b24.png" />
</div>

设备的选择参考上一节的[待写入 U 盘](#待写入U盘)。选择调整好以上参数后，点击全新制作即可。

<div style="margin: 0 auto; text-align: center; width: 40%">
<img src="https://pic.imgdb.cn/item/650841da204c2e34d3a64b32.png" />
</div>

确认注意事项后，等待进度条跑完即可完成制作。

<div style="margin: 0 auto; text-align: center; width: 60%">
  <img src="https://pic.imgdb.cn/item/650841da204c2e34d3a64b4d.png" />
  写入过程
  <br>
</div>

<div style="margin: 0 auto; text-align: center; width: 40%">
<img src="https://pic.imgdb.cn/item/650841e7204c2e34d3a64d5d.png" />
制作成功
</div>

### 使用 Ventoy 引导各种 PE 和系统镜像

一般来说，以上两种方式已经可以制作出稳定易用的 PE，完全可以满足想临时使用一次 PE 环境的同学的几乎全部需求。因此，Ventoy 并不需要这类同学去学习其安装使用，但如果你主张启动盘的优雅高效，或是需要经常使用 PE 来维护系统，抑或经常需要直接运行各种不同的系统镜像进行部署，那么我很推荐大容量移动硬盘 + Ventoy 的安装方案。

> _简单来讲，Ventoy 是一个制作可启动 U 盘的开源工具。有了 Ventoy 你就无需反复地格式化 U 盘，你只需要把 ISO/WIM/IMG/VHD(x)/EFI 等类型的文件直接拷贝到 U 盘里面就可以启动了，无需其他操作。你可以一次性拷贝很多个不同类型的镜像文件，Ventoy 会在启动时显示一个菜单来供你进行选择。你还可以在 Ventoy 的界面中直接浏览并启动本地硬盘中的 ISO/WIM/IMG/VHD(x)/EFI 等类型的文件。Ventoy 安装之后，同一个 U 盘可以同时支持 x86 Legacy BIOS、IA32 UEFI、x86_64 UEFI、ARM64 UEFI 和 MIPS64EL UEFI 模式，同时还不影响 U 盘的日常使用。_
>
> <p align="right"><i>—— Ventoy 官网</i></p>

在 Ventoy 官网的[文档手册](https://www.ventoy.net/cn/doc_news.html)中，“[Ventoy 内部原理](https://www.ventoy.net/cn/doc_composition.html)”一章对 Ventoy 的内部原理，以及针对 MBR 和 GPT 格式的磁盘的分区构成进行了较为详细的说明，这里就不再转述。只是需要注意的是：如果你的硬盘中有较大量数据并且想使用“无损安装”功能的话，请详细阅读官方文档关于此功能的介绍和使用限制。由于此安装方式需要拥有较为深层的磁盘知识，仅适合更加进阶的使用，本文仅介绍较为通用的“全新安装”方式。

<div style="margin: 0 auto; text-align: center; width: 60%">
<img src="https://pic.imgdb.cn/item/650841e7204c2e34d3a64d68.png" />
</div>


在详细阅读了 WEPE 和 FirPE 制作方法的基础上，你应该已经学会了储存介质的选择，除此之外所有设置保持默认即可，如有需求可以自行更改，这里不再赘述。点击安装按钮即可安装Ventoy到设备中：

<div style="margin: 0 auto; text-align: center; width: 60%">
<img src="https://pic.imgdb.cn/item/650841e7204c2e34d3a64d76.png" />
</div>

安装完成后，你只需要将各种系统、PE 的 ISO 镜像文件移入 U 盘，即可完成可以用于启动他们的 Ventoy 设备。本文提到的两种 PE 的 ISO 镜像文件提取方式详见下一节。另外，当日后 Ventoy 发布新版本时，仅需重新下载 Ventoy 软件，插入并选择已安装旧版本 Ventoy 的储存介质，点击“升级”即可进行更新。

### 使用 Rufus 自行制作可启动介质

<div style="margin: 0 auto; text-align: center; width: 60%">
<img src="https://pic.imgdb.cn/item/650841e8204c2e34d3a64d8c.png" />
Rufus 官网介绍
</div>


在极个别情况下，使用 WEPE 和 FirPE 制作的设备不能在某些机器上正确启动，此时可以使用 Rufus 手动创建可启动介质。

首先导出 PE 系统的安装镜像：在 WEPE 软件中，“生成可启动 ISO ”功能位于软件右下角的光盘图标；FirPE 软件中，同样在左下角有生成 ISO 的选项。点击即可导出 ISO 镜像。

<div style="margin: 0 auto; text-align: center; width: 60%">
<img src="https://pic.imgdb.cn/item/650841e8204c2e34d3a64daa.png" />
</div>

在 Rufus 工具中选择合适的介质，在“Boot selection”选项中点击右侧的“SELECT”找到并选择刚刚导出的镜像文件，其他选项默认即可，点击“START”即可开始制作。

## PE 的使用

PE 制作完成后，我们需要通过修改 BIOS 启动顺序的方式将电脑设置为从介质中刚刚安装好的 PE 启动。进入 BIOS 的方法如下：

> **注**：BIOS 的入口主要由主板的不同而不同，在使用计算机时不会明确显示其进入控制的入口。各个生产厂家可能会在计算机自检过程中给出提示（这在笔记本的启动中较为少见），**通过在启动阶段使用快捷键（热键）开启 BIOS 控制台界面**。对于不同品牌的笔记本电脑，其进入 BIOS 的快捷键也是因厂家而不同，具体情况请在网络上自行搜索“笔记本厂商/型号 + BIOS 快捷键”关键词。

电脑关机的情况下，插入制作好 PE 的储存介质，在电脑启动并显示 LOGO 后，**立即不停连续点按**查到的快捷键（快捷键属于 Fn 键区的，若一次启动不成功，可尝试同时/分别点按 Fn + 快捷键），直到进入 BIOS 为止。不同厂商 BIOS 各不相同，对鼠标的支持情况也多有不同，但几乎全部支持键盘操作。

欲进入 PE 环境，需要修改电脑启动顺序。找到屏幕上带有“BOOT”关键词的选项，进入类似于“Boot Sequence”的选项（部分拥有图形化界面的 BIOS 设置方法有所不同，但都很好找）。总之来到一个列出了电脑可启动磁盘（应该包括电脑自带硬盘和新插入的介质）的页面，通过鼠标/键盘按照 BIOS 给出的操作提示移动新介质的位置到最前面，最后使用 F10（大多数如此）保存并退出。退出后电脑会自动重启。若上面的设置完全正确，重启后电脑便应该自动进入到到你所安装的 PE 环境中。至此，制作并进入 PE 的全过程结束。

## 写在后面



