# sm64ex Android Port
This is a port of the reconstructed Super Mario 64 source code to Android using SDL2 with OpenGL ES 2.0.

It has cross-platform Touch Controls, Audio works, it saves the game to the app's internal storage and you can play it with an external keyboard or controller as well (tested on PS3 controller).

# Build instructions

## Linux
0. Clone the repo with `git clone https://github.com/VDavid003/sm64-port-android-base.git --branch sm64ex` and init submodules with `git submodule init && git submodule update`.
1. Make sure you can compile the non-Android PC port and copy the base ROM to app/jni/src like you'd normally do.
2. Build the PC version inside app/jni/src. To do this just `pushd app/jni/src` and `make`. If you will use custom options you should probably use them here too like you'd do on normal sm64ex!
3. `make` again. This is important, since the ex fork has a problem where it doesn't build some stuff on the first build.
4. After going back to the root folder (`popd`), get SDL with `./getSDL.sh`.
5. Configure the options at the top for your liking in app/jni/src/Android.mk
6. Build (and install) using gradle (`./gradlew installDebug`)
7. If you used `EXTERNAL_DATA`, follow the instructions for it below!

## Windows
Coming soon(tm)

## Docker

**Clone the repository:**
```sh
git clone --recursive https://github.com/VDavid003/sm64-port-android-base
```

**Create the build image:**
```sh
# navigate into newly cloned repo
cd sm64-port-android-base
# build the docker image
docker build . -t sm64_android
```
**Copy in your baserom:**
```sh
cp /path/to/your/baserom.z64 ./app/jni/src/baserom.us.z64
```

**Setup symlinks for SDL:**
```sh
docker run --rm -v $(pwd):/sm64 sm64_android sh -c "ln -nsf /SDL2-2.0.12/src /sm64/app/jni/SDL/src"
docker run --rm -v $(pwd):/sm64 sm64_android sh -c "ln -nsf /SDL2-2.0.12/include /sm64/app/jni/SDL/include"
```

**Perform native build twice:**
```sh
# if you have more cores available, you can increase the --jobs parameter
docker run --rm -v $(pwd):/sm64 sm64_android sh -c "cd /sm64/app/jni/src && make --jobs 4"
docker run --rm -v $(pwd):/sm64 sm64_android sh -c "cd /sm64/app/jni/src && make --jobs 4"
```

**Perform Android build:**
```sh
docker run --rm -v $(pwd):/sm64 sm64_android sh -c "./gradlew assembleDebug"
```

**Enjoy your apk:**
```sh
ls -al ./app/build/outputs/apk/debug/app-debug.apk
```

# EXTERNAL_DATA option
If you use `EXTERNAL_DATA`, you'll find a zip named `base.zip` in `app/jni/src/build/<version>_pc/res`.

You should take this zip and put it in `Internal Storage/Android/data/com.vdavid003.sm64port/files`
