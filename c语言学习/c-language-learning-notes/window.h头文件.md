---
tags:
  - C语言
  - windows
时间: 2026-02-11T22:32:00
---
1.` sleep()`，中间单位是毫秒。
1. `system("")`，中间输入在cmd的具体命令。如：
	1. `system("shutdown -s -t 60")`里面的数字可以更改，用于控制电脑关机。
	2. `system("shutdown -a")`取消电脑关机
	3. `system(""cls)`清理屏幕