# GATE-Installation-Guide

# 運行環境： ` openSUSE Tumbleweed `

# 一、準備工作：

  1. 編譯環境：

  - ` gcc ` 4.8 to 7.3
  - ` cmake ` 3.3 to last version
  - ` root ` 6.00 to last version

  2. 源代碼下載：

  - <del>[CLHEP](http://proj-clhep.web.cern.ch/proj-clhep/DISTRIBUTION/tarFiles/clhep-2.3.4.3.tgz)</del>（geant4 已自帶CLHEP）
  - [geant4](http://geant4-data.web.cern.ch/geant4-data/releases/geant4.10.05.p01.tar.gz)
  - [gate](http://www.opengatecollaboration.org/sites/default/files/Gate-8.2.tar.gz)
  - <del>[lmf](http://www.opengatecollaboration.org/sites/default/files/lmf_v3_0.tar_2.gz)</del> (No more Optional Required)




# <del>二、安裝 [CLHEP](http://proj-clhep.web.cern.ch/proj-clhep/clhep23.html)：A Class library for High Energy Physics</del>

  1. Download the source：[clhep-2.3.4.3.tgz](http://proj-clhep.web.cern.ch/proj-clhep/DISTRIBUTION/tarFiles/clhep-2.3.4.3.tgz);
  2. 執行以下命令：

```
 tar -zxvf clhep-2.3.4.3.tgz
 mkdir clhep_build
 cd clhep_build
 ccmake -DCMAKE_INSTALL_PREFIX=/usr/local/CLHEP/ ../2.3.4.3/CLHEP/
 make -j2
 sudo make install
```

![](1.png)




# 三、geant4 10.05的安裝

  1. Download the source：[geant4 10.05](http://geant4-data.web.cern.ch/geant4-data/releases/geant4.10.05.p01.tar.gz)
  2. Dependencies： 

 - <del>` gdml `</del>
 - ` xerces-c `
 - ` libxerces-c-devel `
 - ` libqt4-devel ` ` libqt4-devel-doc `
 - ` libG4OpenGL ` ` libQt5OpenGL-devel `
 - ` libQt5PrintSupport-devel `
 - ` libXmu-devel `
 - <del>` gccxml `</del>

  3. install:

``` bash
    tar -xzf geant4.10.05.p01.tar.gz
    cd /home/geant4.10.05/
    mkdir build
    cd build
    ccmake ../
    # Build Options
    make -j4
    sudo make install
```

## Build Options of Geant4

![](2.png)
配置說明：

  1. 不能選擇 GEANT4_BUILD_MULTITHREADED，Gate 不支持多線程的 Geant4.
  2. GEANT4_INSTALL_DATADIR 指定data文件的位置，可以是CMAKE_INSTALL_DATA的相对位置
  3. GEANT4_INSTALL_DATA 设置为ON时会从internet自动下载data
  4. GEANT4_USE_SYSTEM_CLHEP 设置为ON时会使用外部的CLHEP；CLHEP_ROOT_DIR变量有值时自动为ON
     CLHEP_ROOT_DIR CLHEP的安装位置


### geant4 相關插件：

```
http://geant4.cern.ch/support/source/G4NDL.4.5.tar.gz
http://geant4.cern.ch/support/source/G4EMLOW.7.7.tar.gz
http://geant4.cern.ch/support/source/G4PhotonEvaporation.5.3.tar.gz
http://geant4.cern.ch/support/source/G4RadioactiveDecay.5.3.tar.gz
http://geant4.cern.ch/support/source/G4NEUTRONXS.1.4.tar.gz
http://geant4.cern.ch/support/source/G4PII.1.3.tar.gz
http://geant4.cern.ch/support/source/G4RealSurface.2.1.1.tar.gz
http://geant4.cern.ch/support/source/G4SAIDDATA.2.0.tar.gz
http://geant4.cern.ch/support/source/G4ABLA.3.1.tar.gz
http://geant4.cern.ch/support/source/G4ENSDFSTATE.2.2.tar.gz
sudo mkdir /usr/local/share/Geant4-10.5.1/
sudo mkdir /usr/local/share/Geant4-10.5.1/data/
for tar in *.tar.gz; do sudo tar xvf $tar -C /usr/local/share/Geant4-10.5.1/data/; done
```

## geant4 的[圖形引擎](http://geant4.cern.ch/UserDocumentation/UsersGuides/ForApplicationDeveloper)：

Geant4默认会安装这一些图形引擎：DAWNFILE, HepRepFile, HepRepXML, RayTracer, VRML1FILE, VRML2FILE and ATree and GAGTree.

http://blog.renren.com/blog/bp/Q7K9U584qP 里面提到OpenGL引擎有四种运行方式 ` openGLX, openGLSX, openGLXm, openGLSXm`


## 安裝examples：

     cd /usr/local/share/Geant4-10.5.1/examples/
     sudo mkdir build
     sudo ccmake ../
     sudo make -j4

在 `build` 文件夾中執行這些範例
例如運行

```
 ./exampleB1
```

編譯的例子中，`~/geant4.10.03/examples/extended/` 下的 ` /persistency/ ` 三個例子，編譯出錯，暫時無法解決。將 ` ~/geant4.10.03/examples/extended/CMakeLists.txt ` 中 ` persistency ` 項註釋。

``` 
 # add_subdirectory(persistency)
```

運行例子時若出現：

```
 libGL error: failed to load driver: swrast
```

這是顯卡驅動造成的，需要安裝 `Mesa-32bit`，若安裝完成後仍出現錯誤，執行

```
 sudo mkdir /usr/lib64/dri/updates/
 ln -s /usr/lib64/dri/swrast_dri.so /usr/lib64/dri/updates/swrast_dri.so
```


# <del>四、lmf</del>

 - 修改 ./lmf_v3.0/makefile:

```
PATHLMFCOMMON = /opt/lmf
install : 
	-mkdir $(PATHLMFCOMMON)
	-chmod -R 700 $(PATHLMFCOMMON)
	-mkdir $(PATHLMFCOMMON)/lib
	-mkdir $(PATHLMFCOMMON)/includes
	-rm -rf $(PATHLMFCOMMON)/$(LIB)
	-rm -rf $(PATHLMFCOMMON)/includes/*	
	-cp $(LIB) $(PATHLMFCOMMON)/$(LIB)
	-cp includes/* $(PATHLMFCOMMON)/includes/
	-chmod -R 555 $(PATHLMFCOMMON)
```

 - 修改 ./lmf_v3.0/examples/makefile:

```
EXE_04 : exempleMain_04.o
	gcc -o $@ $^ $(LDFLAGS) -lstdc++
	@echo "example 4 : EXE_04 ok..."
```

```
tar -xzf lmf_v3_0.tar.gz
cd ./lmf_v3.0/
./configure
make -j2
sudo make install
cd ./example
make
```

# 五、ITK && RTK && root6

```
sudo zypper in valgrind
tar -xzf InsightToolkit-4.13.0.tar.gz 
cd InsightToolkit-4.13.0 
mkdir build && cd build
ccmake ../
#  Notice the version of GCC.
make -j4
sudo make install
cd ../..
```

![title](3.png)

```
sudo zypper in gengetopt
unzip RTK-1.4.0.zip
cd RTK-1.4.0
mkdir build && cd build
ccmake ../
make -j4
sudo make install
```

![title](4.png)

```
wget https://root.cern.ch/download/root_v6.14.04.source.tar.gz
tar -xzf root_v6.14.04.source.tar.gz
cd ./root/build
ccmake ../
make -j4
sudo make install

```



# 六、GATE 8.2安裝

  1. Dependencies：
     - ` gcc ` 4.8 to 7.3 (Note: with gcc 6.* and newer, ROOT 5 does not compile!)
     - ` cmake ` 3.3 to last version (with SSL support)
     - ` root ` 6.00 to last version
     - ` geant4 ` 10.05
     - sudo zypper in libqt4-devel libqt4-devel-doc 
     - sudo zypper in libG4OpenGL libQt5OpenGL-devel libQt5PrintSupport-devel gccxml
     - lmf 3.0
     - RTK 1.4 (Optional dependencies)
  2. install:

``` bash
    tar -xzf Gate-8.2.tar.gz -C /home/qingyu
    cd Gate-8.2/
    mkdir build
    cd build/
    ccmake ../  # press `C` to configure 
    # CMAKE_INSTALL_PREFIX = ` /usr/local/ `
    # press `g ` to gernerate the makefile.
    make -j4
    sudo make install
```

![](5.png)
![](6.png)

3. Last step, update your environment variables file with the following command lines(bash or zsh): Gate/Root/Geant4

```
    export PATH=$PATH:/usr/local/bin
    source ${ROOTSYS}/bin/thisroot.sh
    export LMF_HOME=/opt/lmf
    export LD_LIBRARY_PATH=/usr/local/lib64
    source /usr/local/bin/geant4.sh
```

NOTE:

 - 執行 gate 的 `make` 時，會出現下載 `data_Voxel_Phantom.root` 的錯誤，這種情況多嘗試幾次，直到下載成功。
 - 編譯不通過，顯示某些文件未下載成功，解決方法是手動下載該文件至相應目錄：

```
Generating /home/qingyu1/gate_v8.0/examples/example_ROOT_Analyse/data_PET.root
Generating /home/qingyu1/gate_v8.0/examples/example_ROOT_Analyse/data_Voxel_Phantom.root
Generating /home/qingyu1/gate_v8.0/examples/example_dosimetry/brachytherapy/data/ct-2mm.raw
Generating /home/qingyu1/gate_v8.0/examples/example_dosimetry/protontherapy/data/patient-2mm.raw

"http://midas3.kitware.com/midas/api/rest?method=midas.bitstream.download&checksum=a44e7bc7c3f2690a9e94e4ef4c5d3f1a&algorithm=MD5"
"http://midas3.kitware.com/midas/api/rest?method=midas.bitstream.download&checksum=9c0989256d3b6e9b213359cd4a3b6daa&algorithm=MD5"
"http://midas3.kitware.com/midas/api/rest?method=midas.bitstream.download&checksum=83d961494d5b44d76fccb1922012897f&algorithm=MD5"
"http://midas3.kitware.com/midas/api/rest?method=midas.bitstream.download&checksum=baf4a6c7958099ca39965928f010e7c6&algorithm=MD5"
```



# 六、`.bashrc` or `.zshrc` 文件配置

```
# Geant4 10.4.2 & Gate_v8.1
export PATH=$PATH:/usr/local/bin
#export LMF_HOME=/opt/lmf
export LD_LIBRARY_PATH=/usr/local/lib
#source /usr/local/bin/geant4.sh
#-----------------------------------------------------------------------
# Resource file paths
# - Datasets
export G4ENSDFSTATEDATA=/usr/local/share/Geant4-10.4.2/data/G4ENSDFSTATE2.2
export G4NEUTRONHPDATA=/usr/local/share/Geant4-10.4.2/data/G4NDL4.5 
export G4LEDATA=/usr/local/share/Geant4-10.4.2/data/G4EMLOW7.3
export G4LEVELGAMMADATA=/usr/local/share/Geant4-10.4.2/data/PhotonEvaporation4.3
export G4RADIOACTIVEDATA=/usr/local/share/Geant4-10.4.2/data/RadioactiveDecay5.2
export G4NEUTRONXSDATA=/usr/local/share/Geant4-10.4.2/data/G4NEUTRONXS1.4 
export G4PIIDATA=/usr/local/share/Geant4-10.4.2/data/G4PII1.3 
export G4REALSURFACEDATA=/usr/local/share/Geant4-10.4.2/data/RealSurface2.1.1
export G4SAIDXSDATA=/usr/local/share/Geant4-10.4.2/data/G4SAIDDATA1.1 
export G4ABLADATA=/usr/local/share/Geant4-10.4.2/data/G4ABLA3.1
```

注意在 GreanWall 電腦上加入 `source /usr/local/bin/geant4.sh` 這句，會導致 `kde plasma` 桌面黑屏，在安裝 teamviewer 的系統中表現爲開機桌面上儘可見 teamviewer，通過teamviewer可調用firefox，也可連接控制其他電腦，但桌面環境未正常加載。但不加這句話，`Gate`運行時會提示

```
        [G4-cerr] 
-------- EEEE ------- G4Exception-START -------- EEEE -------
*** G4Exception : PART70000
      issued by : G4NuclideTable
G4ENSDFSTATEDATA environment variable must be set
*** Fatal Exception *** core dump ***
-------- EEEE -------- G4Exception-END --------- EEEE -------

        [G4-cerr] 
        [G4-cerr] *** G4Exception: Aborting execution ***
Aborted (core dumped)
```

或者是提示找不到 `libG4Tree.so`。
解決方法是在`.bashrc` or `.zshrc`中加入

 > export G4ENSDFSTATEDATA=/usr/local/share/Geant4-10.2.3/data/G4ENSDFSTATE1.2.3
 > #新版本的 `Geant4` 不需要在加入其他變量信息！！！
 > #Linux 桌面崩潰無法進入時，須考慮是否由用戶配置文件導致，可登錄新用戶查看能否正常進入桌面！！！
