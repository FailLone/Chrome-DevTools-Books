## 一、介绍
Runtime performance 是展示你的页面运行中的各种表现，注意！不是加载期间。本文将教你怎么使用Chrome DevTools Performance面板来分析运行性能。依照[RAIL](https://developers.google.com/web/fundamentals/performance/rail)标准，这些技巧将帮助你改善你页面的`Response`、`Animation`及`Idle`。

ok，我们开始吧！
## 二、开始
Performance可以在线式地帮你发现页面的性能瓶颈。
  1. 打开一个隐身模式窗口。隐身窗口能够保证Chrome处在一个干净的状态下运行。比如，你搞了一堆扩展应用，这些应用会影响Performace的分析。
  2. 打开下面这个网站，上面是一堆方块在那动来动去。
     **https://googlechrome.github.io/devtools-samples/jank**
  3. 打开开发者工具
[![Figure 1.](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/imgs/get-started.png)](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/imgs/get-started.png)
<div align='center'><strong>Figure.1</strong> 打开DevTools</div>

### 模拟移动端CPU
移动设备的CPU性能会稍微差点儿。所以如果想模拟页面在移动端上的情况，可以使用**CPU Throttling**。
  1. 进到**Performance**
  2. 确认**Screenshots**（截屏）被选中了
  3. 点击**Capture Settings**。这个是用来设置DevTools怎么抓取性能相关的各种标准。
  4. 找到**CPU**，选择**2x slowdown**。DevTools将节流，比正常水平慢一倍。

### 把demo跑起来
建立一个对所有人都一视同仁的运行性能demo几乎不可能。所以我们的demo能够让你自定义，你应该尽可能保证你的试验结果在screenshots和descriptions方面和本文差不多，不管你的机子有多好。
  1.  作死地点**Add 10**直到蓝矩形移动得显著变慢。在好机子上，这得点个20次。
  2.  点击**Optimize**。蓝矩形会动的快速和流畅多了。
  3.  点击**Un-Optimize**。蓝矩形应该又慢了下来，很垃圾。

### 记录运行性能
为什么优化版本会快这么多？首先说明，两个版本都是对矩形移动相同的距离和相同的次数。在**Performance**面板里面开启记录，现在开始学习怎么探查非优化版本的性能问题 。
  1.  点击**Record**。DevTools会在页面运行时抓取性能相关的各种参数。
      [![Figure 2.](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/imgs/profiling.png)](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/imgs/profiling.png)
      <div align='center'><strong>Figure.2</strong> 描述页面</div>
  2.  等一会儿。
  3.  点击**Stop**。DevTools停止记录，处理数据，展示结果。
      [![Figure 3.](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/imgs/results.png)](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/imgs/results.png)
      <div align='center'><strong>Figure.3</strong> 描述结果</div>
数据爆炸了！下面我们开始理清数据。
## 三、结果分析
这些数据能够帮助我们测出你的页面到底有多卡，然后，找到这个原因。
### 帧率分析
动画性能衡量的主要标准就是帧率了。`60 FPS`的动画效果就很赞了。
  1.  注意看**FPS**表，如果你看到了一条红色的条条在**FPS**上方，这证明你的帧速率掉得太低了，已经影响到了用户体验了。总的来说，绿色的条条越高，你的帧速率就越高。
  [![Figure 4.](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/imgs/fps-chart.svg)](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/imgs/fps-chart.svg)<div align='center'><strong>Figure 4.</strong> FPS表</div>
  2.  **FPS**表下面是**CPU**表，**CPU**表中的颜色与**Summary**页的颜色是对应的。后者在整个**Performance**页底部。事实上，如果**CPU**表上满是颜色，这说明在记录的这段时间里面，CPU已经跑满了。如果你看到了这种情况，你该找找办法让CPU别那么累了。
  [![Figure 5.](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/imgs/cpu-summary.svg)](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/imgs/cpu-summary.svg)<div align='center'><strong>Figure 5.</strong> CPU表</div>
  3.  在**FPS，CPU，NET**任意表上移动鼠标，你将看到那个事件点的页面快照，这叫`scrubbing`，可以用于手动计算动画的进展。 [![Figure 6.](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/imgs/screenshot.png)](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/imgs/screenshot.png)<div align='center'><strong>Figure 6.</strong> 快照查看</div>
  4.  在**Frames**部分，hover鼠标到绿矩形上面，你将看到对应的FPS。每一个frame的FPS都可能小于60.[![Figure 7.](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/imgs/frame.png)](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/imgs/frame.png)<div align='center'><strong>Figure 7.</strong> hover一个frame</div>

  通过这个demo，我们可以很明显地看出他的性能问题，但是现实生活中，这些东西的表现就不是那么清晰了，所以，这里还需要别的工具派上用场。

#### 小福利：开启FPS meter
还有一个利器，就是FPS meter了，它提供了一种实时的FPS评估。
  1.  `Control+Shift+p`开启命令菜单
  2.  输入`Rendering`，选择**Show Rendering**
  3.  在**Rendering**页，开启**FPS Meter**。一个新的覆盖窗口将会在你视口的右上角出现。[![Figure 8.](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/imgs/fps-meter.png)](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/imgs/fps-meter.png)<div align='center'><strong>Figure 8.</strong> hover一个frame</div>

### 找到瓶颈
现在你知道你的动画表现的很垃圾了，那么下一个问题是：为什么？
  1.  先看summary页。在没选中任何事件的时候，该页展示的是活动期间的饼图。当前页面耗时最多是rendering。Performance是一门减少工作的艺术，所以你的目标就是减少rendering工作。[![Figure 9.](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/imgs/summary.svg)](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/imgs/summary.svg)<div align='center'><strong>Figure 9.</strong> summary</div> 
  2.  展开**Main**。将出现展示主线程活动状态的图表。横轴是时间。每一个横条都代表了一个事件。横条越长，说明这个事件耗时越长。竖轴是调用栈。事件的堆叠代表了上方的事件触发了下方的事件。[![Figure 10.](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/imgs/main.svg)](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/imgs/main.svg)<div align='center'><strong>Figure 10.</strong> Main</div>
   3.  数据确实太多了。你可以缩小以查看某个动画帧的概况，可以放大、拖拽查看更多的信息。他们会只显示你选中的那部分的信息。[![Figure 11.](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/imgs/zoomed.png)](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/imgs/zoomed.png)<div align='center'><strong>Figure 11.</strong> 缩小以查看某一帧触发的事件</div>
   4.  注意在动画帧右上角的红色小三角。只要他出现了，说明你这个事件有点问题了。

   > requestAnimationFrame()回调执行也会触发这个动画帧事件（`Animation Frame Fired`）

   5.  点击动画帧事件。**Summary**页将显示这个事件相应的信息。注意到有个<u>reveal</u>的链接。点击他，将会高亮发起动画帧事件的事件。还有一个<u>app.js:94</u>的链接，他会跳转到相应的代码处。[![Figure 12.](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/imgs/animation-frame-fired.png)](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/imgs/animation-frame-fired.png)<div align='center'><strong>Figure 12.</strong> 动画帧事件更多的信息</div>
   6.  在**app.update**事件下，你会看到一串紫色的事件。如果他们都很宽，这表明好像每一个都应该有个红色三角形。点击随便一个紫色的**Layout**事件。你将在**Summary**页下看到关于他的一系列信息。果然，你看到了一个关于回流的警告（这是布局的另一个称谓）
   7.  在**Summary**tab下，点击**Layout Forced**下的**app.js:70**。你会看到是哪句代码导致了强制布局。
   [![Figure 13.](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/imgs/forced-layout-src.png)](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/imgs/forced-layout-src.png)<div align='center'><strong>Figure 13.</strong> 强制回流的那句代码</div>
   >这段代码有什么问题呢？在每一个动画框（`animation frame`）上，修改了每个矩形的样式，然后，又查询了每个矩形的位置信息。因为样式变了，浏览器就不知道矩形的位置是否变化了，所以只能重新布局以计算位置信息。请看这篇文章，[避免强制同步布局](https://developers.google.com/web/fundamentals/performance/rendering/avoid-large-complex-layouts-and-layout-thrashing#avoid_forced_synchronous_layouts)

   好了，基本功能介绍完了。

#### 小福利： 分析优化后的版本
使用你刚刚学到的知识点，对于优化后的demo进行性能分析。很明显，**Main**里面的帧速率大幅度降低，说明优化版本减少了大量的工作，所以才有了更好的性能。
> 优化版本完美了吗？并不是，它依然计算了每个矩形的top属性。一个更好的办法，是坚持只操作参与合成器（`compositing`）的属性。请看这篇文章，[用transform和opacity来做动画](https://developers.google.com/web/fundamentals/performance/rendering/stick-to-compositor-only-properties-and-manage-layer-count#use_transform_and_opacity_changes_for_animations)

## 三、学无止境
理解Performance的基础，是RAIL规范。这个规范告诉你哪些性能属性对你来说是最重要的。所以，第一篇要继续学习的，是：[通过RAIL模块去衡量性能](https://developers.google.com/web/fundamentals/performance/rail)

实践出真知，想在Performance上更加上手吗，那就开始测测你自己的页面，然后分析下结果。

为了真正掌握运行性能，你应该学会浏览器是怎么把HTML、CSS、JS转换成屏幕上的像素的。这是个学习的好地方->[渲染性能](https://developers.google.com/web/fundamentals/performance/rendering/)。[架构剖析](https://aerotwist.com/blog/the-anatomy-of-a-frame/)会介绍更多的细节。

最后，还有不少方法帮助提升运行性能的技巧。这篇教程只是针对于某一种特殊的动画性能瓶颈让你对Performance产生兴趣，但这只是你可能遇到的众多性能瓶颈中的一个而已。其他的关于渲染性能系列将带给你大量的运行性能各方面的提升，比如：

  *  [优化js执行](https://developers.google.com/web/fundamentals/performance/rendering/optimize-javascript-execution)
  * [缩小样式计算的范围并降低其复杂性](https://developers.google.com/web/fundamentals/performance/rendering/reduce-the-scope-and-complexity-of-style-calculations)
  * [避免大型、复杂的布局和布局抖动](https://developers.google.com/web/fundamentals/performance/rendering/avoid-large-complex-layouts-and-layout-thrashing)
  * [简化绘制的复杂度、减小绘制区域](https://developers.google.com/web/fundamentals/performance/rendering/simplify-paint-complexity-and-reduce-paint-areas)
  * [坚持仅合成器的属性和管理层计数](https://developers.google.com/web/fundamentals/performance/rendering/stick-to-compositor-only-properties-and-manage-layer-count)
  * [使输入处理程序去除抖动](https://developers.google.com/web/fundamentals/performance/rendering/debounce-your-input-handlers)