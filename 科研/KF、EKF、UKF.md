> 主要是线性卡尔曼的基本原理，常用的非线性卡尔曼滤波，如扩展卡尔曼，无迹卡尔曼和粒子滤波。融合方面主要是分布式状态融合，包括矩阵加权融合和CI融合。主要是原理以及用这些方法做些简单分析设计
# 卡尔曼滤波
## 卡尔曼滤波算法
![](https://files.mdnice.com/user/25190/3b5223be-7f39-439a-a9b6-335d1f777c6d.png)
因为Q含在$P_k^-$中，所以当Q很小时，卡尔曼增益会很小，那么就会更相信模型的预测值。

![卡尔曼滤波算法](https://files.mdnice.com/user/25190/c0415bf4-b895-4feb-8d45-e86999baebe6.png)

## 举例

![](https://files.mdnice.com/user/25190/8fe0ab51-e5b9-4ce1-9690-eb1cd9cf4ef2.png)

![](https://files.mdnice.com/user/25190/4d3d8a62-d7ba-40af-ba89-debbe5eec39d.png)

![](https://files.mdnice.com/user/25190/9e26cb5c-0f65-4e87-8663-04a23f3bc9f0.png)

![](https://files.mdnice.com/user/25190/5142eb65-7fa2-477b-922f-300ec69361c9.png)

matlab编程

![](https://files.mdnice.com/user/25190/6d4d5436-24f8-4ce3-a9e6-d38cd10ebfa0.png)

![](https://files.mdnice.com/user/25190/cbef0df6-fa4e-4cd3-956b-c7b11f753e3d.png)

## 卡尔曼滤波存在限制

只适用于线性系统，A是常数矩阵。

![](https://files.mdnice.com/user/25190/5cd83e70-d835-457d-aec9-155b88157f96.png)

![](https://files.mdnice.com/user/25190/9ce87b1f-77c3-4004-ae65-34cd06834079.png)


![](https://files.mdnice.com/user/25190/b48dc679-53f9-40f5-91f1-14aea50e880f.png)

为了能够在非线性系统上做卡尔曼滤波，可以讲非线性系统线性化，这就是扩展卡尔曼滤波（EKF）的精髓。

![](https://files.mdnice.com/user/25190/e59cccc6-2d1b-466c-8ce7-44c848d3bc8e.png)

# 扩展卡尔曼滤波


![](https://files.mdnice.com/user/25190/162970ed-ed27-498a-a0b9-2db7b3485831.png)

![](https://files.mdnice.com/user/25190/13a6f419-05c8-4104-8433-6564a7a9b28c.png)

虽然线性化之后可以使用卡尔曼滤波，但也是由于线性化会带来不准确，故这种方法还需要改进。
## EKF算法

![](https://files.mdnice.com/user/25190/195d310d-5fcc-40da-b650-c5eeee6645b7.png)

![](https://files.mdnice.com/user/25190/4d7e1694-4654-4e33-bf6f-0f5c5f0f549c.png)

![](https://files.mdnice.com/user/25190/01bb264f-079e-4269-9b31-6a19dfe4b729.png)

![](https://files.mdnice.com/user/25190/979160cd-1192-4508-adfb-bb1293d0b818.png)

## EKF的缺点

![](https://files.mdnice.com/user/25190/d25b4219-973c-4a00-9ed5-9cd7e5647827.png)

如果出现上图的情况，还是用直线来近似的话在骤降的那一点斜率就会相差很多，导致失真严重。为了改善这个问题可以使用无迹卡尔曼滤波（UKF）

## EKF举例

![](https://files.mdnice.com/user/25190/958a35bc-c2e8-4cce-bc64-00842515b6bc.png)


![](https://files.mdnice.com/user/25190/ad727e5c-6d40-4d02-8888-a0a51830665d.png)

![](https://files.mdnice.com/user/25190/846303f2-55a6-44aa-a51a-92307dcd37d2.png)

![](https://files.mdnice.com/user/25190/95faff1a-2dc4-407f-a9a6-775279f03619.png)

![](https://files.mdnice.com/user/25190/a0259fe5-3e0e-4767-bb8f-e807dbf0cd15.png)

由于A是线性化得到的，$P_k^-$用到了A，卡尔曼增益用到了$P_k^-$，那么当线性化A的误差很大时，卡尔曼增益误差很大，进而量测的权重误差也会很大，得到的估计值$\hat{x_k}$误差也就偏大。


![](https://files.mdnice.com/user/25190/8b3f5f36-3691-4494-afb4-262300f683a7.png)

# 无迹卡尔曼滤波（Unscented Kalman Filter）

![](https://files.mdnice.com/user/25190/1bccec18-80d0-45c0-a4a5-560e2dc272d7.png)

以撒点的方式替代线性化。

![](https://files.mdnice.com/user/25190/a30d6f0d-ab9b-472a-b0b9-8d957f49e49a.png)

![](https://files.mdnice.com/user/25190/d56d9a92-8aeb-40f0-baad-f935c82940f4.png)


![](https://files.mdnice.com/user/25190/a2f3528c-1861-4b3a-88f1-6c2d354bb8ba.png)


![](https://files.mdnice.com/user/25190/e197c12e-bd07-4772-89f2-97edcd4a9b1c.png)


![](https://files.mdnice.com/user/25190/c57ee2e9-0ca2-4bc2-bd9d-1fe34713753d.png)

![](https://files.mdnice.com/user/25190/35a06135-58b0-46a3-9e94-f645dd3dfa8a.png)


## UKF举例

![](https://files.mdnice.com/user/25190/e9da2029-cba0-4dc3-9482-498b7f9929a8.png)

撒点取值

![](https://files.mdnice.com/user/25190/b7e1119b-6b43-4bde-908f-4e2a01c5005b.png)

![](https://files.mdnice.com/user/25190/0b1c008b-9ccf-4241-a2f1-8e60429a1aa4.png)

![](https://files.mdnice.com/user/25190/35cc6580-3da6-4c88-8a95-ff891ec76597.png)

![](https://files.mdnice.com/user/25190/73a76b12-2843-4175-be35-5fb997d17f16.png)
## UKF算法

![](https://files.mdnice.com/user/25190/9e27d8c9-f5fc-48a8-96ac-fdd22f16d756.png)

![](https://files.mdnice.com/user/25190/4ca2e075-93b4-4875-bd15-1452870919e1.png)

![](https://files.mdnice.com/user/25190/ed8df7dd-f76b-429f-9648-7619ee2431f3.png)

![](https://files.mdnice.com/user/25190/658ee7a2-b784-4566-b0a9-f92712e6482f.png)
UKF相对于EKF多了撒点的动作。

## UKF举例

![](https://files.mdnice.com/user/25190/fd1835a2-eb5d-436b-a3eb-ee747654ea8c.png)

![](https://files.mdnice.com/user/25190/02cdc99b-70ec-4109-8d53-f5ecb539b8dc.png)

![](https://files.mdnice.com/user/25190/f7d8cf90-f4c1-452f-ac0d-17e417393daf.png)

![](https://files.mdnice.com/user/25190/af3008c9-5350-4a27-b0ab-7ea0df7d994c.png)

![](https://files.mdnice.com/user/25190/eaae17d8-ecc1-4ac7-9084-93d49b4061ed.png)
撒点
![](https://files.mdnice.com/user/25190/29fc64e9-1071-44c8-ae5e-10780d52db57.png)

![](https://files.mdnice.com/user/25190/74058bb8-dab1-4430-86d5-cf04c5c1fa42.png)

![](https://files.mdnice.com/user/25190/f3fce451-79e6-4b00-a6d6-a2c854b80185.png)

![](https://files.mdnice.com/user/25190/fdc8408c-0921-41bf-82eb-b87e4ce8d85e.png)


![](https://files.mdnice.com/user/25190/e9aba69f-29db-4cc0-bdb6-ce9f12db3b1e.png)

![](https://files.mdnice.com/user/25190/0f1576c5-986b-4dc1-b799-255dd8be8b95.png)
# 总结

![](https://files.mdnice.com/user/25190/f993d9b5-13d3-4b41-8058-f5e68e309bf7.png)
