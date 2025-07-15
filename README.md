# 卫源擎 (WYLib)

###### v1.0.1

卫星影像常见操作 ***Python*** 库，如读写影像，大气校正，根据经纬度/矢量取光谱，重采样，光谱绘制等。

主要依赖库: `numpy 1.x`, `GDAL`, `opencv-python`, `matplotlib`, `pandas`, `tqdm`, `snappy`, `openpyxl`等。
<!--
## 功能目录

<table>
<tbody>
    <tr>
        <th>类名</th><th>方法名</th><th>功能</th>
    </tr>
    <tr>
        <td rowspan="3" style="text-align:center">RSImage</td>
        <td style="text-align:center">get_imagexy</td>
        <td style="text-align:center">根据经纬度获取影像行列号</td>
    </tr>
    <tr>
        <td style="text-align:center">get_lonlat</td>
        <td style="text-align:center">根据影像行列号获取经纬度</td>
    </tr>
    <tr>
        <td style="text-align:center">save_as</td>
        <td style="text-align:center">影像另存为</td>
    </tr>
    <tr>
        <td rowspan="2" style="text-align:center">Sentinel2L1CImage</td>
        <td style="text-align:center">resample_10m</td>
        <td style="text-align:center">重采样至10米</td>
    </tr>
    <tr>
        <td style="text-align:center">-</td>
        <td style="text-align:center">-</td>
    </tr>
    <tr>
        <td rowspan="2" style="text-align:center">Sentinel2L2AImage</td>
        <td style="text-align:center">resample_10m</td>
        <td style="text-align:center">重采样至10米</td>
    </tr>
    <tr>
        <td style="text-align:center">-</td>
        <td style="text-align:center">-</td>
    </tr>
    <tr>
        <td rowspan="10" style="text-align:center">-</td>
        <td style="text-align:center">image_resize</td>
        <td style="text-align:center">图像缩放</td>
    </tr>
    <tr>
        <td style="text-align:center">position_transform_str2num</td>
        <td style="text-align:center">经纬度字符串形式转浮点数形式</td>
    </tr>
    <tr>
        <td style="text-align:center">show_spectra_AWRMMS</td>
        <td style="text-align:center">AWRMMS光谱仪测量结果展示</td>
    </tr>
    <tr>
        <td style="text-align:center">sentinel_data_download</td>
        <td style="text-align:center">Sentinel卫星影像数据下载</td>
    </tr>
    <tr>
        <td style="text-align:center">load_spectral_respones_function</td>
        <td style="text-align:center">读取Sentinel-2 MSI光谱响应函数</td>
    </tr>
    <tr>
        <td style="text-align:center">batch_resample</td>
        <td style="text-align:center">Snap批量重采样</td>
    </tr>
    <tr>
        <td style="text-align:center">batch_C2RCC</td>
        <td style="text-align:center">Snap批量C2RCC大气校正</td>
    </tr>
    <tr>
        <td style="text-align:center">batch_resample_C2RCC</td>
        <td style="text-align:center">Snap批量重采样并C2RCC大气校正</td>
    </tr>
    <tr>
        <td style="text-align:center">S2_resampled_edit</td>
        <td style="text-align:center">修改Snap生成的重采样影像数据内容</td>
    </tr>
    <tr>
        <td style="text-align:center">band_stack_C2RCC</td>
        <td style="text-align:center">堆叠C2RCC大气校正结果波段</td>
    </tr>
</table>
-->
## Wheel文件安装

若无Conda环境需先创建，并确保`numpy 1.x`, `GDAL`, `opencv-python`, `matplotlib`, `pandas`, `tqdm`, `snappy`, `requests`等库已安装完毕。

本库目前仅支持Wheel文件安装即可。

```bash
pip install wylib-1.0.1-py3-none-any.whl
```

## 各类方法介绍及示例

### RSImage类

实例化RSImage即可加载卫星影像。

```python
from wylib.models.RSImage import *

rs_image = RSImage("./res/input_image.dat")

print(rs_image.ds)
print(rs_image.img_GeoTransform)
print(rs_image.img_proj)
print(rs_image.image_array)
print(rs_image.column)
print(rs_image.row)
print(rs_image.band_count)
print(rs_image.no_data_value)
print(rs_image.dtype)
```

#### get_imagexy

根据经纬度获取影像行列号。

```python
from wylib.models.RSImage import *

rs_image = RSImage("./res/input_image.dat")

print(rs_image.get_imagexy("115°58'11\"", "38°56'18\""))
print(rs_image.get_imagexy(115.969722, 38.938333))
```

#### get_lonlat

根据影像行列号获取经纬度。

```python
from wylib.models.RSImage import *

rs_image = RSImage("./res/input_image.dat")

print(rs_image.get_lonlat(365, 325))
```

#### save_as

新建影像可先实例化空影像，再将必要属性赋值后利用`save_as`方法保存，目前支持格式`dat`、`tif`、`tiff`、`img`。

```python
from wylib.models.RSImage import *

rs_image = RSImage()
rs_image.image_array = np.zeros((3, 128, 128))
# rs_image.img_GeoTransform = img_GeoTransform
# rs_image.img_proj = img_proj
rs_image.save_as("./new_image.tif")
```

#### resample

重采样至指定大小。

```python
from wylib.models.RSImage import *

rs_image = RSImage("./res/input_image.dat")
rs_image.resample(1024, 1024, method="nearest")
rs_image.save_as("./output.tif")
```

### Sentinel2L1CImage类

读取Sentinel-2数据，每个波段图像实例化为`RSImage`对象。

```python
from wylib.models.Sentinel2L1CImage import *

s2 = Sentinel2L1CImage("./res/S2C_MSIL1C_20250317T030551_N0511_R075_T50SMJ_20250317T064203.SAFE")

print(s2.B1)
```

#### resample_10m

重采样至10米，`resample_10m`方法返回值为`RSImage`对象，`B10_flag`标记表示结果中是否包含B10，对于Sentinel-2原数据的坐标操作需设置`sentinel_flag`为`True`。

```python
from wylib.models.Sentinel2L1CImage import *

s2 = Sentinel2L1CImage("./res/S2C_MSIL1C_20250317T030551_N0511_R075_T50SMJ_20250317T064203.SAFE")
rs_image = s2.resample_10m(method="nearest", B10_flag=False)

print(rs_image)
print(rs_image.get_imagexy("115°58'11\"", "38°56'18\"", sentinel_flag=True))
print(rs_image.get_imagexy(115.969722, 38.938333, sentinel_flag=True))
print(rs_image.get_lonlat(1074, 8960, sentinel_flag=True))
```

`method`参数有如下可选：

```python
import cv2

RESAMPLE_METHODS = {
    "nearest": cv2.INTER_NEAREST,
    "bilinear": cv2.INTER_LINEAR,
    "bicubic": cv2.INTER_CUBIC,
    "area": cv2.INTER_AREA,
    "lanczos4": cv2.INTER_LANCZOS4,
    "linear_exact": cv2.INTER_LINEAR_EXACT,
    "nearest_exact": cv2.INTER_NEAREST_EXACT,
    "max": cv2.INTER_MAX,
    "warp_fill_outliers": cv2.WARP_FILL_OUTLIERS,
    "warp_inverse_map": cv2.WARP_INVERSE_MAP
}
```

### Sentinel2L2AImage类

```python
from wylib.models.Sentinel2L2AImage import *

s2 = Sentinel2L2AImage("./res/S2B_MSIL2A_20250319T025529_N0511_R032_T50SMJ_20250319T050430.SAFE")

print(s2.SCL)
```

#### resample_10m

重采样至10米。

```python
from wylib.models.Sentinel2L2AImage import *

s2 = Sentinel2L2AImage("./res/S2B_MSIL2A_20250319T025529_N0511_R032_T50SMJ_20250319T050430.SAFE")
rs_image = s2.resample_10m(method="bicubic")

print(rs_image)
print(rs_image.get_imagexy("115°58'11\"", "38°56'18\"", sentinel_flag=True))
print(rs_image.get_imagexy(115.969722, 38.938333, sentinel_flag=True))
print(rs_image.get_lonlat(1074, 8960, sentinel_flag=True))
```

### 自然图像相关

#### image_resize

图像重采样至指定尺寸，可支持多通道、批处理。

```python
from wylib.utils import *

image = np.zeros((10, 3, 128, 128))
new_image = image_resize(image, 256, 256, dtype=np.uint16, interpolation_method=cv2.INTER_CUBIC)
print(new_image)
```

#### tif_to_jpg

批量tif转jpg。

```python
from wylib.utils import *

tif_to_jpg("./tif_path", "./jpg_path")
```

#### tif_to_png

批量tif转png。

```python
from wylib.utils import *

tif_to_png("./tif_path", "./png_path")
```

### SNAP相关

#### batch_resample

Snap批量重采样，输入目录中可放入若干景Senrinel-2 L1C影像。

```python
from wylib.auto_snap import *

batch_resample("./input", "./output")
```

#### batch_C2RCC

Snap批量C2RCC大气校正，输入目录中可放入若干Snap官方格式数据（dim文件及配套data目录）。

```python
from wylib.auto_snap import *

batch_C2RCC("./input", "./output")
```

#### batch_resample_C2RCC

Snap批量重采样并C2RCC大气校正，输入目录中可放入若干景Senrinel-2 L1C影像。

```python
from wylib.auto_snap import *

batch_resample_C2RCC("./input", "./output")
```

#### S2_resampled_edit

修改Snap生成的重采样影像数据内容。

```python
from wylib.auto_snap import *

S2_resampled_edit("./custom_image.dat", "./S2B_MSIL2A_20250319T025529_N0511_R032_T50SMJ_20250319T050430_resampled.data")
```

#### band_stack_C2RCC

堆叠C2RCC大气校正结果波段，输入目录中可放入若干Snap官方格式数据（dim文件及配套data目录）。

```python
from wylib.auto_snap import *

band_stack_C2RCC("./input", "./output")
```

### 其它

#### position_transform_str2num

经纬度字符串形式转浮点数形式。

```python
from wylib.utils import *

print(position_transform_str2num("115°58'11\""))
```

#### show_spectra_AWRMMS

AWRMMS光谱仪测量结果展示，`inputFolderPath`为光谱仪配套软件输出结果,`delete_index_list`可选择需要删除的测量记录，程序结束后可将平均光谱保存在当前目录下。

```python
from wylib.utils import *

show_spectra_AWRMMS(inputFolderPath="./Output", delete_index_list=[2])
```

#### sentinel_data_download

Sentinel卫星影像数据下载，需要欧空局Copernicus Data Space Ecosystem账号。

```python
from wylib.utils import *

product_name_list = [
    "S2B_MSIL2A_20250319T025529_N0511_R032_T50SMJ_20250319T050430.SAFE",
    "S2C_MSIL1C_20250317T030551_N0511_R075_T50SMJ_20250317T064203.SAFE"
]
sentinel_data_download(product_name_list, path="./Downloads")
```

#### load_spectral_respones_function

读取Sentinel-2 MSI光谱响应函数(https://sentiwiki.copernicus.eu/web/s2-documents)， 读取波长范围327nm - 940nm，包含B1、B2、B3、B4、B5、B6、B7、B8、B8A共9个波段，载荷名称`S2A`, `S2B`, `S2C`三选一。

```python
from wylib.utils import *

srf = load_spectral_respones_function("S2C", "./res/COPE-GSEG-EOPG-TN-15-0007 - Sentinel-2 Spectral Response Functions 2024 - 4.0.xlsx")
print(srf.shape)
```

**更多功能正在开发中，敬请等待！**