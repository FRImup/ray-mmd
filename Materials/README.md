Material
========
This document is designed to help those who wanted to quickly get up to speed in Ray-MMD

ALBEDO:
------
　　`Albedo` is also called `base color`, defines the overall color of the material and clamped between `0.0` and `1.0`

* ##### ALBEDO_MAP_FROM  
    You can use a `linear-color` and `texture` to change the colors in your model by set the `code` to the `ALBEDO_MAP_FROM`.

    0 . Parameter fetch from `linear-color` from the `const float3 albedo = 1.0`.  
    1 . You can use a `srgb-image` (bmp, png, jpg, tga, dds) by enter a relative and absolutely path to the `ALBEDO_MAP_FILE`.  
    2 . You can use a `animation srgb-image` (gif, apng) by enter a relative and absolutely path to the `ALBEDO_MAP_FILE`.  
    3 . Parameter fetch from `Texture` from the `pmx`.  
    4 . Parameter fetch from `Sphere map` from the `pmx`.  
    5 . Parameter fetch from `Toon map` from the `pmx`.  
    6 . Parameter fetch from `avi` or `screen` from the `DummyScreen.x` inside `extension` folder.  
    7 . Parameter fetch from `Ambient Color` from the `pmx`.  
    8 . Parameter fetch from `Specular Color` from the `pmx`.  
    9 . Parameter fetch from `Specular Power` from the `pmx`. // `Only for smoothness`  

* ##### ALBEDO_MAP_UV_FLIP
    You can flip your texture for the `X` and `Y` axis mirror by set `code` to the `ALBEDO_MAP_UV_FLIP`

    1 . Flip axis `X`  
    2 . Flip axis `Y`  
    3 . Flip axis `X` and `Y`  

* ##### ALBEDO_MAP_APPLY_SCALE  
    You can apply the color from `albedo` to change the colors in your texture by set `code` to the `ALBEDO_MAP_APPLY_SCALE`  

    1 . map values * albedo  
    2 . map values ^ albedo  

* ##### ALBEDO_MAP_APPLY_DIFFUSE  
    Texture colors to multiply with `diffuse` from the `PMX`.

* ##### ALBEDO_MAP_APPLY_MORPH_COLOR  
    Texture colors to multiply with color from the `morph` controller (`R+`/`G+`/`B+`).  

* ##### ALBEDO_MAP_FILE  
    If `ALBEDO_MAP_FROM` is `1` or `2`, you will need to enter the path to the texture resource.   

    ##### For example :
    ###### If the xxx.png and material.fx is inside same folder
    * `You can set the xxx.png to the ALBEDO_MAP_FILE like : #define ALBEDO_MAP_FILE "xxx.png"`
    ###### If the xxx.png is inside parent path of the material.fx
    * `You can set the xxx.png to the ALBEDO_MAP_FILE like : #define ALBEDO_MAP_FILE "../xxx.png"`
    ###### If the xxx.png is inside other path from parent path of the material.fx
    * `You can set the xxx.png to the ALBEDO_MAP_FILE like : #define ALBEDO_MAP_FILE "../other path/xxx.png"`
    ###### If the xxx.png is inside your desktop or other disk
    * `You can set the xxx.png to the ALBEDO_MAP_FILE like : #define ALBEDO_MAP_FILE "C:/Users/User Name/Desktop/xxx.png"`

    ##### Tips:
    * Using "../" instead of parent folder
    * Change all "\\" to "/".

* ##### const float3 albedo = 1.0;  
    If `ALBEDO_MAP_FROM` is `0` or `ALBEDO_MAP_APPLY_SCALE` is `1`, you will need to set color to the `albedo`.
    
    ##### For example :
    ##### If the red is normalized value, it can be set to albedo like:
    * `const float3 albedo = float3(1.0, 0.0, 0.0);`
    ##### If the red is unnormalized value, it can be set to albedo like:
    * `const float3 albedo = float3(255, 0.0, 0.0) / 255.0;`
    ##### If the color is fetched from your monitor, you need to convert the color from sRGB to linear color-space by color ^ gamma
    * Convert the `srgb color-space` from normalized value to `linear color-space` like:
      * `const float3 albedo = pow(float3(r, g, b), 2.2);`
    * Convert the `srgb color-space` from unnormalized value to `linear color-space` like:
      * `const float3 albedo = pow(float3(r, g, b) / 255.0, 2.2);`

* #### const float2 albedoMapLoopNum = 1.0;
    You can `tile` your texture for the `X` and `Y` axis separately by change `albedoMapLoopNum = float2(x, y)`

SubAlbedo:
--------------
* ##### ALBEDO_SUB_ENABLE
    You can apply second value for `base color` change by set `ALBEDO_SUB_ENABLE`

    0 . None  
    1 . albedo * albedoSub  
    2 . albedo ^ albedoSub  
    3 . albedo + albedoSub  
    4 . melanin  
    5 . Alpha Blend  

* ##### ALBEDO_SUB_MAP_FROM
    see `ALBEDO_MAP_FROM`
* ##### ALBEDO_SUB_MAP_UV_FLIP
    see `ALBEDO_MAP_UV_FLIP`
* ##### ALBEDO_SUB_MAP_APPLY_SCALE
    see `ALBEDO_MAP_APPLY_SCALE`
* ##### ALBEDO_SUB_MAP_FILE
    see `ALBEDO_MAP_FILE`
* ##### const float3 albedoSub = 1.0;
    between 0 ~ 1

* ##### const float2 albedoSubMapLoopNum = 1.0; 
    see `albedoMapLoopNum`

Alpha:
----------------
　　It has no effect on opaque objects.

* ##### ALPHA_MAP_FROM
    see `ALBEDO_MAP_FROM`

* ##### ALPHA_MAP_UV_FLIP
    see `ALBEDO_MAP_UV_FLIP`

* ##### ALPHA_MAP_SWIZZLE
    The ordering of the data fetched from a `texture` from the `code`.

    0 . R channel  
    1 . G channel  
    2 . B channel  
    3 . A channel  

* ##### ALPHA_MAP_FILE "alpha.png"
    see `ALBEDO_MAP_FILE`

* ##### const float alpha = 0.0 ~ 1.0;
* ##### const float2 alphaMapLoopNum = 0.0 ~ inf;

Normal:
-------------
　　`SSAO` only supports `non-empty` normals else will result a `white edge` issue, 
When you see some error that looks like some `white edges` on your actor's model, you can put the scene in the `PMXEditor`
And check the scene that all `normals` are not `zero-length` (XYZ are same equal to zero) to be used for model.

* ##### NORMAL_MAP_FROM
    see `ALBEDO_MAP_FROM`
* ##### NORMAL_MAP_TYPE
    Other parameter types for tangent normal, see UE4 [docs](https://docs.unrealengine.com/latest/INT/Engine/Rendering/LightingAndShadows/BumpMappingWithoutTangentSpace/index.html) for `PerturbNormalLQ` and `PerturbNormalHQ`.
    
    0 . Calculate world-space normal from RGB tangent-space map.  
    1 . Calculate world-space normal from RG  compressed tangent-space map.  
    2 . Calculate world-space normal from Grayscale bump map by `PerturbNormalLQ` (Low  Quality). It has no effect on small objects.  
    3 . Calculate world-space normal from Grayscale bump map by `PerturbNormalHQ` (High Quality).  
    4 . Calculate world-space normal from RGB world-space map.  

* ##### NORMAL_MAP_UV_FLIP
    see `ALBEDO_MAP_APPLY_SCALE`
* ##### NORMAL_MAP_FILE
    see `ALBEDO_MAP_FILE`
* ##### const float normalMapScale = 0 ~ inf;
* ##### const float normalMapLoopNum = 0 ~ inf;

SubNormal
-------------
* ##### NORMAL_SUB_MAP_FROM
    see `ALBEDO_MAP_FROM`
* ##### NORMAL_SUB_MAP_TYPE
    see `NORMAL_MAP_TYPE`
* ##### NORMAL_SUB_MAP_UV_FLIP
    see `ALBEDO_MAP_APPLY_SCALE`
* ##### NORMAL_SUB_MAP_FILE
    see `ALBEDO_MAP_FILE`
* ##### const float normalSubMapScale = 0.0 ~ inf;
* ##### const float normalSubMapLoopNum = 0.0 ~ inf;

Smoothness
-------------
　　Default data will fetched params from the `SpecularPower` and convert the `SpecularPower` to `Smoothness`.

* ##### SMOOTHNESS_MAP_FROM
    see `ALBEDO_MAP_FROM`

* ##### SMOOTHNESS_MAP_TYPE
    Other parameter types for `Smoothness`

    0 . `Smoothness` (from Frostbite / CE5 textures)  
    1 . Calculate `Smoothness` from `Roughness` by 1.0 - Roughness ^ 0.5 (from UE4/GGX/SubstancePainter2 textures)  
    2 . Calculate `Smoothness` from `Roughness` by 1.0 - Roughness       (from UE4/GGX/SubstancePainter2 with roughness linear roughness)  

* ##### SMOOTHNESS_MAP_UV_FLIP
    see `ALBEDO_MAP_UV_FLIP`
* ##### SMOOTHNESS_MAP_SWIZZLE
    see `ALPHA_MAP_SWIZZLE`
* ##### SMOOTHNESS_MAP_APPLY_SCALE
    see `ALBEDO_MAP_APPLY_SCALE`
* ##### SMOOTHNESS_MAP_FILE
    see `ALBEDO_MAP_FILE`
* ##### const float smoothness = 0.0 ~1.0;
* ##### const float smoothnessMapLoopNum = 1.0; 
    see `albedoMapLoopNum`

Metalness:
-------------
* ##### METALNESS_MAP_FROM
    see `ALBEDO_MAP_FROM`
* ##### METALNESS_MAP_UV_FLIP
    see `ALBEDO_MAP_UV_FLIP`
* ##### METALNESS_MAP_SWIZZLE
    see `ALPHA_MAP_SWIZZLE`
* ##### METALNESS_MAP_APPLY_SCALE
    see `ALBEDO_MAP_APPLY_SCALE`
* ##### METALNESS_MAP_FILE
    see `ALBEDO_MAP_FILE`
* ##### const float metalness = 0.0 ~ 1.0;
* ##### const float metalnessMapLoopNum = 1.0;
    see `albedoMapLoopNum`

Specular:
-------------
　　It has no effect on `Metalness` > 0 and `CUSTOM_ENABLE` > 0, `specular maps` are not `environment` and `sphere` maps,
only modifies the `reflection index` of the model, Used to change the colors of the environment reflect.
If you don't want model to reflect the specular color, you can set zero to `const float3 specular = 0.0;`

* ##### SPECULAR_MAP_FROM
    see ALBEDO_MAP_FROM
* ##### SPECULAR_MAP_TYPE
    Other parameter types for Specular

    0 . Calculate reflection coefficient from specular color by F(x) = 0.08*(x  ) (from UE4 textures)  
    1 . Calculate reflection coefficient from specular color by F(x) = 0.16*(x^2) (from Frostbite textures)  
    2 . Calculate reflection coefficient from specular grays by F(x) = 0.08*(x  ) (from UE4 textures)  
    3 . Calculate reflection coefficient from specular grays by F(x) = 0.16*(x^2) (from Frostbite textures)  
    4 . Using reflection coefficient (0.04) instead of specular value (0.5), Available when `SPECULAR_MAP_FROM` at 0  

* ##### SPECULAR_MAP_UV_FLIP
    see `ALBEDO_MAP_UV_FLIP`
* ##### SPECULAR_MAP_SWIZZLE
    see `ALPHA_MAP_SWIZZLE`
* ##### SPECULAR_MAP_APPLY_SCALE
    see `ALBEDO_MAP_APPLY_SCALE`
* ##### SPECULAR_MAP_FILE
    see `ALBEDO_MAP_FILE`
* ##### const float3 specular = 0.5;
    Anything less than `2%` is physically impossible and is instead considered to be shadowing  
    For example: The reflectance coefficient is equal to `F(x) = (x - 1)^2 / (x + 1)^2`  
    Consider light that is incident upon a transparent medium with a refractive index of `1.5`  
    That result will be equal `to (1.5 - 1)^2 / (1.5 + 1)^2` = `0.04` (or `4%`).  
    Specular to reflection coefficient is equal to `F(x) = 0.08*x`, if the `x` is equal to `0.5` the result will be `0.04`.  
    So default value is `0.5` for `0.04` coeff and clamped value between `0` ~ `1`  

* ##### const float2 specularMapLoopNum = 1.0;
    see albedoMapLoopNum

Occlusion
-------------
　　The ambient occlusion (AO) is an effect that approximates the attenuation of environment light due to occlusion.
Bacause `sky lighting` from many directions, cannot simply to calculating shadows in the real-time.
A simply way able to replaced by using `occlusion map` and `SSAO`,
and also you can set zero to the `const float occlusion = 1.0` if you don't want `diffuse` and `specular`.

* ##### OCCLUSION_MAP_FROM
    see `ALBEDO_MAP_FROM`

* ##### OCCLUSION_MAP_TYPE
    Other parameter types for `occlusion`

    0 . Fetch ambient occlusion from linear color-space  
    1 . Fetch ambient occlusion from sRGB   color-space  
    2 . Fetch ambient occlusion from linear color-space from second UV set  
    3 . Fetch ambient occlusion from sRGB   color-space from second UV set  

* ##### OCCLUSION_MAP_UV_FLIP
    see ALBEDO_MAP_UV_FLIP for more information.
* ##### OCCLUSION_MAP_SWIZZLE
    see ALPHA_MAP_SWIZZLE for more information.
* ##### OCCLUSION_MAP_APPLY_SCALE
    see ALBEDO_MAP_APPLY_SCALE for more information.
* ##### const float occlusion = 0.0 ~ 1.0;
* ##### const float occlusionMapLoopNum = 0.0 ~ inf;

Parallax:
-------------
　　The parallax map does not work with vertex displacement in the `DX9`

* ##### PARALLAX_MAP_FROM
    see ALBEDO_MAP_FROM for more information.

* ##### PARALLAX_MAP_TYPE
    Other parameter types for `parallax`

    0 . calculate without transparency  
    1 . calculate parallax occlusion with transparency and best `SSDO`  

* ##### PARALLAX_MAP_UV_FLIP 
    see `ALBEDO_MAP_UV_FLIP`

* ##### PARALLAX_MAP_SWIZZLE 
    see `ALPHA_MAP_SWIZZLE`

* ##### PARALLAX_MAP_FILE
    see `ALBEDO_MAP_FILE`

* ##### const float parallaxMapScale = 0.0 ~ inf;
* ##### const float parallaxMapLoopNum = 0.0 ~ inf;

Emissive
-------------
　　You can add a light source in MMD (PointLight or others), And key it as part of emissive of the model, and same color set it to your light source and emissive color
* ##### EMISSIVE_ENABLE
* ##### EMISSIVE_MAP_FROM
   see `ALBEDO_MAP_FROM`
* ##### EMISSIVE_MAP_UV_FLIP
   see `ALBEDO_MAP_UV_FLIP`
* ##### EMISSIVE_MAP_APPLY_SCALE
   see `ALBEDO_MAP_APPLY_SCALE`
* ##### EMISSIVE_MAP_APPLY_MORPH_COLOR
   see `ALBEDO_MAP_APPLY_MORPH_COLOR`

* ##### EMISSIVE_MAP_APPLY_MORPH_INTENSITY
   Texture colors to multiply with intensity from morph controller (Intensity+/-).
* ##### EMISSIVE_MAP_APPLY_BLINK
   You can set the blink using following code.

   1 . colors to multiply with frequency from `emissiveBlink`. like : const float3 emissiveBlink = float3(1.0, 2.0, 3.0);  
   2 . colors to multiply with frequency from `morph controller`, For PointLight.pmx...

* ##### EMISSIVE_MAP_FILE
  see `ALBEDO_MAP_FILE`

* ##### const float3 emissive = 0.0 ~ 1.0;
* ##### const float3 emissiveBlink = 0.0 ~ 10.0;
* ##### const float  emissiveIntensity = 0 ~ 100 and above
* ##### const float2 emissiveMapLoopNum = 0.0 ~ inf;

Shading Model ID
-------------
   The curvature also called "opacity", You can see the UE4 docs for more information
   https://docs.unrealengine.com/latest/INT/Engine/Rendering/Materials/LightingModels/SubSurfaceProfile/index.html

   0 . Default            customA = invalid,    customB = invalid  
   1 . PreIntegrated Skin customA = curvature,  customB = transmittance color;  
   2 . Unlit placeholder  customA = invalid,    customB = invalid  
   3 . Anisotropy         customA = Anisotropic, customB = invalid  

 In order to make refraction work, you must set alpha value of the pmx model to less then 0.999
 4 : Glass              customA = curvature   customB = transmittance color

 see paper for cloth information : http://blog.selfshadow.com/publications/s2017-shading-course/imageworks/s2017_pbs_imageworks_sheen.pdf
 Sheen is difference between GGX and InvGGX
 Fuzz Color is f0 of fresnel params in sRGB color-space
    5 . Cloth              customA = sheen,      customB = Fuzz Color  
    6 . Clear Coat         customA = smoothness, customB = invalid;  
    7 . Subsurface         customA = curvature,  customB = transmittance color;  

 see this paper for more information, but chinese
 https://zhuanlan.zhihu.com/p/26409746
    8 . Cel Shading        customA = threshold,  customB = shadow color;
    9 . ToonBased Shading  customA = haredness,  customB = shadow color;

* ##### CUSTOM_ENABLE 0

* ##### CUSTOM_A_MAP_FROM 0 
    see ALBEDO_MAP_FROM for more information.

* ##### CUSTOM_A_MAP_UV_FLIP 0
* ##### CUSTOM_A_MAP_COLOR_FLIP 0
* ##### CUSTOM_A_MAP_SWIZZLE 0
* ##### CUSTOM_A_MAP_APPLY_SCALE 0
* ##### CUSTOM_A_MAP_FILE "custom.png"

* ##### const float customA = 0.0 ~ 1.0;
    linear-space

* ##### const float customAMapLoopNum = 1.0;
   see `albedoMapLoopNum`

* ##### CUSTOM_B_MAP_FROM 0
   see `ALBEDO_MAP_FROM`
* ##### CUSTOM_B_MAP_UV_FLIP 0
* ##### CUSTOM_B_MAP_COLOR_FLIP 0
* ##### CUSTOM_B_MAP_APPLY_SCALE 0
* ##### CUSTOM_B_MAP_FILE "custom.png"

* ##### const float3 customB = 0.0 ~ 1.0;
    sRGB color-space
* ##### const float2 customBMapLoopNum = 1.0;
    see albedoMapLoopNum

FAQ:
--------------------
* What is sRGB-color and Gamma
    * The Gamma is near 2.2 used most of time, About sRGB and Gamma, You can see docs for more information  
    * `https://developer.nvidia.com/gpugems/GPUGems3/gpugems3_ch24.html`  
    * `https://en.wikipedia.org/wiki/SRGB`  

* What is gloss map
    * Gloss map is a `smoothness map`

* Where melanin
    * It has moved into `ALBEDO_SUB_ENABLE`, see `ALBEDO_SUB_ENABLE` for more information

* Why increase number of parallaxMapLoopNum will increase the loop number of albedo, normals, etc  
    * Bacause parallax coordinates can be calculated from height map,that are then used to access textures with albedo, normals, smoothness, metalness, etc, In other words like fetched data (albedo, normals, etc) from parallax coordinates * parallaxMapLoopNum * albedo/normal/MapLoopNum