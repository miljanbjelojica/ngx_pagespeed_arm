# ngx_pagespeed for aarch64 and armv7l

This repo contains PSOL binaries (v1.15.0.0-8809) required to get ngx_pagespeed running on aarch64/armv7l and the patches that produced them.

Note that libwebp and libpng were built without the NEON code-paths as I could not get them to compile.

I have only tested a handful of filters and give no guarantees.

However, if you do experience issues that are clearly ARM specific, please let me know.

For any other questions, refer to the official documentation:

https://www.modpagespeed.com/doc/

https://www.ngxpagespeed.com/

---

### To build a dynamic ngx_pagespeed.so module for NGINX v1.18.0 on aarch64

```
git clone https://github.com/apache/incubator-pagespeed-ngx.git
cd incubator-pagespeed-ngx
wget https://gitlab.com/gusco/ngx_pagespeed_arm/-/raw/master/psol-1.15.0.0-aarch64.tar.gz
tar xvf psol-1.15.0.0-aarch64.tar.gz
sed -i 's/x86_64/aarch64/' config
sed -i 's/x64/aarch64/' config
cd ..

wget http://nginx.org/download/nginx-1.18.0.tar.gz
tar xvf nginx-1.18.0.tar.gz
cd nginx-1.18.0
./configure --add-dynamic-module=../incubator-pagespeed-ngx --with-compat
make
```
Then copy the module `objs/ngx_pagespeed.so` to the right place for your NGINX modules.

---

### To build a dynamic ngx_pagespeed.so module for NGINX v1.18.0 on armv7l

```
git clone https://github.com/apache/incubator-pagespeed-ngx.git
cd incubator-pagespeed-ngx
wget https://gitlab.com/gusco/ngx_pagespeed_arm/-/raw/master/psol-1.15.0.0-armv7l.tar.gz
tar xvf psol-1.15.0.0-aarch64.tar.gz
sed -i 's/x86_64/armv7l/' config
sed -i 's/x64/armv7l/' config
cd ..

wget http://nginx.org/download/nginx-1.18.0.tar.gz
tar xvf nginx-1.18.0.tar.gz
cd nginx-1.18.0
./configure --add-dynamic-module=../incubator-pagespeed-ngx --with-compat
make
```
Then copy the module `objs/ngx_pagespeed.so` to the right place for your NGINX modules.

---

### The PSOL binaries were built with the following procedure

```
wget https://gitlab.com/gusco/ngx_pagespeed_arm/-/raw/master/incubator-pagespeed-mod-aarch64.patch
  or
wget https://gitlab.com/gusco/ngx_pagespeed_arm/-/raw/master/incubator-pagespeed-mod-armv7l.patch
git clone --recursive https://github.com/apache/incubator-pagespeed-mod.git
cd incubator-pagespeed-mod
patch -Np1 -i ../incubator-pagespeed-mod-aarch64.patch
  or
patch -Np1 -i ../incubator-pagespeed-mod-armv7l.patch
install/build_psol.sh --skip_tests --skip_deps
```