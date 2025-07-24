# DX12 VS2022环境搭建踩坑

![Box](https://picgo-1256575825.cos.ap-guangzhou.myqcloud.com/202507250001810.webp)

在2025年基本上不管Unity还是UE都至少推荐装VS2022的版本了，但龙书《DirectX 12 3D 游戏开发实战》的代码库还默认用的是VS2015，所以项目配置多少有些不一样，这里记录一下踩的几个坑。

- 编译遇到C2102等报错：[设置“符合模式”为“否”](#_2)
- 无法解析的外部符号main：[设置“子系统”为“窗口”](#_3)

!!! note "What if"
    如果你是下载的国内中文版本的配套资源，直接打开里面的`MyD3D12Project`，可能也就不存在这些问题。

    国内中文版配套资源下载地址：[异步社区 - DirectX 12 3D 游戏开发实战](https://www.epubit.com/bookDetails?id=N36691)

## 搭建工程

首先照常拉取随书附带的代码仓库：[d3dcoder/d3d12book](https://github.com/d3dcoder/d3d12book)

确保Visual Studio中已经安装好了C++项目的前置内容（通过Visual Studio Installer修改工作负荷）：

![使用C++的桌面开发](https://picgo-1256575825.cos.ap-guangzhou.myqcloud.com/202507242320463.webp)

打开VS2022，新建C++的空项目，项目目录放置在d3d12book下的一个新建文件夹下，例如`d3d12book/MyDemos/HelloDX12`，注意需要勾选“将解决方案和项目放在同一目录中”：

![New Project](https://picgo-1256575825.cos.ap-guangzhou.myqcloud.com/202507242326815.webp)

之后按照书上的步骤，将`d3d12book\Chapter 6 Drawing in Direct3D\Box`下的：

- `BoxApp.cpp`代码
- `Shaders`目录

**拷贝**一份到工程文件夹`d3d12book\MyDemos\HelloDX12`下：

![copy files](https://picgo-1256575825.cos.ap-guangzhou.myqcloud.com/202507242328085.webp)

然后，在VS的解决方案资源管理器工具栏下，右键`HelloDX12`（或你写的工程名），选择“添加/现有项”，将`BoxApp.cpp`添加到项目中：

![Add code](https://picgo-1256575825.cos.ap-guangzhou.myqcloud.com/202507242331846.webp)

再进行一次“添加/现有项”，这次将`d3d12book\Common`下的所有`.h`和`.cpp`文件都添加到工程（不需要拷贝代码，仅添加）：

![Add common](https://picgo-1256575825.cos.ap-guangzhou.myqcloud.com/202507242332955.webp)

这里如果直接运行，VS编译结束后会向你抱怨一些问题：

![Errors](https://picgo-1256575825.cos.ap-guangzhou.myqcloud.com/202507242334222.webp)

## 设置“符合模式”为“否”

这里的报错主要是因为VS2022工程默认开启“符合模式”，对C++语言检查更加严格，旧代码一些写法在新标准下无法通过编译验证，我们主要做DX12开发练习，没有必要去重新全部修改代码，关掉该模式即可。

右键解决方案“HelloDX12”（注意不要右键到最上面那个），选择最下方的“属性”，找到“配置属性 -> C/C++ -> 语言”，选择“符合模式”为“否(/permissive)”，更改后点击“应用”：

![permissve](https://picgo-1256575825.cos.ap-guangzhou.myqcloud.com/202507242340115.webp)

重试编译后，可以看到之前的报错内容都不会再提示，但又出现了新的问题：

![no main](https://picgo-1256575825.cos.ap-guangzhou.myqcloud.com/202507242342320.webp)

## 设置“子系统”为“窗口”

这里的原因是VS2022新建的空项目，默认是使用的控制台程序模式，程序会在代码集中寻找`int main(void)`函数作为入口函数，而我们程序明显不是控制台，而是一个窗口程序（毕竟是渲染程序），可以在`BoxApp.cpp`中找到窗口程序的入口函数`WinMain`：

```cpp
int WINAPI WinMain(HINSTANCE hInstance, HINSTANCE prevInstance,
                   PSTR cmdLine, int showCmd)
{
    // codes
}
```

同样的，右键打开解决方案属性，找到“配置属性 -> 连接器 -> 系统”下的“子系统”，将选项修改为“窗口(/SUBSYSTEM:WINDOWS)”：

![subsys](https://picgo-1256575825.cos.ap-guangzhou.myqcloud.com/202507242345890.webp)

## 大功告成！

再次尝试编译运行，就能看到Chapter 6的Box顶点着色实例程序了，左键控制Box旋转，右键控制摄像机离Box的距离。

![Box](https://picgo-1256575825.cos.ap-guangzhou.myqcloud.com/202507250001810.webp)
