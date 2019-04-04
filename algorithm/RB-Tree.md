# 红黑树
---
## 一、特性
* 每个节点非红即黑
* 根节点是黑色
* 每个叶子节点（Nil）是黑色
* 如果一个节点是红色，则它的子节点必须是黑色
* 从一个节点到该节点的子孙节点的所有路径上包含相同数目的黑色节点

## 二、时间复杂度：O(lgn)

## 三、左旋
![](https://github.com/c-agam/notes/blob/master/images/%E5%B7%A6%E6%97%8B.png)

**对x进行左旋，意味着将x变成一个左节点**

**伪代码：**
```
LEFT-ROTATE(T, x)  
 y ← right[x]            
 right[x] ← left[y]      
 p[left[y]] ← x          
 p[y] ← p[x]             
 if p[x] = nil[T]       
 then root[T] ← y                 
 else if x = left[p[x]]  
           then left[p[x]] ← y    
           else right[p[x]] ← y   
 left[y] ← x             
 p[x] ← y                
```

## 四、右旋
![](https://github.com/c-agam/notes/blob/master/images/%E5%8F%B3%E6%97%8B.png)

**对节点Y右旋，意味着将Y变成右节点**

**伪代码：**
```
RIGHT-ROTATE(T, y)  
 x ← left[y]             
 left[y] ← right[x]      
 p[right[x]] ← y         
 p[x] ← p[y]             
 if p[y] = nil[T]       
 then root[T] ← x                 
 else if y = right[p[y]]  
           then right[p[y]] ← x   
           else left[p[y]] ← x    
 right[x] ← y            
 p[y] ← x               
```
