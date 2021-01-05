# 自定义view简介以及相关方法

1. 自定义View简介

   自定义View可以认为继承自View，系统没有的效果（ImageView,TextView,Button）

    分为两类 ：extends View ,  extends ViewGroup

2. onMeasure()
   ```java
   // 获取宽高的模式
   // 获取前两位
   int widthMode = MeasureSpec.getMode(widthMeasureSpec); 
   int heightMode = MeasureSpec.getMode(heightMeasureSpec);
   // 获取宽高的值 
   // 获取后面30位
   int widthSize = MeasureSpec.getSize(widthMeasureSpec); 
   int heightSize = MeasureSpec.getSize(heightMeasureSpec);
   ```

   ```
   MeasureSpec.AT_MOST : 在布局中指定了wrap_content   
   MeasureSpec.EXACTLY : 在不居中指定了确切的值  100dp   match_parent  fill_parent 
   MeasureSpec.UNSPECIFIED : 尽可能的大,很少能用到，ListView , ScrollView 在测量子布局的时候会用UNSPECIFIED 
   
   ScrollView + ListView 会显示不全问题，
   widthMeasureSpec widthMeasureSpec : 会包含两个信息是一个32位的值，第一个信息是模式：2位   值：30位  
   ```

3. onDraw() 

   ```java
    /**
        * 用于绘制
        *
        * @param canvas
        */
       @Override
       protected void onDraw(Canvas canvas) {
           super.onDraw(canvas);       
           // 画文本    
           canvas.drawText();    
           // 画弧   
           canvas.drawArc();  
           // 画圆    
           canvas.drawCircle();   
       }
   ```

   

4. onTouch()  

   ```java
    /**
        * 处理跟用户交互的，手指触摸等等
        *
        * @param event 事件分发事件拦截
        * @return
        */
       @Override
       public boolean onTouchEvent(MotionEvent event) {
           switch (event.getAction()) {
               case MotionEvent.ACTION_DOWN:
                   // 手指按下            
                   Log.e("TAG", "手指按下");
                   break;
               case MotionEvent.ACTION_MOVE:
                   // 手指移动      
                   Log.e("TAG", "手指移动");
                   break;
               case MotionEvent.ACTION_UP:
                   // 手指抬起            
                   Log.e("TAG", "手指抬起");
                   break;
           }
           return super.onTouchEvent(event);
       }
   ```

   

5. 自定义属性

   自定义属性的方法与使用

   1. 在res下的values下面新建attrs.xml

      ```xml
      <resources>   
          <!--name 自定义View的名字TextView-->  
          <declare-styleable name = "TextView" >    
          <!--name 属性名称 
      		format 格式: 
      		string 文字 
      		color 颜色 
      		dimension 宽高 字体大小 
      		integer 数字 
      		reference 资源（drawable）-->    
          <attr name="text"format="string"/>    
          <attr name="textColor"format="color"/>   
          <attr name="textSize"format="dimension"/>   
          <attr name="maxLength"format="integer"/>     
          <attr name="background"format="reference|color"/>  
          <!--枚举 -->       
          <attr name="inputType">       
          <enum name="number"value="1"/>            
          <enum name="text"value="2"/>            
          <enum name="password"value="3"/>     
          </attr>  
          </declare-styleable>
      </resources>
      ```

      

   2. 在布局中使用

      声明命名空间，然后在自己的自定义View中使用

      ```xml
       xmlns:app="http://schemas.android.com/apk/res-auto"
      ```

      ```xml
      <包全路径   
      app:text="Dy"  <--包自定义属性-->     
      app:textColor="@color/colorAccent"      
      android:layout_width="wrap_content"    
      android:layout_height="wrap_content" />
      ```

   3. 在自定义View中获取属性

      ```java
       // 获取自定义属性   
          TypedArray array = context.obtainStyledAttributes(attrs, R.styleable.TextView);
          mText =array.getString(R.styleable.TextView_text);
          mTextColor =array.getColor(R.styleable.TextView_textColor,mTextColor);      
          mTextSize =array.getDimensionPixelSize(R.styleable.TextView_textSize,mTextSize);
          // 回收    
           array.recycle();
      ```