---
layout: post
title: "通用的List JavaBean转换"
header-img: "img/flower2.jpg"
---
<span class="span">在开发会遇到两个`javaBean`之间的相互转换,单个`javaBean`可以用`apache`提供的`BeanUtils`,如果是`List<Bean>`就需要`foreach`循环做转换.因为这种应用场景较多,不处理好
冗余代码也会变多,所以使用`泛型`和`BeanUtils`写了通用的转换方法.代码如下:</span>
{% highlight ruby %}
public <T,V> List<V> pubCopy(List<T> sourceObj, Class<V> targetObj)
throws InstantiationException, IllegalAccessException,InvocationTargetException{
		List<V> result = new ArrayList<>();
		for (T t : sourceObj) {
			V v = newTclass(targetObj);
			BeanUtils.copyProperties(v, t);
			result.add(v);
		}
		return result;
	}
{% endhighlight %}

{% highlight ruby %}
public <V> V newTclass(Class<V> clazz)
throws InstantiationException, IllegalAccessException{
  		V v = clazz.newInstance();
  		return v;
  	}
{% endhighlight %}
