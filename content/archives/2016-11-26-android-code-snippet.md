---
title: Java/Android代码片段
date: 2016-11-26 16:12:25
tags: ["java","android"]
categories: ["java","android"]
---

> 持续更新...  

## 目录(Java)
>  
1、翻页的开始索引和结束索引计算    
2、由总记录数和页大小计算页数量    


## 目录(Android)
>  
1、随意拖动一个View  
2、修复 ScrollView 嵌套 ListView 数据显示不全  
3、修复 ScrollView 嵌套 ListView 显示焦点在 ListView  


## Java  
### 1、翻页的开始索引和结束索引计算  

```  
int begin = (pageNo - 1) * pageSize + 1;
int end = pageNo * pageSize;
```  

### 2、由总记录数和页大小计算页数量  

```  
pageCount = (int)Math.ceil((double) dataSize / (double) pageSize);
```  

---  

## Android  

### 1、随意拖动一个View  

```  
public class MainActivity extends AppCompatActivity {
    private Button mButton = null;
    private int mLastX, mLastY;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        mButton = (Button) findViewById(R.id.button);

        mButton.setOnTouchListener(new View.OnTouchListener() {
            @Override
            public boolean onTouch(View view, MotionEvent motionEvent) {
                int x = (int) motionEvent.getX();
                int y = (int) motionEvent.getY();

                if(MotionEvent.ACTION_DOWN == motionEvent.getAction()){
                    mLastX = x;
                    mLastY = y;
                    return false;
                }
                if(MotionEvent.ACTION_MOVE == motionEvent.getAction()){
                    int offsetX = x - mLastX;
                    int offsetY = y -mLastY;
                    mButton.layout(mButton.getLeft() + offsetX , mButton.getTop() + offsetY, mButton.getRight() + offsetX , mButton.getBottom()+offsetY);
                }

                return false;
            }
        });
    }
}
```  

### 2、修复 ScrollView 嵌套 ListView 数据显示不全  

```  
ListView listView = ...
ArrayList<Map<String, String>> dataList = ...
int size = dataList.size();
SimpleAdapter simpleAdapter = new SimpleAdapter(...);
listView.setAdapter(simpleAdapter);
int totalHeight = 0;
for (int i = 0; i < size; i++) {
    View listItem = simpleAdapter.getView(i, null, listView);
    listItem.measure(0, 0);
    totalHeight += listItem.getMeasuredHeight();
}
ViewGroup.LayoutParams params = listView.getLayoutParams();
params.height = totalHeight + (listView.getDividerHeight() * (size - 1));
listView.setLayoutParams(params);
```  

### 3、修复 ScrollView 嵌套 ListView 显示焦点在 ListView  

```  
//设置 ListView 不可用获取焦点即可
listView.setFocusable(false);
```  
