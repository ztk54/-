---
tags:
  - Git
  - SSH
时间: 2026-02-10T12:05:00
---
>本文将带你完成：
>1. 用TortoiseGit同时关联Gitee与GitHub仓库。
>2. 配置更稳定的SSH协议推送（解决GitHub 443错误）。
>3. 解决SSH配置中的关键报错（如：No supported authentication methods available）

**Gitee** 是国内的码云平台，连接稳定，访问速度更快； **Github** 是全球最大的开源网站，有更加丰富的内容。我们平常写代码，写笔记的时候可以把这些内容上传到这些代码托管平台。两者各有其优劣，所以我选择全要，接下来我将以一个新手的身份讲一讲怎么恰当的把代码同时托管到Github和Gitee。  
> 注：以下git软件均使用的是git的图形化软件，**TortoiseGit**，更方便操作。
# 1.初始化任一仓库  
* 首先我们需要在gitee或者github 任一平台新建一个全新的仓库，复制其HTTPS协议（如果是github，更建议使用SSH协议，推送将不再经过443端口，访问更加稳定，下面我也将讲述怎么使用SSH协议）。
* 接下来打开我们需要管理的文件夹，鼠标右键之后点击克隆，这时候toritosegit会自动将我们刚刚复制的HTTPS或者SSH填写上去，我们点击确认即可初始化完成第一个仓库。  
# 2.添加另一个仓库  
* 与第一步操作类似，我们在另一平台上也初始化创建一个仓库。
* 我们在刚刚初始化完成的那个文件夹中右键，移动鼠标到tortoisegit上面，点击右边显示的设置二字。
* 点击Git下面的远端二字，然后选择添加一栏，为我们的另一个远端进行命名（如命名为Gitee或者Github，方便区分），在下面的URL里面填写我们复制的HTTPS协议或者SSH协议。之后选择确定，我们即设置了推送的另一个远端。
# 3. 如何推送到两个仓库  
在提交代码或者其他内容时候，选择push之后，我们要在弹出的“远程Remote”一栏中选择全部，这样子我们就可以同时把内容推送到Gitee和Github了。  
# 4.设置SSH协议  
> 相较于HTTPS协议，SSH协议对网络需求更小，尤其是针对Github经常访问不稳定的情况，因此我极其推荐使用SSH协议推送到Github。  
>SSH的配置对于新手来说，可能不是很容易上手，我当初也是遇到了各种各样的问题才成功完成，但是一旦配置完成，即可一劳永逸，接下来我将尽可能通俗的讲述怎么去完成这个步骤。 
* 打开Git Bash：我们打开Git Bash，在任意界面右击鼠标，选择显示更多选项，然后选择Open Git Bash Here。
* 生成密钥对：
	* 在命令行中输入以下命令`ssh -keygen -t rsa -b 4096 -C "这里输入你注册Github的邮箱"`，请注意这里的C是大写C，小写c是另一命令。
	* 接下来连续敲击三次回车键。（注：其中有设置密码的选项，我们这里可选可不选，没有强制要求）
	* 其中有一步会显示我们生成公钥的保存地址，可以记录一下，类似以下的形式:
	* ```
	  Generating public/private rsa key pair.
	Enter file in which to save the key (/c/Users/你的用户名/.ssh/id_rsa):
	  ```
* 打开公钥文件：我们按照上面的路径找到生成的文件，或者可以直接使用[Everything](https://www.voidtools.com/zh-cn/downloads/)，搜索id_rsa。然后我们会看到两个分别名为`id_rsa`和` id_rsa.pub`的文件， 前者为私钥匙(保密，不给别人)，后者为公钥，也就是我们需要的那个。右键选择后者，选择用记事本打开，然后全选所有内容并复制。
* 登录Github添加密钥：点击头像，选择Settings。在左侧菜单栏中找到SSH和GPG keys，点击Nes SSH key按钮。为我们的公钥填写一个名字，Key type保持默认，在 Key里粘贴刚刚复制的公钥内容。最后点击Add SSH key即可。
# 5.修改TortoiseGit中的远程地址（HTTPS修改为SSH）
打开所需的文件夹，右键点击TortoiseGit中的设置，选择Git-->远端，选中之前设置的Github，将里面的URL修改为SSH地址。SSH地址格式为 `git@github.com:用户名/仓库名.git`，点击应用即可。
# 6.首次使用SSH协议推送内容 
* 打开Git Bash调试：
	* 输入：`ssh -T git@github.com`
	* 第一次连接会显示：```The authenticity of host 'github.com (IP地址)' can't be established.  RSA key fingerprint is SHA256:一串字符...
	Are you sure you want to continue connecting (yes/no/[fingerprint])?```
	 我们输入yes点击回车即可。
	 如果成功的话，我们会看到：
	 `Hi 你的用户名! You've successfully authenticated, but GitHub does not provide shell access.`
* 之后我们打开文件夹进行推送代码，第一次使用SSH可能会弹出确认窗口，我们勾选保存，然后点击“是”
* 但是我们却发现，代码推送并不能成功，这是为什么呢？这是由于我们使用的TortoiseGit并不能识别出私钥（二者保存路径不同）。我们打开TortoiseGit设置，选择网络，找到下面SSH客户端一项，将路径改为Git自带的SSH客户端路径，通常为 `C:\Program Files\Git\usr\bin\ssh.exe`路径，如果你安装Git改了文件路径，请找到对应的ssh.exe文件（可以使用Everything），点击应用，这样就完成了。
# 7.一些其他问题
按照我们上面的步骤，本应是能够与完成推送过程的，但是其中也会有一些其他问题，下面我列出我遇到的一些问题，以供大家参考。
* 在Git Bash一步中显示`command not found`。
	* Git版本过低，我们输入`git ---version`，查看git版本，版本过低请去官网进行更新安装，
	* 输入指令错误，比如那里的大写C输入为小写c，自然找不到这个command。
* 在用github进行第一次推送的时候，我们可能会显示一个`Github Sign in`的界面，这里我们推荐选择`Sign in with browser`，提前打开浏览器登录好Github，就能通过验证了。
作为一名新手，我的内容可能还不是很完善，可能按照我的步骤来，最后却没有达到预期的效果。希望能和大家多多包涵，有什么问题了提出来我会尽己所能来帮助大家。