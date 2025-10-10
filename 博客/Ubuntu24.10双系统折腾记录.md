#博客
## 安装 TODO

## 配置 TODO

## 问题
1. 时不时整个屏幕卡住，键盘、鼠标输入完全无反应，但 KDE Connect 仍然可以控制，说明后台程序可以正常运行，推测是 Nvidia 显卡驱动导致的图形界面崩溃。
> [!failure] 
> 直接检查 Software & Updates 中的 Additional Drivers，发现已选用推荐的驱动程序，未解决。

> [!failure] 
> 尝试 `sudo ubuntu-drivers autoinstall`，开始自动安装更新驱动，再次检查 Additional Drivers，发现原驱动不再是推荐选项，选用新的推荐驱动，效果观察中。
> 2025/8/25 UPDATE: 又炸了，烦

> [!question] 
> 怀疑是 snap 版 Firefox 的性能问题导致，卸载改装 Debian 版。


2. 在 Gnome-Look 上找了一个 grub 美化，发现 grub 主体部分过小，严重影响美观。
> [!failure] 
> 尝试修改 `/etc/default/grub`，将 GRUB_GFXMODE 修改为 `1440x900`，无明显效果。

> [!question] 
> 再次修改 `/etc/default/grub`，将 GRUB_GFXMODE 修改为 `800x600`，此外根据文件注释推荐，取消 `GRUB_DISABLE_OS_PROBER` 一行的注释。
> 虽然主体部分放大了，但分辨率太小看的有点难受，而且 Ubuntu 的启动 Logo 分辨率也降了，很难说整体有没有更美观，继续折腾。

> [!success] 
> 将 GRUB_GFXMODE 修改为 `1280x800`，顺眼多了，就当问题解决。

3. 在系统一段时间不活跃后，显示画面会暂时停止更新，此时移动鼠标、键盘输入会有明显延迟。
> [!success] 
> 推测是 tlp 省电策略的问题，导致输入设备被临时关闭。尝试修改 `/etc/tlp.conf`，将 `USB_AUTOSUSPEND` 禁用。