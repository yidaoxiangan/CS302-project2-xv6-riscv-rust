##运行环境：
####系统环境(Ubuntu20.04)：

    Linux book-VirtualBox 5.8.0-53-generic #60~20.04.1-Ubuntu SMP Thu May 6 09:52:46 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux
    
####仿真环境(Qemu):

    wget https://download.qemu.org/qemu-4.1.0.tar.xz  #下载后解压并进入目录
    ./configure                                       #默认安装所有目标平台，产物路径为/usr/local/bin
    make -j4
    make install

####语言(Rust)

    1. curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh   #下载Rust安装包
    2. source ~/.bashrc
    3. cd ./72-code
    4. rustup target add riscv64gc-unknown-none-elf                     #添加交叉编译器
    5. cargo run

##使用说明：

    cd ./72-code
    cargo run       #编译并运行程序

    1. 运行成功后可以打字，但最多只能输入128个字符。目前按退回键并不会删除字符，只会输出空格。

    2. 将src/process/proc.rs文件中，syscall()函数的a7修改一下：
    更改为1可以测试fork，目前fork会一直运行，生成63个子进程。
    更改为5目前read没实现（因为原项目过于残缺控制台不能输入任何字符，要先实现控制台的输入才能实现sys_read，因此我们去实现了控制台的输入）。
    更改为7可以测试sys_exec，但目前没能完全实现，只实现了path转inode这一步，第二步加载elf文件到内存还没实现（这是因为elf文件需要file system的支持，但原项目过于残缺，file system没实现完全，因此我们去尝试修复补全file system）
    
    3. 本文件保留了git，并且没有commit，因此可以用vscode打开，下载git插件，在左侧栏从上往下选第三个源代码管理，可以看到所有的代码更改记录，并与源文件做对比。

git比较文件如下：

![](../images/git.png)