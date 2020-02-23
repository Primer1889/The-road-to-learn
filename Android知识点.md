# Android原生动画
## 帧动画
### 1.帧动画
* 连贯的按顺序播放的图片
* `android：oneshot="false" `动画仅仅执行一次还是循环执行
* 实现步骤
  - 创建animation-list xml动画文件
  - 为动画xml文件添加属性drawable图片和动画实践duration (单位是毫秒)
* 具体代码实现
```
ImageView animationImg1 = (ImageView) findViewById(R.id.animation1);
animationImg1.setImageResource(R.drawable.frame_anim1);
AnimationDrawable animationDrawable1 = (AnimationDrawable) animationImg1.getDrawable();
animationDrawable1.start();
```

### 2.补间动画
* 一般是xml文件实现
* 分类
  - alpha 淡入淡出
  - translate 位移
  - scale 缩放大小
  - rotate 旋转
* 实现步骤
  - 在res下创建anim文件夹
  - 创建animation对象
  - 在anim里面创建动画效果的xml文件
  - 调用imageview的`startAnimation(animation);`方法
*代码实现
```
//补间动画
Animation animation = AnimationUtils.loadAnimation(this, R.anim.alph);
//logoImageView.startAnimation(animation);
supportImageView.startAnimation(animation);
```
* 更多补间动画的anim文件自行百度 (比如可以有旋转,透明,缩放) 
 
## 属性动画
* 动画类是ObjectAnimator,继承自ValueAnimator类
* 代码实现 (deom是一个view同时或者顺序执行多个动画,功能大大的)
```
 //属性动画
ObjectAnimator alphaAnim = ObjectAnimator.ofFloat(logoImageView, "alpha", 1.0f,0.8f,0.6f,0.4f,0.2f,0.4f,0.6f,0.8f,1.0f);
ObjectAnimator rotateAnim = ObjectAnimator.ofFloat(logoImageView, "rotation", 0, 360);
AnimatorSet set = new AnimatorSet();
set.playTogether(alphaAnim,rotateAnim);
set.setDuration(8000);
set.start();
```
* 功能多多 (百度知道)

## 传统动画 VS 属性动画的区别
  - 补间动画实际上view没有真正的移动,属性动画才是view真正的移动,体现在补间动画移动之后,点击事件仍然有效
  - 属性动画需要正确停止,否则跳转activity之后没有正确停止就会产生内存泄漏,补间动画不会
  - xml实现的补间动画复用率高
  - 帧动画若使用图片过大会导致内存不足
