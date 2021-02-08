#  python脚本-base64大写转换
## 前言
** 在无聊刷到一道关于ctf简单解密题的时候，发现该题目的key是解密需要用到脚本进行爆破解码**
## 正文
**该题的key是：AGV5IULSB3ZLVSE=   ，拿到base64在线解码后发现是乱码。**

**一看很像是经过base64编码后的字符串，但是base64编码过的字符串中是包含有小写字母的字符串。这个key却是没有，可能是把小写字母转大写了，最后经过python爆破小写再base64解码之后，成功解码出来。**

## python 脚本
**代码**
> 
> 	import base64
> 
> """
>     base64 编码是由字母大小写组成的，AGV5IULSB3ZLVSE= 明显是base64 编码之后再把字母全部转为大写字母
> """
> base_list="AGV5IULSB3ZLVSE="
> base_list=list(base_list)
> #函数，递归 排列组合每一种可能性
> def fun(a,b):
>     #终止条件
>     if a==b:
>         return ;
>     #循环组合，分割为子问题
>     for i in range(a,b):
>         if base_list[i]>= 'A' and base_list[i]<='Z':  
>             #大写转小写        
>             base_list[i]=chr(ord(base_list[i])+32)
>             base_str="".join(base_list)  
>             #python 不支持base64 解码为乱码，当base64 解码后str中有乱码，会报错     
>             try:
>                 key=base64.b64decode(base_str.encode('utf-8')).decode('utf-8')
>                 print(key)
>             # 忽略
>             except:
>                 pass
>             #递归调用
>             fun(i+1,b)
>             #还原现场
>             base_list[i]=chr(ord(base_list[i])-32)  
> fun(0,len(base_list))
