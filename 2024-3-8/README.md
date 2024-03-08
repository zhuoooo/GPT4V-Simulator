### 测试对象

![](./test-object.png)

### 视觉标记后

![](./test-object-2.png)

### 测试目标

识别出对象中哪些节点存在超出的情况

### 测试结果

第三次调优。

```json
{
  "briefExplanation": "An element is overflowing its container, indicating a layout problem.",
  "bug": [
    {
      "type": "overflow",
      "element": [33],
      "reason": "The content of the element is not fully contained within its designated area."
    }
  ]
}

```

解释：

1. 元素编号 [33] 被部分遮挡，似乎超出了容器的边缘，这表明存在溢出问题。



### prompt

今日调整：

1. 缩减任务数量减少至 “识别超出容器”
2. 添加原图片，作为在对照图
3. 添 few-shot 案例

```
任务: 分析这张图片，识别出超出容器的标记元素。

## 超出容器
type OverflowAction = { type: "overflow", element: number[], reason: string }

## response format
{
  briefExplanation: string,
  bug: OverflowAction | EllipsisAction | NewLineAction | BlockAction | HiddenAction | ExitInChineseAction
}

## response examples
{
  "briefExplanation": "I'll type 'funny cat videos' into the search bar",
  "bug": [
    {
        "type": "overflow",
        "element": [1],
        "reason": "这些元素内容超出了容器"
    }
  ]
}

## 指示
# 你是一个专业的视觉设计师，现在需要你检查页面布局。
# 第一张图片是网页的截图，第二张图片是根据第一张图片中页面元素使用 COCO Annotator进行数字标记；注意用于标记元素的边框不属于元素自身。
# 只提供一个JSON格式的输出，归类并描述这些布局问题。
# 判定超出容器：截断的文字、越过指定区域边界、元素未完全包含。
```

