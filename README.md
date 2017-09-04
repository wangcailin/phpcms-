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
        {elseif $r[catid]==17}
          {pc:content action="category" catid="17" num="1" siteid="$siteid" order="listorder ASC" return="tmp"}
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
