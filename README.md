# Taming-R
记录一系列在使用R的过程中遇到的问题及解决方法

- [更换在ubuntu系统上更换Rstudio初始化调用的R](#更改调用的R)

## 更改调用的R

在linux桌面系统ubuntu希望更改**Rstudio**调用的R，在尽量不直接修改`/usr/local/bin/R`的基础上，可以使用**miniconda**或者**micromamba**先新建R环境，配置好了以后进行更改。

基本可以总结几种更改的方法：

1. 在**\.bashrc**文件添加 `export RSTUDIO_WHICH_R="/home/bio/micromamba/envs/R440/bin/R"`；
2. 找到**`~/.config/rstudio/rstudio-prefs.json`**文件并添加`{"r-bin": "/home/bio/micromamba/envs/R440/bin/R"}`；
3. 直接修改默认的R软链接`sudo ln -sf /home/bio/micromamba/envs/R440/bin/R /usr/local/bin/R`。**（不建议）**

理论上更改完就生效了，但是在更改过程中发现了一个有趣的事情，如果在终端使用`rstudio`启动Rstudio，得到正确R版本，如果是点击桌面图标，发现还是原版本。
最终发现是存在桌面应用的缓存问题，应该是类似于桌面应用也需要一个`source .bashrc`的操作用以更新，最终通过打开**`/usr/share/applications/rstudio.desktop`**这个文件，并修改了Exec

```
[Desktop Entry]
#Exec=/usr/lib/rstudio/rstudio %F
Exec=env RSTUDIO_WHICH_R=/home/bio/micromamba/envs/R440/bin/R rstudio %F
Icon=rstudio
Type=Application
Terminal=false
Name=RStudio
Categories=Development;IDE;
MimeType=text/x-r-source;text/x-r;text/x-R;text/x-r-doc;text/x-r-sweave;text/x-quarto-markdown;text/x-r-markdown;text/x-r-html;text/x-r-presentation;application/x-r-data;application/x-r-project;application/x-rdp-rsp;text/x-r-history;text/x-r-profile;text/x-tex;text/x-markdown;text/css;text/javascript;text/x-chdr;text/x-csrc;text/x-c++hdr;text/x-c++src;
```

修改完以后`sudo update-desktop-database`,这样点击图标的时候也成功更换了**Rstudio**对应的**R**版本
