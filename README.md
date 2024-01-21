# Nuxt Image bug reproduction repository

I am running into a problem using NuxtPicture over NuxtImg.

## NuxtImg - static generation of images works

When using NuxtImg and generating the images statically using `nuxt generate` works well:

```vue
<NuxtImg src="ladder-light.jpg" width="300" />
```

produces the following output

```
# ...
ℹ Prerendering 3 initial routes with crawler                                                                                                                                                                                                                  nitro 21:03:23
  ├─ /200.html (56ms)                                                                                                                                                                                                                                          nitro 21:03:23
  ├─ /404.html (57ms)                                                                                                                                                                                                                                          nitro 21:03:23
  ├─ / (60ms)                                                                                                                                                                                                                                                  nitro 21:03:23  
  ├─ /_payload.json (5ms)                                                                                                                                                                                                                                      nitro 21:03:23
  ├─ /_ipx/w_300/ladder-light.jpg (121ms)                                                                                                                                                                                                                      nitro 21:03:24
  ├─ /_ipx/w_600/ladder-light.jpg (174ms)                                                                                                                                                                                                           nitro 21:06:16
✔ Generated public .output/public 
```
## NuxtPicture - static generation of images is flawed

In contrast to this, using NuxtPicture and generating the images statically does not respect the desired width for the image and instead generates all possible size/format/fit combinations:

```vue
<NuxtPicture src="/ladder-light.jpg" width="300" />
```

produces the following output

```
# ...
ℹ Prerendering 3 initial routes with crawler                                                                                                                                                                                                                  nitro 21:06:13
  ├─ / (65ms)                                                                                                                                                                                                                                                  nitro 21:06:13
  ├─ /200.html (61ms)                                                                                                                                                                                                                                          nitro 21:06:13
  ├─ /404.html (62ms)                                                                                                                                                                                                                                          nitro 21:06:13
  ├─ /_payload.json (10ms)                                                                                                                                                                                                                                     nitro 21:06:13
  ├─ /_ipx/w_320&f_webp/ladder-light.jpg (227ms)                                                                                                                                                                                                               nitro 21:06:16
  ├─ /_ipx/w_640&f_webp/ladder-light.jpg (365ms)                                                                                                                                                                                                               nitro 21:06:16
  ├─ /_ipx/w_1024&f_webp/ladder-light.jpg (696ms)                                                                                                                                                                                                              nitro 21:06:16  
  ├─ /_ipx/w_768&f_webp/ladder-light.jpg (1155ms)                                                                                                                                                                                                              nitro 21:06:16  
  ├─ /_ipx/w_320&f_jpeg/ladder-light.jpg (1509ms)                                                                                                                                                                                                              nitro 21:06:16  
  ├─ /_ipx/w_640&f_jpeg/ladder-light.jpg (1651ms)                                                                                                                                                                                                              nitro 21:06:16  
  ├─ /_ipx/w_768&f_jpeg/ladder-light.jpg (1969ms)                                                                                                                                                                                                              nitro 21:06:16  
  ├─ /_ipx/w_1280&f_webp/ladder-light.jpg (791ms)                                                                                                                                                                                                              nitro 21:06:16  
  ├─ /_ipx/w_1280&f_jpeg/ladder-light.jpg (1518ms)                                                                                                                                                                                                             nitro 21:06:16  
  ├─ /_ipx/w_1536&f_webp/ladder-light.jpg (1583ms)                                                                                                                                                                                                             nitro 21:06:16  
  ├─ /_ipx/w_1536&f_jpeg/ladder-light.jpg (1903ms)                                                                                                                                                                                                             nitro 21:06:16  
  ├─ /_ipx/w_3072&f_webp/ladder-light.jpg (1330ms)                                                                                                                                                                                                             nitro 21:06:16  
  ├─ /_ipx/w_2560&f_webp/ladder-light.jpg (1426ms)                                                                                                                                                                                                             nitro 21:06:16  
  ├─ /_ipx/w_2048&f_jpeg/ladder-light.jpg (2027ms)                                                                                                                                                                                                             nitro 21:06:16  
  ├─ /_ipx/w_3072&f_jpeg/ladder-light.jpg (2218ms)                                                                                                                                                                                                             nitro 21:06:16  
  ├─ /_ipx/w_2560&f_jpeg/ladder-light.jpg (2346ms)                                                                                                                                                                                                             nitro 21:06:16
  ├─ /_ipx/w_1024&f_jpeg/ladder-light.jpg (2354ms)                                                                                                                                                                                                             nitro 21:06:16
  ├─ /_ipx/w_2048&f_webp/ladder-light.jpg (2433ms)                                                                                                                                                                                                             nitro 21:06:16
✔ Generated public .output/public    
```

NuxtImg generates 2 images vs NuxtPicture's 18. 

As the sizes outside the desired dimensions set on NuxtImg/NuxtPicture are irrelevant 16 irrelevant images were generated in this example.

This quickly gets out of hand with multiple images and inflates the size of the output.

# Reproduction steps

To reproduce edit the [app.vue](app.vue) file, toggle the comment on either NuxtImg or NuxtPicture and run 'nuxt generate'.