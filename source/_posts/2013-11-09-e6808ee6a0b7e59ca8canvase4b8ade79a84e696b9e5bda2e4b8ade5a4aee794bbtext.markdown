---
author: qqwerrd
comments: true
date: 2013-11-09 05:22:10+00:00
layout: post
slug: '%e6%80%8e%e6%a0%b7%e5%9c%a8canvas%e4%b8%ad%e7%9a%84%e6%96%b9%e5%bd%a2%e4%b8%ad%e5%a4%ae%e7%94%bbtext'
title: 怎样在canvas中的方形中央画Text
wordpress_id: 164
categories:
- Android
---

```java
    /**
    *
    * @param canvas The canvas you need to draw on.
    * @param x The X Coordinate from left to right.
    * @param y The Y Coordinate form bottom to top.
    */
    private void drawPopup(Canvas canvas,String num,int x,int y){
    int bottomTriangleHeight = 12;
    int padding = MyUtils.dip2px(mContext,2);
    int marginBottom = MyUtils.dip2px(mContext,5);
    boolean singularNum = (num.length() == 1);
    int sidePadding = MyUtils.dip2px(mContext,singularNum? 8:5);
    int xCoor = xCoordinateList.get(x);
    int yCoor = yCoordinateList.get(yCoordinateList.size()-y)-MyUtils.dip2px(mContext,5);
    
    Paint textPaint = new Paint();
    textPaint.setAntiAlias(true);
    textPaint.setColor(Color.BLACK);
    textPaint.setTextSize(MyUtils.sp2px(mContext, 13));
    textPaint.setStrokeWidth(5);
    textPaint.setTextAlign(Paint.Align.CENTER);
    Paint.FontMetrics fm = new Paint.FontMetrics();
    textPaint.getFontMetrics(fm);
    
    Rect r = new Rect((int)(xCoor-textPaint.measureText(num)/2)-sidePadding,
    (int)(yCoor - textPaint.getTextSize())-bottomTriangleHeight-padding-marginBottom,
    (int)(xCoor + textPaint.measureText(num)/2)+sidePadding,
    yCoor+padding-marginBottom);
    
    NinePatchDrawable popup = (NinePatchDrawable)getResources().
    getDrawable(R.drawable.popup_red);
    popup.setBounds(r);
    popup.draw(canvas);
    
    textPaint.setColor(Color.WHITE);
    canvas.drawText(num, xCoor, yCoor-bottomTriangleHeight-marginBottom, textPaint);
    }
```

被这个折腾死了。。
