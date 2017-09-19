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
