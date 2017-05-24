# Notepad
# 实验效果
# 实现显示记事本时间戳和查询功能
# 实现增删改功能 界面美化 
#public class DatabaseManage {

	private Context mContext = null;

	private SQLiteDatabase mSQLiteDatabase = null;//用于操作数据库的对象
	private DatabaseHelper dh = null;//用于创建数据库的对象

	private String dbName = "notepad.db";
	private int dbVersion = 1;

	public DatabaseManage(Context context){
		mContext = context;
	}

	/**
	 * 打开数据库
	 */
	public void open(){

		try{
			dh = new DatabaseHelper(mContext, dbName, null, dbVersion);
			if(dh == null){
				Log.v("msg", "is null");
				return ;
			}
			mSQLiteDatabase = dh.getWritableDatabase();
			//dh.onOpen(mSQLiteDatabase);

		}catch(SQLiteException se){
			se.printStackTrace();
		}
	}

	/**
	 * 关闭数据库
	 */
	public void close(){

		mSQLiteDatabase.close();
		dh.close();

	}

	//获取列表
	public Cursor selectAll(){
		Cursor cursor = null;
		try{
			String sql = "select * from record";
			cursor = mSQLiteDatabase.rawQuery(sql, null);
		}catch(Exception ex){
			ex.printStackTrace();
			cursor = null;
		}
		return cursor;
	}

	public Cursor selectById(int id){

		//String result[] = {};
		Cursor cursor = null;
		try{
			String sql = "select * from record where _id='" + id +"'";
			cursor = mSQLiteDatabase.rawQuery(sql, null);
		}catch(Exception ex){
			ex.printStackTrace();
			cursor = null;
		}

		return cursor;
	}

	//插入数据
	public long insert(String title, String content){

		long datetime = System.currentTimeMillis();
		long l = -1;
		try{
			ContentValues cv = new ContentValues();
			cv.put("title", title);
			cv.put("content", content);
			cv.put("time", datetime);
			l = mSQLiteDatabase.insert("record", null, cv);
			//	Log.v("datetime", datetime+""+l);
		}catch(Exception ex){
			ex.printStackTrace();
			l = -1;
		}
		return l;

	}

	//删除数据
	public int delete(long id){
		int affect = 0;
		try{
			affect = mSQLiteDatabase.delete("record", "_id=?", new String[]{id+""});
		}catch(Exception ex){
			ex.printStackTrace();
			affect = -1;
		}

		return affect;
	}

	//修改数据
	public int update(int id, String title, String content){
		int affect = 0;
		try{
			ContentValues cv = new ContentValues();

			cv.put("title", title);
			cv.put("content", content);
			String w[] = {id+""};
			affect = mSQLiteDatabase.update("record", cv, "_id=?", w);
		}catch(Exception ex){
			ex.printStackTrace();
			affect = -1;
		}
		return affect;
	}

}
#/**
	 * 通过该函数显示数据
	 */
	public View getView(int position, View convertView, ViewGroup parent) {
		// TODO Auto-generated method stub

		if(convertView == null){
			convertView = inflater.inflate(R.layout.notepad_list_item,null);
		}

		TextView text = (TextView)convertView.findViewById(R.id.listItem);
		text.setText(listItems.get(position));

		TextView time = (TextView)convertView.findViewById(R.id.listItemTime);
		String datetime = DateFormat.format("yyyy-MM-dd kk:mm:ss",
				Long.parseLong(listItemTimes.get(position))).toString();
		time.setText(datetime);

		return convertView;
	}
//设置滑动监听器
		listView.setOnScrollListener(this);
		listView.setOnCreateContextMenuListener(new myOnCreateContextMenuListener());


		//设置按钮监听器
		addRecordButton.setOnClickListener(new AddRecordListener());//新增
		deleteRecordButton.setOnClickListener(new DeleteRecordListener());//删除
		checkRecordButton.setOnClickListener(new CheckRecordListener());//查看
		modifyRecordButton.setOnClickListener(new ModifyRecordListener());//修改
<img src="https://github.com/15205080581/Notepad/blob/master/imp/1.jpg">
<img src="https://github.com/15205080581/Notepad/blob/master/imp/2.jpg">
<img src="https://github.com/15205080581/Notepad/blob/master/imp/3.jpg">
<img src="https://github.com/15205080581/Notepad/blob/master/imp/4.jpg">
<img src="https://github.com/15205080581/Notepad/blob/master/imp/5.jpg">
<img src="http://img.blog.csdn.net/20170514164543742?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvampiMTk5NDUy/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center">
