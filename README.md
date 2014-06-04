test
#前言
本專案在透過webmin建立系統工程各項作業之管理,透過webmin的客製化命令模組建立各項系統管理作業
其中包括
1. CMOD更新
*依客戶分別有不同作業,目前建立的客戶作業有FCB, LB等
2. CMOD HA手動切換
3. NIM管理
4. OCM管理
5. 常用AIX管理作業    

#aix7客製安裝
##系統初始安裝選項
    1. 系統設定
        *安裝方式           新安裝並完全覆蓋
        *安裝使用硬碟       hdisk0(預設值, 依實際需求修改)
    2. 主要語言環境設定(安裝後)
        *文化慣例           繁體中文
        *語言               繁體中文
        *鍵盤               繁體中文
        *鍵盤型態           Alternate
    3. <依預設值>
    4. <依預設值>
    5. 選擇版本             standard

##aix作業系統與軟體之同意授權

##確認並變更root的使用者限制
    1. 確認root的使用者限制,以root登入,執行指令
      ulimit -a
    2. 變更root的使用者限制,以root登入,執行指令
      chuser cpu='-1' fsize='-1' data='-1' stack='-1' core='-1' root
    
##變更paging space (建議值為記憶體兩倍)
    1. 確認paging space所在位置
      lsrsrc IBM.PagingDevice
    2. 確認paging space大小
      lsps -s
    3. 增加paging space大小 (以pp size為計算單位)
      chps -s'24' <LV Name> (預設為 hd6)
    4. 減少paging space大小 (以pp size為計算單位)
      chps -d'24' <LV Name> (預設為 hd6)
##變更檔案系統大小
    chfs -a size=1G /     (變更root檔案系統大小為1GB)
    chfs -a size=2G /tmp
    chfs -a size=2G /var
    chfs -a size=5G /opt
    chfs -a size=2G /home

##安裝X11.Dt
    1. use smit
        smitty install_boundle --> select device --> Select a Fileset Bundle "CDE" 
    2. 使用指令預覽安裝
        /usr/lib/instl/sm_inst installp_cmd -a -Q -d '/dev/cd0' -b 'CDE' -f 'all' -p -c -N -g -X -G -Y
    3. 使用指令安裝
        /usr/lib/instl/sm_inst installp_cmd -a -Q -d '/dev/cd0' -b 'CDE' -f 'all' -c -N -g -X -G -Y
##安裝openssh server
    1. use smit
        smitty install_boundle --> select device --> Select a Fileset Bundle "openssh_server" 
    2. 使用指令預覽安裝
        /usr/lib/instl/sm_inst installp_cmd -a -Q -d '/dev/cd0' -b 'openssh_server' -f 'all' -p -c -N -g -X -G -Y
    3. 使用指令安裝
        /usr/lib/instl/sm_inst installp_cmd -a -Q -d '/dev/cd0' -b 'openssh_server' -f 'all' -c -N -g -X -G -Y
##增加語言套件
    smitty lang --> Add Additional Language Environments --> 
        *CULTURAL convention to install (select UTF-8      Chinese (Traditional UTF) [ZH_TW],big5 T-Chinese (big5) [Zh_TW]) 
        *LANGUAGE translation to install (UTF-8      Chinese (Traditional) [ZH_TW], IBM-eucTW  Chinese (Traditional) [zh_TW] ,big5       T-Chinese (big5) [Zh_TW])
        *INPUT device/directory for software    /dev/cd0
        *EXTEND file systems if space needed?   yes
##安裝perzl org tools
###安裝gcc(include bash)
>rpm -ivh bash-4.2-12.aix5.1.ppc.rpm \
>         ettext-0.10.40-8.aix5.2.ppc.rpm \
>         info-5.1-1.aix5.1.ppc.rpm \
>         gcc-4.8.2-1.aix7.1.ppc.rpm \
>         gcc-cpp-4.8.2-1.aix7.1.ppc.rpm \
>         gmp-5.0.5-1.aix5.1.ppc.rpm \
>         libgcc-4.8.2-1.aix7.1.ppc.rpm \
>         libiconv-1.14-2.aix5.1.ppc.rpm \
>         libmpc-1.0.1-2.aix5.1.ppc.rpm \
>         libstdc++-4.8.2-1.aix7.1.ppc.rpm \
>         libstdc++-devel-4.8.2-1.aix7.1.ppc.rpm \
>         mpfr-3.1.2-1.aix5.1.ppc.rpm
###安裝tar
>cd ../tar-1.27.1-1/
>rpm -ivh tar-1.27.1-1.aix5.1.ppc.rpm
###安裝tightvnc
>cd ../tightvnc-server-1.3.10-2
>rpm -ivh libjpeg-8d-1.aix5.1.ppc.rpm \
>         tightvnc-server-1.3.10-2.aix5.1.ppc.rpm \
>         zlib-1.2.3-4.aix5.2.ppc.rpm
*設定vncserver --> 執行指令 vncserver
###安裝wget (include perl)
>cd ../wget-1.15-1
>rpm -iUh zlib-1.2.8-1.aix5.1.ppc.rpm \
>         info-5.1-2.aix5.1.ppc.rpm
>rpm -ivh bzip2-1.0.6-1.aix5.1.ppc.rpm \
>         bzip2-devel-1.0.6-1.aix5.1.ppc.rpm \
>         dbm-1.10-1.aix5.1.ppc.rpm \
>         libidn-1.28-1.aix5.1.ppc.rpm \
>         openssl-1.0.1f-1.aix5.1.ppc.rpm \
>         openssl-devel-1.0.1f-1.aix5.1.ppc.rpm \
>         openssl-doc-1.0.1f-1.aix5.1.ppc.rpm \
>         pcre-8.34-1.aix5.1.ppc.rpm \
>         pcre-devel-8.34-1.aix5.1.ppc.rpm \
>         perl-5.8.8-2.aix5.1.ppc.rpm \
>         readline-6.2-5.aix5.1.ppc.rpm \
>         readline-devel-6.2-5.aix5.1.ppc.rpm \
>         wget-1.15-1.aix5.1.ppc.rpm
###安裝python(include sqlite)
>cd ../python-2.6.8-1/
>rpm -ivh db4-4.7.25-2.aix5.1.ppc.rpm \
>         expat-2.1.0-1.aix5.1.ppc.rpm \
>         fontconfig-2.8.0-2.aix5.1.ppc.rpm \
>         freetype2-2.5.0-1.aix5.1.ppc.rpm \
>         libXft-2.3.1-1.aix5.1.ppc.rpm \
>         libXrender-0.9.7-2.aix6.1.ppc.rpm \
>         libffi-3.0.13-1.aix5.1.ppc.rpm  \
>         libpng-1.6.9-1.aix5.1.ppc.rpm \
>         sqlite-3.7.17-1.aix5.1.ppc.rpm \
>         tcl-8.5.14-1.aix5.1.ppc.rpm \
>         tk-8.5.14-1.aix5.1.ppc.rpm \
>         python-2.6.8-1.aix6.1.ppc.rpm \
>         python-libs-2.6.8-1.aix6.1.ppc.rpm
###安裝vim
>cd ../vim-7.4.052-1/
>rpm -ivh vim-common-7.4.052-1.aix5.1.ppc.rpm \
>         vim-enhanced-7.4.052-1.aix5.1.ppc.rpm
###安裝unzip
>cd ../unzip/
>rpm -ivh unzip-64bit-6.0-3.aix5.1.ppc.rpm
###安裝git (include openldap, curl)
>cd ../git-1.8.5.4-1
>rpm -ivh libssh2-1.4.3-2.aix5.1.ppc.rpm \
>         openldap-2.4.23-0.3.aix5.1.ppc.rpm \
>         popt-1.7-2.aix5.1.ppc.rpm \
>         curl-7.27.0-1.aix5.1.ppc.rpm \
>         less-458-1.aix5.1.ppc.rpm \
>         rsync-3.1.0-1.aix5.1.ppc.rpm \
>         git-1.8.5.4-1.aix5.1.ppc.rpm \
>         git-daemon-1.8.5.4-1.aix5.1.ppc.rpm
###config git
*get Certification file
 curl http://curl.haxx.se/ca/cacert.pem -o /var/ssl/certs/cacert.pem
*config system use cacert.pem
git config --system http.sslcainfo /var/ssl/certs/cacert.pem
##copy profiles (.profile , .bashrc , .vimrc , .vnc/xstartup)
cd <path to profiles>
cp -pR .* /
