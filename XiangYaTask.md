不要用brats那个代码了。即使是正确的t1ce和FLAIR也无法得到很好的效果

考虑用第一个代码，然后逐个序列选出效果最佳的。
虽然是累了点。。

T1: T1-weighted, native image, sagittal or axial 2D acquisitions, with 1–6 mm slice thickness.
T1c: T1-weighted, contrast-enhanced (Gadolinium) image, with 3D acquisition and 1 mm isotropic voxel size for most patients.
T2: T2-weighted image, axial 2D acquisition, with 2–6 mm slice thickness.
FLAIR: T2-weighted FLAIR image, axial, coronal, or sagittal 2D acquisitions, 2–6 mm slice thickness.

FLAIR包含：
T2FLAIR
t2_fse_flair
Turbo_dark_fluid

## 接下来的改进点：
- batchsize设成10，试一下能否一个病人一个病人地喂入
- 试着使用新的img和mask的组合方式。channel=4的话是无法使用预训练模型的。可以用乘性的或者加性的
- 是否要加入k折交叉验证？

