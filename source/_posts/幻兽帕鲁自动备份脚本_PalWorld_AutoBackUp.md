---
title: 幻兽帕鲁自动备份脚本_PalWorld_AutoBackUp
abbrlink: 6eed18b3
date: 2024-01-26 10:39:55
---

# PalWorld_AutoBackUp

## 幻兽帕鲁自动备份脚本_PalWorld_AutoBackUp

GitHub仓库地址：[XIAOZHAOXS/PalWorld_AutoBackUp](https://github.com/XIAOZHAOXS/PalWorld_AutoBackUp)

```powershell
@echo off
setlocal enabledelayedexpansion

:: 设置源文件夹和备份文件夹
set "sourceFolder=C:\Steam\steamapps\common\PalServer\Pal\Saved\SaveGames\"
set "backupFolder=C:\Pal_BackUp"

:: 获取当前日期和时间，格式化为YYYYMMDDHHMMSS
for /f "tokens=2 delims==" %%a in ('wmic OS Get localdatetime /value') do set datetime=%%a
set "currentDate=!datetime:~0,8!!datetime:~8,6!"

:: 设置备份文件名
set "backupFile=%backupFolder%\Backup_!currentDate!.zip"

:: 确保备份文件夹存在
if not exist "%backupFolder%" mkdir "%backupFolder%"

:: 创建压缩文件
powershell -command "Compress-Archive -Path '%sourceFolder%' -DestinationPath '%backupFile%'"

:: 删除除最新五个备份以外的所有备份
for /f "skip=5 delims=" %%A in ('dir "%backupFolder%\*.zip" /b /a-d /o-d') do del "%backupFolder%\%%A"

:: 完成备份
echo Backup completed successfully.

endlocal
```

使用本脚本通过计划任务可实现

- 自动备份幻兽帕鲁服务器存档
- 自动以当前系统日期时间命名压缩包
- 只保留最近的五个存档

在这个位置您需要设置要备份的目录 `sourceFolder`以及存储备份的目录 `backupFolder`

```powershell
:: 设置源文件夹和备份文件夹
set "sourceFolder=C:\Steam\steamapps\common\PalServer\Pal\Saved\SaveGames\" ::这里改成要备份的目录
set "backupFolder=C:\Pal_BackUp" ::这里改成存储备份的目录
```

然后添加计划任务

以下是步骤：

1. **打开任务计划程序**:

   - 在 Windows 搜索框中输入“任务计划程序”，然后选择它以打开。
2. **创建任务**:

   - 在任务计划程序的右侧，点击“创建任务...”以打开任务属性窗口。
3. **设置常规信息**:

   - 输入任务的名称和描述。
   - 在“常规”标签下，选择“使用最高权限运行”。
4. **设置触发器**:

   - 切换到“触发器”标签，点击“新建...”。
   - 设置开始时间。
   - 在“任务设置”中，选择“按日”。
   - 然后勾选“重复任务间隔”，选择“30 分钟”，并在“持续时间”中选择“无限期”。
5. **设置操作**:

   - 切换到“操作”标签，点击“新建...”。
   - 在“操作”下拉菜单中选择“启动程序”。
   - 在“程序/脚本”栏中，浏览并选择您的 `BackUp.bat` 批处理文件。
6. **完成创建任务**:

   - 根据需要调整其他标签（如“条件”和“设置”）中的选项。
   - 点击“确定”完成任务的创建。
7. **测试任务**:

   - 找到您刚创建的任务，右键点击并选择“运行”来测试。

## 如果需要自动重启服务器并自启动PalServer

### 创建自动重启服务器的计划任务：

1. **打开任务计划程序**:

   - 在 Windows 搜索框中输入“任务计划程序”，然后选择它以打开。
2. **创建任务**:

   - 在任务计划程序的右侧，点击“创建任务...”。
3. **设置常规信息**:

   - 在“常规”标签下，输入任务的名称，例如“自动重启服务器”。
   - 选择“不管用户是否登录都要运行”。
   - 选择“使用最高权限运行”。
4. **配置触发器**:

   - 切换到“触发器”标签，点击“新建...”。
   - 选择“按日”执行。
   - 设置开始日期和每天的执行时间为 05:30。
5. **配置操作**:

   - 切换到“操作”标签，点击“新建...”。
   - 在“操作”下拉菜单中选择“启动程序”。
   - 在“程序/脚本”栏中输入 `shutdown`。
   - 在“添加参数 (可选)”栏中输入 `/r /f /t 0`。这些参数的意思是：`/r` - 重启计算机，`/f` - 强制关闭所有应用程序，`/t 0` - 在命令执行后立即重启。
6. **完成创建任务**:

   - 点击“确定”保存任务。

### 设置 `PalServer.exe` 为自动启动应用：

如果 `PalServer.exe` 不是作为服务运行的，您需要将其添加到 Windows 启动文件夹中以实现自动启动：

1. 按 Win + R 键，输入 `shell:startup`，然后按 Enter 打开启动文件夹。
2. 在启动文件夹中，右键点击空白区域，选择“新建” -> “快捷方式”。
3. 在弹出的窗口中输入 `PalServer.exe` 的完整路径，然后点击“下一步”和“完成”。

这样，每次 Windows 启动时，`PalServer.exe` 就会自动运行。

### 注意事项：

- 在生产环境中部署之前，请在一个安全的测试环境中验证这个计划任务。
- 自动重启服务器可能会影响正在运行的应用程序和服务，因此请确保这一操作不会对您的业务造成不利影响。
