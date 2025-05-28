# 实验5 **实现智能图像分类APP**

## 1.安装好Android Studio 4.1以上的版本

## 2.**下载代码编译运行**

从https://github.com/hoitab/TFLClassify中下载好代码文件，并且编译运行并解决生成项目中的错误。

## 向应用中添加[TensorFlow Lite](https://so.csdn.net/so/search?q=TensorFlow Lite&spm=1001.2101.3001.7020)

![image](https://github.com/user-attachments/assets/768a137d-1d83-4da8-8d5f-b99999835cd7)


选择已经下载的自定义的训练模型。

![image](https://github.com/user-attachments/assets/c4a24399-c5f1-47a3-96e4-7127b45ee594)


点击“Finish”完成模型导入，系统将自动下载模型的依赖包并将依赖项添加至模块的build.gradle文件。

TensorFlow Lite模型被成功导入，并生成摘要信息

## 3.**实现框架中的TODO代码项**

### 3.1 复制tflite模型

在start的main文件下创建ml文件夹

将finish下的ml文件夹中的tflite文件复制到start下的ml中

同时在start下的build.gradle添加并重新同步

```
buildFeatures{
    dataBinding = true
    mlModelBinding true
}
```

### 添加并初始化一个 TensorFlow Lite 模型

在TODO1下方添加代码

```kotlin
private val flowerModel: FlowerModel by lazy{
   // Initialize the Flower Model
            FlowerModel.newInstance(ctx, options)
        }
```

引入模型（例如使用 Model.newInstance(context)）。

定义一个成员变量来持有模型对象。

###  将 `ImageProxy` 转换为 `Bitmap`，然后转换为 `TensorImage`。

在TODO2下方添加代码

```kotlin
// TODO 2: Convert Image to Bitmap then to TensorImage
val tfImage = TensorImage.fromBitmap(toBitmap(imageProxy))
```

### 使用模型处理 `TensorImage` 输入，获取预测结果。

在TODO3下方添加代码

```kotlin
val outputs = flowerModel.process(tfImage)
    .probabilityAsCategoryList.apply {
        sortByDescending { it.score } // Sort with highest confidence first
    }.take(MAX_RESULT_DISPLAY) // take the top results
```

### 把模型返回的 `Category` 或 `Label` 列表转成你的 `Recognition` 类的对象列表。

在TODO4下方添加代码

```kotlin
for (output in outputs) {
    items.add(Recognition(output.label, output.score))
}
```



###  连接真机进行调试

![image](https://github.com/user-attachments/assets/fc3ac6b2-f857-41d5-93c3-ed98523158d6)

