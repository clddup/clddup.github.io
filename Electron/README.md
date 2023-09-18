> Electron

Build cross-platform desktop apps with JavaScript, HTML, and CSS

## MAC Gatekeeper 机制下获取应用真正的路径

在 Electron 项目开发中, 发现在 mac 中未签名并且应用程序不在 application 文件夹下的时候, 因为 Gatekeeper 机制, 通过 app.getAppPath 方法获取到的是一个临时只读目录, 无法对其进行读写操作

在解决过程中, 通过对只读目录与真实目录的对比, 发现 inode 相同, 通过研究发现是通过挂载实现的

所以最后的解决办法是通过 mount 命令拿到系统挂载信息, 解析挂载信息, 通过临时只读路径得到真实路径
