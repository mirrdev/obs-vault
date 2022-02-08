## Апаратное ускорение

Слушай после того как написал, решил повторно проверить со свежим nvidia-vaapi-driver, т.к. в него сейчас активно коммитят. С последней git версией заработал и этот момент.  
Если кому будет полезно, то короткий миниагайд:  
1) удалить libva-vdpau-driver  
2) установить последний nvidia-vaapi-driver с гита ([https://github.com/elFarto/nvidia-vaapi-driver).](https://github.com/elFarto/nvidia-vaapi-driver).) Пользователи Arch/Manjaro могут просто поставить пакет из AUR (nvidia-vaapi-driver-git)  
3) в /etc/environment добавляем  
LIBVA_DRIVER_NAME=nvidia  
MOZ_ENABLE_WAYLAND=1  
4) перезагружаемся  
5) проверяем что VA-API заработал  
# vainfo  
vainfo: VA-API version: 1.13 (libva 2.13.0)  
vainfo: Driver version: VA-API NVDEC driver  
vainfo: Supported profile and entrypoints  
 VAProfileMPEG2Simple            : VAEntrypointVLD  
 VAProfileMPEG2Main              : VAEntrypointVLD  
 VAProfileVC1Simple              : VAEntrypointVLD  
 VAProfileVC1Main                : VAEntrypointVLD  
 VAProfileVC1Advanced            : VAEntrypointVLD  
 <unknown profile>               : VAEntrypointVLD  
 VAProfileH264Main               : VAEntrypointVLD  
 VAProfileH264High               : VAEntrypointVLD  
 VAProfileH264ConstrainedBaseline: VAEntrypointVLD  
 VAProfileVP8Version0_3          : VAEntrypointVLD  
6) проверяем что завелось

для стандартного хромого все работает с такими аргументами:  
cat ~/.config/chromium-flags.conf  
--enable-features=VaapiVideoEncoder,VaapiVideoDecoder,CanvasOopRasterization  
--enable-zero-copy  
--use-gl=desktop  
--ignore-gpu-blocklist  
--enable-oop-rasterization  
--enable-raw-draw  
--enable-gpu-rasterization  
--use-vulkan  
--disable-gpu-sandbox  
--disable-reading-from-canvas  
--disable-sync-preferences