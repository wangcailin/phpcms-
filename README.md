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
phpcms\libs\functions\global.func.php这个文件，找到分页函数，复制一下，粘贴到默认分页函数的下面，重新命名，比如我的就命名为wz_pages，保存。
打开
phpcms/libs/classes/template_cache.class.php，找到207行的
$str .= '$pages = pages($'.$op.'_total, $page, $pagesize, $urlrule);';
在这行的下面加上
$str .= '$wz_pages = wz_pages($'.$op.'_total, $page, $pagesize, $urlrule);';
保存。

最后，如果你要使用你自定义的分页函数，那么在模板中直接用{$wz_pages}就可以了、、

以后要修改样式，直接修改
phpcms\libs\functions\global.func.php这个文件中的wz_pages函数就可以了，不会影响到后台

打开
phpcms/libs/classes/template_cache.class.php，找到178行的
$str .= '$r = $get_db->sql_query("'.$sql.'");$s = $get_db->fetch_next();$pages=pages($s[\'count\'], $page, $pagesize, $urlrule);';
在他下面增加：
$str .= '$r = $get_db->sql_query("'.$sql.'");$s = $get_db->fetch_next();$wz_pages=wz_pages($s[\'count\'], $page, $pagesize, $urlrule);';

再次后续优化：
有朋友提到，会员中心的收藏列表分页不能使用上面的办法。
因为收藏列表的分页函数直接在其他文件定好了，所以通过修改模板缓存文件是不可行的
下面提供修改办法：
具体是
phpcms\libs\classes\model.class.php  61行
$this->pages = pages($this->number, $page, $pagesize, $urlrule, $array, $setpages);
把上面这行修改成：
$this->pages = wz_pages($this->number, $page, $pagesize, $urlrule, $array, $setpages);
(模板中的{$pages}不用改为{$wz_pages})
上面是第一种办法。
不过这样子的话，可能后台也会影响到了，这个地方好像是控制好多地方的
也可以这样修改，还是上面那个文件，61行不要修改。直接在61行下面添加：
$this->wz_pages= wz_pages($this->number, $page, $pagesize, $urlrule, $array, $setpages);
再去到：phpcms\modules\member\index.php 718行
把 $pages = $this->favorite_db->pages;
修改为：
$wz_pages = $this->favorite_db->wz_pages;
模板中的{$pages}要修改为{$wz_pages}
```
