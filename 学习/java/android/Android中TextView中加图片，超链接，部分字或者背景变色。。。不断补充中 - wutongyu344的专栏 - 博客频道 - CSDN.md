
对一个TextView中添加图片或者本身文字颜色或者背景色变化的demo:

布局： Android :drawableLeft="@drawable/tv_img"，android:drawableTop="@drawable/tv_img"，android:drawableRight="@drawable/tv_img" ，android:drawableBottom="@drawable/tv_img" 可以添加上下左右的图片

<TextView android:id="@+id/tv_01" 
android:layout_width="fill_parent"
android:layout_height="wrap_content" 
android:text="上下左右有图片"
android:textSize="15dip"
android:drawableLeft="@drawable/tv_img" 
android:drawableTop="@drawable/tv_img"
android:drawableRight="@drawable/tv_img" 
android:drawableBottom="@drawable/tv_img"
android:background="#545454" />





在 Java 代码中的加上下左右图片的方法：

/**上下左右有图片*/
TextView tv01 = (TextView)findViewById(R.id.tv_01);
tv01 .setCompoundDrawablesWithIntrinsicBounds( getResources().getDrawable(R.drawable.tv_img01),  
getResources().getDrawable(R.drawable.tv_img02), 
getResources().getDrawable(R.drawable.tv_img03), 
getResources().getDrawable(R.drawable.tv_img04));





截图：






二、部分字体有颜色或者背景色

布局：

<TextView android:id="@+id/tv_02" 
android:layout_width="fill_parent"
android:layout_height="wrap_content"
android:background="#f5f5f5" 
android:layout_below="@+id/tv_01"
android:textSize="23dip"/>





/**部分字体有颜色或者背景色*/
TextView tv02 = (TextView)findViewById(R.id.tv_02);
int start =3;      
int end = 5; 
SpannableStringBuilder style=new SpannableStringBuilder("中间部分字体变色部分有背景色");      
   style.setSpan(new BackgroundColorSpan( getResources().getColor(R.color.mycolor)),start,end,Spannable.SPAN_EXCLUSIVE_EXCLUSIVE);      
   style.setSpan(new ForegroundColorSpan(Color.RED),7,9,Spannable.SPAN_EXCLUSIVE_INCLUSIVE);      
   tv02.setText(style);  





结果：




三、在textView中添加本地Drawable文件夹中的图片

<TextView android:id="@+id/tv_03"
android:layout_width="fill_parent"
android:layout_height="wrap_content" 
android:text="本地图片"
android:textColor="#ffffff"
android:layout_below="@+id/tv_02"
android:background="#A2C762" />





 /**文字前后添加本地图片*/
   TextView tv03 = (TextView)findViewById(R.id.tv_03);
   ImageGetter imageGetter = new ImageGetter() {    
       @Override  
      public Drawable getDrawable(String source) { 
      int id = Integer.parseInt(source);  
 
     //根据id从资源文件中获取图片对象  
      Drawable d = getResources().getDrawable(id);  
      d.setBounds(0, 0, d.getIntrinsicWidth(),d.getIntrinsicHeight());  
       return d;  
      }  
      };  
   
      tv03.append (Html.fromHtml ("图片前文字   <img src=\""+R.drawable.tv_img_small+"\"> 图片后面文字"
, imageGetter, null) );
       


结果：







四、在TextView 中添加网络获取的图片

<TextView android:id="@+id/tv_04" 
android:layout_width="fill_parent"
android:layout_height="wrap_content"
android:background="#f5f5f5" 
android:layout_below="@+id/tv_03"
android:text="网络图片"
android:textSize="23dip"
android:textColor="#000000"/>





  /**文字前后添加网络图片*/
      String url="http://wiki.huihoo.com/images/2/28/Android-Robot.jpg";
  TextView tv04 = (TextView)findViewById(R.id.tv_04);
   

  ImageGetter imgGetter2 = new Html.ImageGetter() {
public Drawable getDrawable(String source) {
Drawable drawable = null;
URL url;
try {
url = new URL(source);
drawable = Drawable.createFromStream(url.openStream(), ""); 
} catch (Exception e) {
return null;
}

drawable.setBounds(0, 0, drawable.getIntrinsicWidth(), drawable.getIntrinsicHeight());
return drawable;
}
};

tv04. append ( Html.fromHtml ("网络下载的图片加上空格防止不"

 +"到换行的时候字就因为图片的原因被换行        <img src=\""+url+"\"> 此处也可以加文字",imgGetter2, null) );

结果：










这里用的是append 追加到后面，用setText也可以，自己按照需要来。







五、超链接 ：

超链接参考如下：

http://blog.csdn.net/caiyunfreedom/article/details/6763834















