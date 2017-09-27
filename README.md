# Flora

可能是Android平台上最快的图片压缩框架。

## 用法用例

* 异步压缩：

```java
Flora.with().load(source...).compress(new Callback<>());
```

* 同步压缩:

```java
Flora.with().load(R.drawable.test2).compressSync();
```

## 可控的压缩任务

```java
Flora.with(Activity)
Flora.with(FragmentActivity)
Flora.with(Fragment)
Flora.with(SupportFragment)
// 上面的四类，会自动在页面生命周期结束后，终止压缩任务。

// 通过TAG标志位来结束一系列的任务。
Flora.with(TAG)
// 强制结束任务。
Flora.cancel(TAG);

// 无参，此时的任务进行其实是不受控的，强烈建议不要采用这种写法。
Flora.with() 
```

## 更多属性
```java
Flora.with()
        // 配置inSample和quality的算法，内置了一套基于Luban的压缩算法
        .calculation(new Calculation() {
            @Override
            public int calculateInSampleSize(int srcWidth, int srcHeight) {
                return super.calculateInSampleSize(srcWidth, srcHeight);
            }

            @Override
            public int calculateQuality(int srcWidth, int srcHeight, int targetWidth, int targetHeight) {
                return super.calculateQuality(srcWidth, srcHeight, targetWidth, targetHeight);
            }
        })
        // 对压缩后的图片做个性化地处理，如：添加水印
        .addDecoration(new Decoration() {
            @Override
            public Bitmap onDraw(Bitmap bitmap) {
                return super.onDraw(bitmap);
            }
        })
        // 配置Bitmap的色彩格式
        .bitmapConfig(Bitmap.Config.RGB_565)
        // 最大文件尺寸，不建议配置此项，目前采用的方式是重复写入文件，判断大小，比较耗时
        .maxFileSize(1.0)
        // 同时可进行的最大压缩任务数量
        .compressTaskNum(1)
        // 安全内存，设置为2，表示此次压缩任务需要的内存小于1/2可用内存才进行压缩任务
        .safeMemory(2)
        // 压缩完成的图片在磁盘的存储目录
        .diskDirectory(File)
        .load(source...)
        .compress();
```

## 其他

* 压缩速度

本身内部采用线程池的方案去进行压缩任务，同时进行了必要的内存检查。

在不会OOM的前提下，最大的提升了压缩的速度，常见的9图大小在20M+能够在2s内处理完成。

当然，机器性能，系统当时的内存都是对此产生影响，我的测试机是【魅蓝Note】...

* 压缩效果

由于压缩策略集成自Luban，所以最后图片压缩大小前后对比可以参考Luban。

我在此基础上，对社交产品中常见的长图的需求进行了一定的优化。