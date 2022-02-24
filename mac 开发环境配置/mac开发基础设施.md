# macOS 开发相关基础设施

这是本人第一次用 macOS，还是新的 m1 芯片，在安装基本的程序和平台时遇到许多问题。也踩了很多坑，一并在这里记录

 

## Xcode

类似 Windows 平台上的 Visual Studio，Xcode 是开发 mac 相关程序的 IDE ，它也包括一些常用的命令

如果不开发苹果家的应用，一般只用安装 command line tools for xcode 即可

坑：在app store 安装 xcode 13.1 后输入 `Xcode-select -p` 后显示路径 -----，但 `xcode-select -- install` 仍会自动跳转安装程序，并且显示因为网络问题安装失败

解决方法：去官网手动下载，使用 safari 打开https://developer.apple.com/download/all/ 用 edge 跳转至其他页面



## Homebrew

homebrew 是一个包管理器，可以在 mac 上获得类似 linux 的体验

安装 homebrew 前要先安装 command line tool

用官网脚本安装一直有网络错误出现在拉取 git 过程中，换用国人脚本后解决[Homebrew国内如何自动安装（国内地址） - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/111014448)



官方安装脚本关于安装 xcode 的内容

```shell

if should_install_command_line_tools && version_ge "${macos_version}" "10.13"
then
  ohai "Searching online for the Command Line Tools"
  # This temporary file prompts the 'softwareupdate' utility to list the Command Line Tools
  clt_placeholder="/tmp/.com.apple.dt.CommandLineTools.installondemand.in-progress"
  execute_sudo "${TOUCH[@]}" "${clt_placeholder}"

  clt_label_command="/usr/sbin/softwareupdate -l |
                      grep -B 1 -E 'Command Line Tools' |
                      awk -F'*' '/^ *\\*/ {print \$2}' |
                      sed -e 's/^ *Label: //' -e 's/^ *//' |
                      sort -V |
                      tail -n1"
  clt_label="$(chomp "$(/bin/bash -c "${clt_label_command}")")"

  if [[ -n "${clt_label}" ]]
  then
    ohai "Installing ${clt_label}"
    execute_sudo "/usr/sbin/softwareupdate" "-i" "${clt_label}"
    execute_sudo "/usr/bin/xcode-select" "--switch" "/Library/Developer/CommandLineTools"
  fi
  execute_sudo "/bin/rm" "-f" "${clt_placeholder}"
fi

# Headless install may have failed, so fallback to original 'xcode-select' method
if should_install_command_line_tools && test -t 0
then
  ohai "Installing the Command Line Tools (expect a GUI popup):"
  execute_sudo "/usr/bin/xcode-select" "--install"
  echo "Press any key when the installation has completed."
  getc
  execute_sudo "/usr/bin/xcode-select" "--switch" "/Library/Developer/CommandLineTools"
fi

```



