# NotePad
# 显示时间戳
# 关键代码
# 在NotePadProvider中的insert方法和NoteEditor中的updateNote方法
Long now = Long.valueOf(System.currentTimeMillis());  
Date date = new Date(now);  
SimpleDateFormat format = new SimpleDateFormat("yyyy.MM.dd HH:mm:ss");  
String dateTime = format.format(date);  
# 在dataColumns，viewIDs中补充时间部分：
String[] dataColumns = { NotePad.Notes.COLUMN_NAME_TITLE ,  NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE } ;    
int[] viewIDs = { android.R.id.text1 , R.id.text1_time };   
![image](https://github.com/2380890390/qizhongzuoye/blob/master/TIM%E5%9B%BE%E7%89%8720190522121758.jpg)    
# 搜索功能
# 搜索图标
<item  
    android:id="@+id/menu_search"  
    android:title="@string/menu_search"  
    android:icon="@android:drawable/ic_search_category_default"  
    android:showAsAction="always">  
</item>  
# NoteList中onOptionsItemSelected方法
case R.id.menu_search:  
    Intent intent = new Intent();  
    intent.setClass(NotesList.this,NoteSearch.class);  
    NotesList.this.startActivity(intent);  
    return true;
# 在layout中新建布局文件note_search.xml
<?xml version="1.0" encoding="utf-8"?>  
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    android:orientation="vertical" android:layout_width="match_parent"  
    android:layout_height="match_parent">  
    <SearchView  
        android:id="@+id/search_view"  
        android:layout_width="match_parent"  
        android:layout_height="wrap_content"  
        android:iconifiedByDefault="false"  
        android:queryHint="输入搜索内容..."  
        android:layout_alignParentTop="true">  
    </SearchView>  
    <ListView  
        android:id="@android:id/list"  
        android:layout_width="match_parent"  
        android:layout_height="wrap_content">  
    </ListView>  
</LinearLayout>  
![image](https://github.com/2380890390/qizhongzuoye/blob/master/TIM%E5%9B%BE%E7%89%8720190522121805.jpg)  

# 导出笔记
在NoteEditor中找到onOptionsItemSelected()方法，在菜单的switch中添加：  
case R.id.menu_output:  
        outputNote();  
        break;  
在NoteEditor中添加函数outputNote()：  
private final void outputNote() {  
        Intent intent = new Intent(null,mUri);  
        intent.setClass(NoteEditor.this,OutputText.class);  
        NoteEditor.this.startActivity(intent);  
    }  
 # 笔记排序
 <item
    android:id="@+id/menu_sort"
    android:title="@string/menu_sort"
    android:icon="@android:drawable/ic_menu_sort_by_size"
    android:showAsAction="always" >
    <menu>
        <item
            android:id="@+id/menu_sort1"
            android:title="@string/menu_sort1"/>
        <item
            android:id="@+id/menu_sort2"
            android:title="@string/menu_sort2"/>
        <item
            android:id="@+id/menu_sort3"
            android:title="@string/menu_sort3"/>
        </menu>
    </item>

在NoteList菜单switch下添加case：
case R.id.menu_sort1:  
        cursor = managedQuery(  
                getIntent().getData(),              
                PROJECTION,                        
                null,                             
                null,                              
                NotePad.Notes._ID     
                );    
        adapter = new MyCursorAdapter(  
                this,  
                R.layout.noteslist_item,  
                cursor,  
                dataColumns,  
                viewIDs  
        );  
        setListAdapter(adapter);  
        return true;  
