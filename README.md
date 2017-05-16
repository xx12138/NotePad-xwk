# NotePad
这次期中作业我完成了基本功能,增加时间项以及搜索功能,UI简单的美化以及增加了一个修改背景色的功能。</br></br>
1.首先是增加时间的过程:</br>
因为我看到数据库一开始就有了创建时间(COLUMN_NAME_CREATE_DATE)和修改时间(COLUMN_NAME_MODIFICATION_DATE)这两个字段。但是他们都是长整形的,这样我们就需要把他们转化为时间字符串，我这里是直接在创建数据库的时候就修改了字段。</br>
public void onCreate(SQLiteDatabase db) {</br>
           db.execSQL("CREATE TABLE " + NotePad.Notes.TABLE_NAME + " ("</br>
                   + NotePad.Notes._ID + " INTEGER PRIMARY KEY,"</br>
                   + NotePad.Notes.COLUMN_NAME_TITLE + " TEXT,"</br>
                   + NotePad.Notes.COLUMN_NAME_NOTE + " TEXT,"</br>
                   + NotePad.Notes.COLUMN_NAME_CREATE_DATE + " TEXT,"//修改了这个地方,把类型变成了text</br>
                   + NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE + " TEXT"</br>
                   + ");");</br>
       }</br>
这里我在生成数据库的时候就定义成text字符串类型的数据。</br>
SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");</br>
String re_StrTime = sdf.format(System.currentTimeMillis());</br>
if (values.containsKey(NotePad.Notes.COLUMN_NAME_CREATE_DATE) == false) {</br>
    values.put(NotePad.Notes.COLUMN_NAME_CREATE_DATE, re_StrTime);</br>
}</br>
保存的时候就直接用系统的把时间戳转化为字符串的函数进行转化后保存。</br>
我重新定义了一个textview用来放时间</br>
这和原来的title是放在一个relativeLayout里面的，还有前面的ImageView控件,都放在notelist_item里面。note_list布局大概如下:</br>
relativeLayout ImageView图片 TextView标题 TextView时间 relativeLayout效果:</br>
![image](https://github.com/xx12138/NotePad-xwk/blob/master/images/2.png)</br>
我重新定义了一个布局用来显示NoteList，大致是这样的:</br>
RelativeLayout  SearchView 搜索框  ListView笔记列表  RelativeLayout 整体的效果是这样的:</br>
![image](https://github.com/xx12138/NotePad-xwk/blob/master/images/1.png)</br></br>

接下来是search搜索功能，当我们点击搜索图标的时候就可以输入文字，效果如下:</br>
![image](https://github.com/xx12138/NotePad-xwk/blob/master/images/3.png)</br>
![image](https://github.com/xx12138/NotePad-xwk/blob/master/images/4.png)</br>
![image](https://github.com/xx12138/NotePad-xwk/blob/master/images/5.png)</br>
我使用系统自带的Searchview实现的，代码:</br>
final SearchView mSearchView = (SearchView)findViewById(R.id.searchview);</br>
mSearchView.setOnQueryTextListener(new SearchView.OnQueryTextListener()//为搜索框添加监听事件，当文字改变以后触发</br>
当我们修改文字以后，触发下面的事件，动态改变listview的内容，</br>
public boolean onQueryTextChange(String queryText) {</br>
    String selection = NotePad.Notes.COLUMN_NAME_TITLE + " LIKE '%" + queryText + "%' " ;</br>
    Cursor cursor = getContentResolver().query(getIntent().getData(), PROJECTION, selection, null, null);</br>
    adapter.swapCursor(cursor); // 交换指针，展示新的数据</br>
    return true;</br>
}</br>
先使用like找到我们查询的内容，在改变指针，交换adapter的指针就可以查询数据了。用系统自带的searchview实现搜索功能比较方便</br>
</br>
对UI稍微美化了一下，就是为searchview增加一个圆角边框，增加了一个笔记的icon还有listview的隔行变色效果</br>
只要把下面这段代码放在生成cursorAdapter的后面就可以实现隔行变色的效果</br>
{</br>
@Override</br>
public View getView(int position, View convertView, ViewGroup parent) {</br>
View view = super.getView(position, convertView, parent);</br>
    if (position % 2 != 0 )</br>
    {</br>
        view.setBackgroundColor(Color.parseColor("#FFF5EE"));</br>
    }</br>
    else</br>
    {</br>
        view.setBackgroundColor(Color.parseColor("#F5FFFA"));</br>
    }</br>
    return view;</br>
}</br>
整体效果:</br>
![image](https://github.com/xx12138/NotePad-xwk/blob/master/images/1.png)</br>
![image](https://github.com/xx12138/NotePad-xwk/blob/master/images/6.png)</br>
![image](https://github.com/xx12138/NotePad-xwk/blob/master/images/7.png)</br>
![image](https://github.com/xx12138/NotePad-xwk/blob/master/images/8.png)</br>
![image](https://github.com/xx12138/NotePad-xwk/blob/master/images/9.png)</br>
