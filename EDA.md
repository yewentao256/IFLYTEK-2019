# EDA

## label标签分布 

### 初赛+复赛，共600万

![1566457086453](assets\1566457086453.png)

```
0.0    2448555	0.4080925
1.0    3551445	0.5919075
```

### 初赛 100万

![1566457759270](assets\1566457759270.png)

注意此处图表x轴坐标顺序

```
0.0    517106	0.517106
1.0    482894	0.482894
```

### 复赛 500万

![1566457879524](assets\1566457879524.png)

```
0.0    1931449	0.3862898
1.0    3068551	0.6137102
```

### 小结

1. 初赛标签分布较为平均，几乎为1：1，而复赛数据中标签0：1的比例接近4：6，整体训练集比例也接近4：6
2. 从正常理解角度，欺诈的请求数量应远小于正常的请求数量，有可能有部分是生成样本？还是说实际上现在大部分的广告流量已被欺诈占据？
3. 从初赛来看，测试集的真实分布与训练集较为接近

## 特征种数分布 （包括nan）

### 初赛训练集+复赛全部数据（训练集+testA+testB）

![1566458115565](assets\1566458115565.png)

### 初赛训练集+复赛训练集

![1566458272639](assets\1566458272639.png)

### test_A_data

![1566459793296](assets\1566459793296.png)

### test_B_data

![1566459837537](assets\1566459837537.png)

### 小结

1. 特征根据种类数多少可分为：
   1. 小于10：['province','dvctype','ntt','carrier','orientation']
   2. 10-100：[‘apptype’,'lan']
   3. 100-1000:['adunitshowid','mediashowid','city','osv','ppi','w']
   4. 1000-10000:['pkgname','ver','make','h']
   5. 10000-100000:['idfamd5','model','reqrealip']
   6. 大于100000:['nginxtime','adidmd5','imeimd5',’openudidmd5','macmd5','ip']
2. 根据相关类型可分为：
   1. 应用程序app:['pkgname','ver','apptype']
      1. apptype比较有限，一种app下有多款app
      2. pkgname代表一个app，一个app可有多种版本
   2. 手机系统:['dvctype','ntt','carrier','os','osv','lan']
      1. osv和lan清洗后应少很多，大部分都小于10
   3. ip:['ip','reqrealip']
      1. 大部分ip应至少有两次出现
      2. 'rqrealip' - 请求的http协议头携带IP，有可能是下游服务器的ip。不太理解
   4. 手机身份标识:['adidmd5','imeimd5','macmd5']
      1. 都大于200万
   5. 屏幕:['orientation','h','w','ppi']
   6. 品牌:['make','model']
   7. 地理位置:['city','province']
   8. 广告媒体:['adunitshowid','mediashowid','idfamd5','openudidmd5']
      1. 前二者小于1000
   9. 时间:['nginxtime']
      1. 几乎每个请求的时间戳不一致，但仍有极少一致的存在

## 缺失值与缺失值比例

### 初赛训练集+复赛全部数据（训练集+testA+testB）

![1566459457243](assets\1566459457243.png)

### 初赛训练集+复赛训练集

![1566459513362](assets\1566459513362.png)

### test_A_data

![1566459676363](assets\1566459676363.png)

### test_B_data

![1566459738324](assets\1566459738324.png)

### 小结

1. province需要通过城市清洗
2. idfa和openudid可删去
3. 没有或几乎没有缺失值:['nginxtime','ip','city','reqrealip','adunitshowid','mediashowid','imeimd5','model','os','osv','orientation']
4. 其余特征应有针对性的进行填充
5. 对缺失值的多少进行计数

## 对种类数较少特征的统计

### 初赛训练集+复赛全部数据（训练集+testA+testB）

#### province

![1566460631787](assets\1566460631787.png)

#### dvctype

![1566460671376](assets\1566460671376.png)

#### ntt 网络类型

![1566460710060](assets\1566460710060.png)

#### carrier 运营商

![1566460757929](assets\1566460757929.png)

#### os

![1566460801215](assets\1566460801215.png)

#### orientation

![1566460830736](assets\1566460830736.png)

## 对屏幕特征（视为数值）的统计

### 初赛训练集+复赛全部数据（训练集+testA+testB）

#### h

![1566461012181](assets\1566461012181.png)

![1566461506204](assets\1566461506204.png)

平板的h取值更加集中，且中位数取值大于手机，符合常规。

![1566461637858](assets\1566461637858.png)

尽管ntt=7是不在列表中的取值，但是该特征关于h分布仍然是一个偏正常的。

![1566461755152](assets\1566461755152.png)

#### w

![1566461039167](assets\1566461039167.png)

出现离群点

#### ppi

![1566461079781](assets\1566461079781.png)

## 同一特征的在不同取值下的标签分布

### 初赛训练集+复赛训练集

![train_data_raw-distribution](assets\train_data_raw-distribution-1566464264380.png)

### 初赛训练集

![train_data_raw_1-distribution](assets\train_data_raw_1-distribution.png)

### 复赛训练集

![train_data_raw_2-distribution](assets\train_data_raw_2-distribution.png)

### 时间特征

#### 天数分布

##### 初赛训练集+复赛全部数据（训练集+testA+testB）

![1566476452535](assets\1566476452535.png)

##### 初赛训练集+复赛训练集

![1566476527440](assets\1566476527440.png)

![1566479480781](assets\1566479480781.png)

##### 复赛全部测试集

![1566476612649](assets\1566476612649.png)

test_A与test_B各100万

#### 小时分布

##### 初赛训练集+复赛全部数据（训练集+testA+testB）

![1566476944519](assets\1566476944519.png)

##### 初赛训练集+复赛训练集

![1566477001576](assets\1566477001576.png)

![1566479499589](assets\1566479499589.png)

##### 复赛全部测试集

![1566477024045](assets\1566477024045.png)

### 小结

1. 发现尽管天数相同，但是月份不同，初赛为6月，复赛为8月

## 关于ip信息与地理位置

发现 city为空值的对应 ip为外国，但是对应reprealip为国内。（reqrealip应都是国内）

猜测：city是由ip获取的，该查询ip的api舍弃了外国的地区。reqrealip有可能是请求目的地下游的服务器，因此即使ip是外国也至少要经过国内的服务器来抵达目的地。