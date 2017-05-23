# NotePad
这次期中作业我完成了基本功能,增加时间项以及搜索功能,UI简单的美化以及增加了一个修改背景色的功能。</br></br>
1.首先是增加时间的过程:</br>
因为我看到数据库一开始就有了创建时间(COLUMN_NAME_CREATE_DATE)和修改时间(COLUMN_NAME_MODIFICATION_DATE)这两个字段。但是他们都是长整形的,这样我们就需要把他们转化为时间字符串，我这里是直接在创建数据库的时候就修改了字段。</br>
```
public void onCreate(SQLiteDatabase db) {
           db.execSQL("CREATE TABLE " + NotePad.Notes.TABLE_NAME + " ("
                   + NotePad.Notes._ID + " INTEGER PRIMARY KEY,"
                   + NotePad.Notes.COLUMN_NAME_TITLE + " TEXT,"
                   + NotePad.Notes.COLUMN_NAME_NOTE + " TEXT,"
                   + NotePad.Notes.COLUMN_NAME_CREATE_DATE + " TEXT,"//修改了这个地方,把类型变成了text
                   + NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE + " TEXT"
                   + ");");
       }
```
</br>这里我在生成数据库的时候就定义成text字符串类型的数据。</br>
```
SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
String re_StrTime = sdf.format(System.currentTimeMillis());</br>
if (values.containsKey(NotePad.Notes.COLUMN_NAME_CREATE_DATE) == false) {
    values.put(NotePad.Notes.COLUMN_NAME_CREATE_DATE, re_StrTime);
}
```
</br>保存的时候就直接用系统的把时间戳转化为字符串的函数进行转化后保存。</br>
我重新定义了一个textview用来放时间</br>
这和原来的title是放在一个relativeLayout里面的，还有前面的ImageView控件,都放在notelist_item里面。note_list的项的布局大概如下:</br>
```
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/noteList"
    android:layout_width="match_parent"
    android:layout_height="55dp"
    android:orientation="vertical"
    >
    <ImageView
        android:id="@+id/noteicon"
        android:layout_height="40dp"
        android:layout_width="40dp"
        android:src="@drawable/app_notes" />
    <TextView xmlns:android="http://schemas.android.com/apk/res/android"
        android:id="@android:id/text1"
        android:layout_width="match_parent"
        android:layout_height="?android:attr/listPreferredItemHeight"
        android:textAppearance="?android:attr/textAppearanceLarge"
        android:layout_marginTop="5dp"
        android:paddingLeft="5dip"
        android:layout_toRightOf="@+id/noteicon"
        android:singleLine="true"
    />
    <TextView
        android:id="@+id/note_date"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textAppearance="?android:attr/textAppearanceLarge"
        android:layout_alignParentRight="true"
        android:paddingLeft="5dip"
        android:layout_alignParentBottom="true"
        android:textSize="20px"
        android:singleLine="true"
        />
</RelativeLayout>
```
</br>![image](https://github.com/xx12138/NotePad-xwk/blob/master/images/2.png)</br>
我重新定义了一个布局用来显示NoteList，这样的:</br>
```
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/noteLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@android:color/transparent">

    <SearchView
        android:id="@+id/searchview"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="@xml/corners_bg">
    </SearchView>

    <ListView
        android:id="@android:id/list"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:transcriptMode="normal"
        android:layout_below="@+id/searchview"/>

</RelativeLayout>
```
</br>整体的效果是这样的:</br>
![image](https://github.com/xx12138/NotePad-xwk/blob/master/images/1.png)</br></br>

2.接下来是search搜索功能，当我们点击搜索图标的时候就可以输入文字，效果如下:</br>
![image](https://github.com/xx12138/NotePad-xwk/blob/master/images/3.png)</br>
![image](https://github.com/xx12138/NotePad-xwk/blob/master/images/4.png)</br>
![image](https://github.com/xx12138/NotePad-xwk/blob/master/images/5.png)</br>
我使用系统自带的Searchview实现的，代码:</br>
```
final SearchView mSearchView = (SearchView)findViewById(R.id.searchview);
        // 设置搜索文本监听
        mSearchView.setOnQueryTextListener(new SearchView.OnQueryTextListener() {
            // 当点击搜索按钮时触发该方法
            private String TAG = getClass().getSimpleName();

            /*
             * 在输入时触发的方法，当字符真正显示到searchView中才触发，像是拼音，在舒服法组词的时候不会触发
             *
             * @param queryText
             *
             * @return false if the SearchView should perform the default action
             * of showing any suggestions if available, true if the action was
             * handled by the listener.
             */
            @Override
            public boolean onQueryTextChange(String queryText) {
                Log.d(TAG, "onQueryTextChange = " + queryText);
                String selection = NotePad.Notes.COLUMN_NAME_TITLE + " LIKE '%" + queryText + "%' " ;
                // String[] selectionArg = { queryText };
                Cursor cursor = getContentResolver().query(getIntent().getData(), PROJECTION, selection, null, null);
                adapter.swapCursor(cursor); // 交换指针，展示新的数据
                return true;
            }

            @Override
            public boolean onQueryTextSubmit(String queryText) {
                Log.d(TAG, "onQueryTextSubmit = " + queryText);

                if (mSearchView != null) {
                    // 得到输入管理对象
                    InputMethodManager imm = (InputMethodManager) getSystemService(Context.INPUT_METHOD_SERVICE);
                    if (imm != null) {
                        // 这将让键盘在所有的情况下都被隐藏，但是一般我们在点击搜索按钮后，输入法都会乖乖的自动隐藏的。
                        imm.hideSoftInputFromWindow(mSearchView.getWindowToken(), 0); // 输入法如果是显示状态，那么就隐藏输入法
                    }
                    mSearchView.clearFocus(); // 不获取焦点
                }
                return true;
            }
```
</br>先使用like找到我们查询的内容，在改变指针，交换adapter的指针就可以查询数据了。用系统自带的searchview实现搜索功能比较方便。</br>
</br></br>
3.对UI稍微美化了一下，就是为searchview增加一个圆角边框，增加了一个笔记的icon还有listview的隔行变色效果</br>
只要把下面这段代码放在生成cursorAdapter的后面就可以实现隔行变色的效果</br>
```
{
            @Override
            public View getView(int position, View convertView, ViewGroup parent) {
                View view = super.getView(position, convertView, parent);
                //      view.setBackgroundResource(colors[position % 10]);
                if (position % 2 != 0 )
                {
                    //如果注释掉这句，滑动后，所有cell都会变成Color.GRAY
                    //可能是有种缓存机制
                    view.setBackgroundColor(Color.parseColor("#FFF5EE"));
                }
                else
                {
                    //          view.setBackgroundResource(Color.GRAY);
                    view.setBackgroundColor(Color.parseColor("#F5FFFA"));
                }
                return view;
            }
        };
```
</br>整体效果:</br>
![image](https://github.com/xx12138/NotePad-xwk/blob/master/images/1.png)</br>
4.切换背景的功能</br>
我使用一个AlertDialog，用自定义view显示出几个button，首先要定义一个menu，在顶端actionbar上面显示用来弹出切换背景功能的按钮</br>
在onCreateOptionsMenu中注册menu:inflater.inflate(R.menu.changeback_menu, menu);</br>
然后添加点击事件:</br>
```
public boolean onOptionsItemSelected(MenuItem item) {
    switch (item.getItemId()) {
    case R.id.changeback:
    LayoutInflater inflater = getLayoutInflater();
    View layout = inflater.inflate(R.layout.alert_changeback, null);
    final AlertDialog.Builder builder =new AlertDialog.Builder(this);
    builder.setView(layout);
    final AlertDialog aDialog=builder.create();
}
```
</br>最后在为那些颜色按钮添加点击事件就可以了，可以存在数据库里面增加一张表，作为系统配置的，用来保存背景颜色,我在list和edit页面都添加了这个功能</br>
效果:</br>
![image](https://github.com/xx12138/NotePad-xwk/blob/master/images/6.png)</br>
![image](https://github.com/xx12138/NotePad-xwk/blob/master/images/7.png)</br>
![image](https://github.com/xx12138/NotePad-xwk/blob/master/images/8.png)</br>
![image](https://github.com/xx12138/NotePad-xwk/blob/master/images/9.png)</br>
