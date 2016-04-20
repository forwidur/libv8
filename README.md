Static v8 artifacts
===

This repository contains statically-built
[libv8](https://developers.google.com/v8/) (version 4.8.271.17).

How to replicate this from source
---

Download [v8](https://github.com/v8/v8/wiki/Using%20Git).

### Linux

Build:

    make x64.release GYPFLAGS="-Dv8_use_external_startup_data=0 \
      -Dv8_enable_i18n_support=0 -Dv8_enable_gdbjit=0"`

If build system produces a thin archive, you want to make it into a fat one:

    for lib in `find out/x64.release/obj.target/tools/gyp/ -name '*.a'`;
      do ar -t $lib | xargs ar rvs $lib.new && mv -v $lib.new $lib;
    done`


Copy the libraries to the destination directory:

    cp -v out/x64.release/obj.target/tools/gyp/libv8_{base,libbase,external,libplatform}* \
      ${GO_V8}/libv8/

### Mac

To build:

    CXX="`which clang++` -std=c++11 -stdlib=libc++" \
    GYP_DEFINES="mac_deployment_target=10.10" \
    make x64.release GYPFLAGS="-Dv8_use_external_startup_data=0 \
      -Dv8_enable_i18n_support=0 -Dv8_enable_gdbjit=0"

Copy the libraries to the destination directory:

    cp -v out/x64.release/libv8_{base,libbase,external,libplatform}* \
      ${GO_V8}/libv8/x86_64-apple-darwin

Note: On MacOS, the resulting libraries contain debugging information by default
(even though we've built the release version). As a result, the binaries are 30x
larger, then they should be. Strip that with `strip -S out/x64.release/libv8*.a`
to reduce the size of the archives very significantly.

Godspeed!

License
---
Please refer to the [official v8 licensing information](https://github.com/v8/v8/blob/master/LICENSE).
