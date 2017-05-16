# NotePad
这次期中作业我完成了基本功能,增加时间项以及搜索功能,UI简单的美化以及增加了一个修改背景色的功能。</br>
2.首先是增加时间的过程:</br>
因为我看到数据库一开始就有了创建时间(COLUMN_NAME_CREATE_DATE)和修改时间(COLUMN_NAME_MODIFICATION_DATE)这两个字段。但是他们都是长整形的,这样我们就需要把他们转化为时间字符串，我这里是直接在创建数据库的时候就修改了字段。</br>
public void onCreate(SQLiteDatabase db) {</br>
           db.execSQL("CREATE TABLE " + NotePad.Notes.TABLE_NAME + " ("</br>
                   + NotePad.Notes._ID + " INTEGER PRIMARY KEY,"</br>
                   + NotePad.Notes.COLUMN_NAME_TITLE + " TEXT,"</br>
                   + NotePad.Notes.COLUMN_NAME_NOTE + " TEXT,"</br>
                   + NotePad.Notes.COLUMN_NAME_CREATE_DATE + " TEXT,"</br>
                   + NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE + " TEXT"</br>
                   + ");");</br>
       }</br>
这里我直接在生成数据库的时候就定义成text字符串类型的数据。</br>
![image](https://github.com/xx12138/NotePad-xwk/blob/master/images/1.png)</br>
![image](https://github.com/xx12138/NotePad-xwk/blob/master/images/2.png)</br>
![image](https://github.com/xx12138/NotePad-xwk/blob/master/images/3.png)</br>
![image](https://github.com/xx12138/NotePad-xwk/blob/master/images/4.png)</br>
![image](https://github.com/xx12138/NotePad-xwk/blob/master/images/5.png)</br>
![image](https://github.com/xx12138/NotePad-xwk/blob/master/images/6.png)</br>
![image](https://github.com/xx12138/NotePad-xwk/blob/master/images/7.png)</br>
![image](https://github.com/xx12138/NotePad-xwk/blob/master/images/8.png)</br>
![image](https://github.com/xx12138/NotePad-xwk/blob/master/images/9.png)</br>
