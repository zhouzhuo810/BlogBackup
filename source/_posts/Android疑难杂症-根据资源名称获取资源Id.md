---
title: Android疑难杂症-根据资源名称获取资源Id
date: 2017-08-10 21:55:22
	- Android
categories: Android疑难杂症
---

```java
Resources resources = context.getResources();
int id= getResources().getIdentifier("图标名称(不带后缀)", "drawable", "包名");
```

<!-- more -->

### 应用场景

- 用数字图片表示剩余天数

```java
    private void setLeftDays(int days) {
        String dayStr = Integer.toString(days);
        llLeftDays.removeAllViews();
        for (int i=dayStr.length()-1; i>0; i--) {
            int d = (int) (days / (Math.pow(10, i)));
            int s = d%10;
            ImageView iv = new ImageView(context);
            iv.setImageResource(getResources().getIdentifier("image_number"+s, "drawable", "me.zhouzhuo810.test"));
            iv.setLayoutParams(new LinearLayout.LayoutParams(ViewUtil.scaleTextValue(context, 52)
                    ,ViewUtil.scaleTextValue(context, 64)));
            llLeftDays.addView(iv);
        }
        int l = days%10;
        ImageView iv = new ImageView(context);
        iv.setImageResource(getResources().getIdentifier("image_number"+l, "drawable", "me.zhouzhuo810.test"));
        iv.setLayoutParams(new LinearLayout.LayoutParams(ViewUtil.scaleTextValue(context, 52)
                ,ViewUtil.scaleTextValue(context, 64)));
        llLeftDays.addView(iv);
    }
```

### 工具封装

```java
/**
 * 资源文件工具类
 */
public class ResourcesUtils {
	
	private static final String RES_ID = "id";
	private static final String RES_STRING = "string";
	private static final String RES_DRABLE = "drable";
	private static final String RES_LAYOUT = "layout";
	private static final String RES_STYLE = "style";
	private static final String RES_COLOR = "color";
	private static final String RES_DIMEN = "dimen";
	private static final String RES_ANIM = "anim";
	private static final String RES_MENU = "menu";
	
	
	/**
	 * 获取资源文件的id
	 * @param context
	 * @param resName
	 * @return
	 */
	public static int getId(Context context,String resName){
		return getResId(context,resName,RES_ID);
	}
	
	/**
	 * 获取资源文件string的id
	 * @param context
	 * @param resName
	 * @return
	 */
	public static int getStringId(Context context,String resName){
		return getResId(context,resName,RES_STRING);
	}
	
	/**
	 * 获取资源文件drable的id
	 * @param context
	 * @param resName
	 * @return
	 */
	public static int getDrableId(Context context,String resName){
		return getResId(context,resName,RES_DRABLE);
	}
	
	/**
	 * 获取资源文件layout的id
	 * @param context
	 * @param resName
	 * @return
	 */
	public static int getLayoutId(Context context,String resName){
		return getResId(context,resName,RES_LAYOUT);
	}
	
	/**
	 * 获取资源文件style的id
	 * @param context
	 * @param resName
	 * @return
	 */
	public static int getStyleId(Context context,String resName){
		return getResId(context,resName,RES_STYLE);
	}
	
	/**
	 * 获取资源文件color的id
	 * @param context
	 * @param resName
	 * @return
	 */
	public static int getColorId(Context context,String resName){
		return getResId(context,resName,RES_COLOR);
	}
	
	/**
	 * 获取资源文件dimen的id
	 * @param context
	 * @param resName
	 * @return
	 */
	public static int getDimenId(Context context,String resName){
		return getResId(context,resName,RES_DIMEN);
	}
	
	/**
	 * 获取资源文件ainm的id
	 * @param context
	 * @param resName
	 * @return
	 */
	public static int getAnimId(Context context,String resName){
		return getResId(context,resName,RES_ANIM);
	}
	
	/**
	 * 获取资源文件menu的id
	 */
	public static int getMenuId(Context context,String resName){
		return getResId(context,resName,RES_MENU);
	}
	 
	/**
	 * 获取资源文件ID
	 * @param context
	 * @param resName
	 * @param defType
	 * @return
	 */
	public static int getResId(Context context,String resName,String defType){
		return context.getResources().getIdentifier(resName, defType, context.getPackageName());
	}
}

```