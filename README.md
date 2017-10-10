# phpcms-常用结构

### 导航

```php
<ul>
    <li><a class="<?php if(!$catid) echo 'active'; ?>" href="{siteurl($siteid)}/">首页</a></li>
     {pc:content action="category" catid="0" num="25" siteid="$siteid" order="listorder ASC"}
     {loop $data $r}
        {if $r[catid]==12}
          {pc:content action="category" catid="12" num="1" siteid="$siteid" order="listorder ASC" return="tmp"}
          {loop $tmp $s}
            <li><a class="<?php if($r[catid]==$catid||$r[catid]==$CATEGORYS[$parentid][catid]) echo 'active'; ?>" href="{$s[url]}">{$r[catname]}</a></li>
          {/loop}
          {/pc}
        {else}
           <li><a class="<?php if($r[catid]==$catid||$r[catid]==$CATEGORYS[$parentid][catid]) echo 'active'; ?>" href="{$r[url]}">{$r[catname]}</a></li>
        {/if}
     {/loop}
     {/pc}
</ul>
```

### 导航高亮
```php
{if $top_parentid == $r[catid]||$catid==$r[catid]||$parentid==$r[catid]} active{/if}
```

### 默认子导航链接
```php
{pc:content action="category" catid="0" num="25" siteid="$siteid" order="listorder ASC"}
 {loop $data $r}
{if $r[catid]==9}
  {pc:content action="category" catid="9" num="1" siteid="$siteid" order="listorder ASC" return="tmp"}
  {loop $tmp $s}
    <li class="<?php if($r[catid]==$catid||$r[catid]==$CATEGORYS[$parentid][catid]) echo 'active'; ?>" ><a href="{$s[url]}">{$r[catname]}</a></li>
  {/loop}
  {/pc}
    {elseif $r[catid]==10}
      {pc:content action="category" catid="10" num="1" siteid="$siteid" order="listorder ASC" return="tmp"}
      {loop $tmp $s}
      {pc:content action="category" catid="$s[catid]" num="1" siteid="$siteid" order="listorder ASC" return="tmp"}
      {loop $tmp $s}
	<li class="<?php if($r[catid]==$catid||$r[catid]==$CATEGORYS[$top_parentid][catid]) echo 'active'; ?>" ><a href="{$s[url]}">{$r[catname]}</a></li>
      {/loop}
      {/pc}
      {/loop}
      {/pc}
    {else}
	     <li class="<?php if($r[catid]==$catid||$r[catid]==$CATEGORYS[$parentid][catid]) echo 'active'; ?>"><a href="{$r[url]}">{$r[catname]}</a></li>
    {/if}
 {/loop}
 {/pc}
```

### 循环二三级导航
```php
<ul>  
{pc:content action="category" catid="0" num="12" siteid="$siteid" order="listorder ASC"}  
{loop $data $c}  
<li><a href="{$c['url']}">{$c['catname']}</a>  
    <ul>  
    {loop subcat($c['catid']) $c2}  
        <li><a href="{$c2['url']}">{$c2['catname']}</a>  
            <ul>  
                {loop subcat($c2['catid']) $c3}  
                    <li><a href="{$c2['url']}">{$c3['catname']}</a></li>  
                {/loop}  
            </ul>
         </li>
      {/loop}
    </ul>
</li>
{/loop}  
{/pc}  
</ul>
```

### 面包屑导航

```php
当前位置：首页 &gt; {catpos($catid)}
function catpos($catid, $symbol=' > '){
	$category_arr = array();
	$siteids = getcache('category_content','commons');
	$siteid = $siteids[$catid];
	$category_arr = getcache('category_content_'.$siteid,'commons');
	if(!isset($category_arr[$catid])) return '';
	$pos = '';
	$siteurl = siteurl($category_arr[$catid]['siteid']);
	$arrparentid = array_filter(explode(',', $category_arr[$catid]['arrparentid'].','.$catid));

	$lenght = sizeof($arrparentid);
	$i=1;
	foreach($arrparentid as $catid) {
		$url = $category_arr[$catid]['url'];
		if(strpos($url, '://') === false) $url = $siteurl.$url;
		if($lenght<=$i){
			$pos .= '<span>'.$category_arr[$catid]['catname'].'</span>';
		}else{
			$pos .= $category_arr[$catid]['catname'].$symbol;
		}
		$i++;
	}
	return $pos;
}
```

### 左侧导航
```php
<div class="side-column">
<ul>
{pc:content action="category" catid="$top_parentid" num="6" siteid="$siteid" order="listorder ASC"}
    {if $data}
        {loop $data $r}
        <li {if $catid==$r[catid] || $top_parentid==$r[catid]} class="active"{/if}><a href="{$r[url]}">{$r[catname]}</a></li>
        {/loop}
    {else}
        <li class=""><a href="{$CATEGORYS[$catid][url]}">{$CATEGORYS[$catid][catname]}</a></li>
    {/if}
{/pc}
</ul>
</div>
```

### 单页左侧导航
```
<div class="about-left">
    <div class="about-left-nav">
    	<div class="about-left-list">
			<p class="active">{$CATEGORYS[$parentid][catname]}<i class="second-icon"><img src="/static/images/bottom.png" alt=""></i></p>
		    <ul>
			   {loop $arrchild_arr $cid}
	                <li><a {if $catid==$cid} class="left-nav-active"{/if} href="{$CATEGORYS[$cid][url]}">{$CATEGORYS[$cid][catname]}</a></li>
				{/loop}
		    </ul>
		 </div>
    </div>
</div>
```

### 三四级左侧导航
```php
<div class="about-left">
    <div class="about-left-nav">
    	{pc:content action="category" catid="$top_parentid" num="12" siteid="$siteid" order="listorder ASC"}  
		{loop $data $c}
    	<div class="about-left-list">
	    		<p {if $parentid==$c[catid]} class="active"{/if}>{$c[catname]}<i class="second-icon"><img src="/static/images/bottom.png" alt=""></i></p>
			    <ul>
			    {pc:content action="category" catid="$c[catid]" num="12" siteid="$siteid" order="listorder ASC"}  
				{loop $data $c}
			    		<li><a href="{$c['url']}" {if $catid==$c[catid] || $top_parentid==$c[catid]} class="left-nav-active"{/if}>{$c['catname']}</a></li>
			    {/loop}
			    {/pc}
			    </ul>
		 </div>
	    {/loop}
	    {/pc}
    </div>
</div>

```


### 当前栏目banner

```php
<img class="" src="{$CATEGORYS[$catid][image]}" alt="">
```
### 多图字段调用
```php
{loop string2array($r['images']) $pic_k $v} 
<li><img src="{$v[url]}" alt="{$v[alt]}"></li>   
{/loop}
```

### 解析多附件地址
```php
{php $a = string2array($r[video]);}
{php var_dump($a[0][fileurl])}
```

### 自定义分页
```php
phpcms\libs\functions\global.func.php这个文件，新建wx_pages分页函数
phpcms/libs/classes/template_cache.class.php，找到207行的
$str .= '$pages = pages($'.$op.'_total, $page, $pagesize, $urlrule);';
在这行的下面加上
$str .= '$wz_pages = wz_pages($'.$op.'_total, $page, $pagesize, $urlrule);';

phpcms/libs/classes/template_cache.class.php，找到178行的
$str .= '$r = $get_db->sql_query("'.$sql.'");$s = $get_db->fetch_next();$pages=pages($s[\'count\'], $page, $pagesize, $urlrule);';
在他下面增加：
$str .= '$r = $get_db->sql_query("'.$sql.'");$s = $get_db->fetch_next();$wz_pages=wz_pages($s[\'count\'], $page, $pagesize, $urlrule);';

会员中心的收藏列表分页：
phpcms\libs\classes\model.class.php  61行
$this->pages = pages($this->number, $page, $pagesize, $urlrule, $array, $setpages);
把上面这行修改成：
$this->pages = wz_pages($this->number, $page, $pagesize, $urlrule, $array, $setpages);
(模板中的{$pages}不用改为{$wz_pages})

前台调用：
wx_page="$page"
{$$wz_pages}
```
### SQL查询篇
```php
添加查询条件
{php $sql5 = " pay_type_int = 24"}
{pc:content action="lists" catid="11"   moreinfo="1"  where="$sql5"  num="1" order="listorder asc,inputtime asc" page="$page"  cache="$cachetime" }

同时调用指定栏目和指定推荐位
{pc:get sql="SELECT * FROM v9_house WHERE id IN (SELECT id FROM v9_position_data WHERE posid =2) or catid in(11,12) order by id DESC" num="7"   return="data" }

同时调用两个不同表的相同字段
{pc:get sql="(select id,title,catid,inputtime from v9_house where status = 99) union (select id,title,catid,inputtime from v9_qiugou where status = 99) ORDER BY inputtime DESC limit 7 --" return="data"}


调用需求：文章范围为59 60 61三个栏目，并且推送到了27 和28两个推荐位；
{pc:get sql="SELECT * FROM v9_news WHERE id IN (SELECT id FROM v9_position_data WHERE posid in(27,28) and catid in(59,60,61)) order by listorder DESC" cache="3600" start="3" num="7" return="data" }

PHPCMS V9的get标签调用 

1、调用本系统单条数据，示例（调用ID为1的信息，标题长度不超过25个汉字，显示更新日期）：
{get sql="select * from phpcms_content where contentid=1" /}
标题：{str_cut($r[title], 50)} URL：{$r[url]} 更新日期：{date('Y-m-d', $r[updatetime])} 
2、调用本系统多条数据，示例（调用栏目ID为1通过审核的10条信息，标题长度不超过25个汉字，显示更新日期）：
{get sql="select * from phpcms_content where catid=1 and status=99 order by updatetime desc" rows="10"}
     标题：{str_cut($r[title], 50)} URL：{$r[url]} 更新日期：{date('Y-m-d', $r[updatetime])}
{/get}
3、带分页，示例（调用栏目ID为1通过审核的10条信息，标题长度不超过25个汉字，显示更新日期，带分页）：
{get sql="select * from phpcms_content where catid=1 and status=99 order by updatetime desc" rows="10" page="$page"}
     标题：{str_cut($r[title], 50)} URL：{$r[url]} 更新日期：{date('Y-m-d', $r[updatetime])}
{/get}
分页：{$pages}
4、自定义返回变量，示例（调用栏目ID为1通过审核的10条信息，标题长度不超过25个汉字，显示更新日期，返回变量为 $v）：
{get sql="select * from phpcms_content where catid=1 and status=99 order by updatetime desc" rows="10" return="v"}
     标题：{str_cut($v[title], 50)} URL：{$v[url]} 更新日期：{date('Y-m-d', $v[updatetime])} 
{/get}
5、调用同一帐号下的其他数据库，示例（调用数据库为bbs，分类ID为1的10个最新主题，主题长度不超过25个汉字，显示更新日期）：
{get dbname="bbs" sql="select * from cdb_threads where fid=1 order by dateline desc" rows="10"}
     主题：{str_cut($r[subject], 50)} URL：http://bbs.phpcms.cn/viewthread.php?tid={$r[tid]} 更新日期：{date('Y-m-d', $r[dateline])} 
{/get}
6、调用外部数据，示例（调用数据源为bbs，分类ID为1的10个最新主题，主题长度不超过25个汉字，显示更新日期）：
{get dbsource="bbs" sql="select * from cdb_threads where fid=1 order by dateline desc" rows="10"}
     主题：{str_cut($r[subject], 50)} URL：http://bbs.phpcms.cn/viewthread.php?tid={$r[tid]} 更新日期：{date('Y-m-d', $r[dateline])} 
{/get}


7、当前栏目调用父级及以下栏目信息方法
{php $arrchildid = $CATEGORYS[$CAT[parentid]][arrchildid]}
{pc:get sql="SELECT * FROM v9_news where catid in($arrchildid) cache="3600" page="$page" num="12" return="data"}

不知道有没有朋友在使用中遇到这几种方法不能满足需求的时候呢？
例如：取出评论数最多的第3条到第10条记录，有人说我是多此一举，一般取最多评论没理由不取第一和第二条，因为PHPCMS的缩略图对4：3这样的尺寸比较好，对长条形（如3：4）的图片缩略效果不好，为了自己手动更新评论最多的第一和第二条记录，故此不想自动更新读取前两条。

<ul>
<!--{get sql="select A.contentid,A.catid,A.title,A.thumb,A.description,A.url,A.status,A.updatetime,B.contentid,B.hits,B.comments from `phpcms_content` as A INNER JOIN `phpcms_content_count` as B ON A.contentid=B.contentid and A.status=99 and LENGTH(A.thumb)>0 order by B.comments desc LIMIT 3,7"}-->
<li><span class="xxnse_fen2">{$r[comments]}人参与评论</span>?<a href="{$r[url]}" title="{$r[title]}">{str_cut($r[title], 28)}</a></li>
<!--{/get}-->
</ul>
关键点就在 order by B.comments desc LIMIT 3,7 （表示从第3条记录开始，向下读取7条数据）这个在MYSQL是经常用，我抱着试试看的想法，结果是可以的。
phpcms V9 保留了2008的get标签的使用方法
它包括了2种方式一种是内部数据，另一种是外部数据
我们先分析下内部数据的使用方法
1、内部数据的调用

        {pc:get sql="SELECT * FROM `XX` WHERE  fid =$ltid AND digest =2 AND ifupload =1 ORDER BY tid DESC" num="2" cache= "3600" return="data" } 
        {loop $data $r}
      ｛/loop｝{/pc}
由此可以看出  get 语句支持num的用法但是不支持 limit 5，5.这样的用法
实在是很遗憾
num是调用的条数

2、外部数据的调用
{ pc : get sql = "SELECT * FROM phpcms_member" cache = "3600" page = "$page" dbsource = "discuz" return = "data" }
<ul>
{ loop $data $key $val }
{ $val [ username ]}< br />
{ /loop}
</ ul >
{ $pages }
{/ pc }
一个是数据源，一个是产生的pages翻页效果
```

### phpcms添加全局自定义常量
```php
1:修改网站system.php配置文件
2:修改网站base.php配置文件
3:修改网站setting.tpl.php文件
4:修改网站admin.lang.php文件，添加语言包
5:修改global.func.php文件set_config函数

文件路径：/caches/configs/system.php
'chat_path' => 'tencent://message/?uin=343326675&Site=宜宾微云网络&Menu=yes', //咨询地址

文件路径：/phpcms/base.php
define('CHAT_PATH',pc_base::load_config('system','chat_path'));

文件路径：/phpcms/modules/admin/templates/setting.tpl.php
<tr>
<th width="120"><?php echo L('setting_chat_path')?></th>
<td class="y-bg"><input type="text" class="input-text" name="setconfig[chat_path]" id="chat_path" size="50" value="<?php echo $chat_path?>" /></td>
</tr>

文件路径：/phpcms/languages/zh-cn/admin.lang.php
$LANG['setting_chat_path'] = '咨询地址';

文件路径：phpcms/modules/admin/functions/global.func.php
if(in_array($k,array('js_path','css_path','img_path','chat_path',))) {
```


### 站点管理 自定义变量 增加备案等字段
```php
打开\phpcms\languages\zh-cn\admin.lang.php 
PHPCMS的中文语言定义文件。
查找“site_management”大概在505行，在上面新建一行。
加入新建字段的名称
$LANG['contacts'] = 'Contacts'; //联系方式
$LANG['contacts_address'] = 'Address';//地址
同样打开\phpcms\languages\en\admin.lang.php 
加入英文名称。

修改后台模板文件
打开\phpcms\modules\admin\templates\site_add.tpl.php
搜索“seo_configuration”在“<div class="bk15"></div>”下面新建一行
复制以下内容
<div class="bk15"></div>
<fieldset>
<legend><?php echo L('contacts')?></legend>
<table width="100%"  class="table_form">
  <tr>
    <th width="80"><?php echo L('contacts_address')?>：</th>
    <td class="y-bg"><input type="text" class="input-text" name="contacts_address" id="contacts_address" size="30" /></td>
  </tr>
</table>
</fieldset>
3
同样打开 站点信息修改页面\phpcms\modules\admin\templates\site_edit.tpl.php
加入上一步添加的字段。

打开后台站点信息修改文件
\phpcms\modules\admin\site.php
查找“add()”
查找“$default_style”
在下面新建一行，加入字段获取代码：
$contacts_address = isset($_POST['contacts_address']) && trim($_POST['contacts_address']) ? trim($_POST['contacts_address']) : '';
查找“=>$default_style”在后面加入'contacts_address'=>$contacts_address)
同样的在"edit()”函数里面
加入更新字段的代码
然后在修改数据库
打开数据表
v9_site
在数据表结构新建以下字段
contacts_address varchar(100)
```
