# template
Agile Template是一个JS模板引擎框架。

##用法：
1.下载或者克隆git之后，将dist目录下的template.js文件加入到web工程中。<br/>
2.在html页面引入template.js文件。<br/>
3.在html页面中使用
```javascript
/**
 * 模板引擎入口
 * @method template
 * @param  {String} str   模板id||模板的唯一标识||模板内容
 * @param  {Object} data  注入模板的json数据
 * @return {String} 模板渲染后的HTML代码片段
 */
var html = template(str, data);
```
即可将模板用json数据渲染为一个html代码片段。

参数str的语法类似于JSP，动态代码包裹在<%和%>中间，动态代码可以为JS的基本语法，采用/**/进行代码注释。

## str有三种形式

### 形式一（模板id）：
需要事先通过script标签指定模板，标签必须设置id和type="text/template"，比如：
```html
<script type="text/template" id="demo_script">
<div id="tag_cloud">
<%
for(var i=0,len=tagCloud.length;i<len;i++){
	var ctx = tagCloud[i];
%>
	/*原样输出为*/
	<a href='#' class='tag_item<% if(ctx['is_selected']){ %> selected<%}%>' title='<%=ctx['title']%>'><%=ttt%><%=#add(ctx['text'])%></a><br/>
<%
}
%>
</div>
</script>
```
代码中可通过script的id引用，如：
```javascript
var html = template('tag_cloud', {
    tagCloud : [
		{is_selected:true, title:'Agile Lite框架', text:'<b>MVP模式</b>'},
		{is_selected:false, title:'Agile VM框架', text:'MVVM模式'},
		{is_selected:null, title:'Agile ExMobi框架', text:'MVP框架'}
	],
	text : '这是共享数据',
	text1 : '测试数据'
});
```
### 形式二（模板的唯一标识）：
需要事先通过template.setter(id, content);来设定模板唯一标识。<br/>
其中id为模板的唯一标识，不可与script模板活着其他模板标识一样；content为模板内容。<br/>
一般用于JS中预置模板，提高模板读取速度。<br/>
比如：
```javascript
template.setter('newtemplate', "<div id='tag_cloud'><%for(var i=0,len=tagCloud.length;i<len;i++){var ctx = tagCloud[i];%><a href='#' class='tag_item<% if(ctx['is_selected']){ %> selected<%}%>' title='<%=ctx['title']%>'><%=ttt%><%=#add(ctx['text'])%></a><br/><%}%></div>");
var html = template('newtemplate', {
    tagCloud : [
		{is_selected:true, title:'Agile Lite框架', text:'<b>MVP模式</b>'},
		{is_selected:false, title:'Agile VM框架', text:'MVVM模式'},
		{is_selected:null, title:'Agile ExMobi框架', text:'MVP框架'}
	],
	text : '这是共享数据',
	text1 : '测试数据'
});
```

### 形式三（模板内容）：
直接使用模板内容字符串，一般不建议这么使用。<br/>
比如：
```javascript
var html = template("<div id='tag_cloud'><%for(var i=0,len=tagCloud.length;i<len;i++){var ctx = tagCloud[i];%><a href='#' class='tag_item<% if(ctx['is_selected']){ %> selected<%}%>' title='<%=ctx['title']%>'><%=ttt%><%=#add(ctx['text'])%></a><br/><%}%></div>", {
    tagCloud : [
		{is_selected:true, title:'Agile Lite框架', text:'<b>MVP模式</b>'},
		{is_selected:false, title:'Agile VM框架', text:'MVVM模式'},
		{is_selected:null, title:'Agile ExMobi框架', text:'MVP框架'}
	],
	text : '这是共享数据',
	text1 : '测试数据'
});
```

如何为模板添加自定义函数？<br/>
可以通过template.helper(funcName, funcHandler);方法添加自定义函数.<br/>
其中funcName为在模板中调用方法使用的方法名；funcHandler为调用此方法时的处理函数，函数接受的参数取决于调用函数的时候的传参。<br/>
比如：
```javascript
template.helper('add', function(str){
	return str+'[追加的数据]';
});
```