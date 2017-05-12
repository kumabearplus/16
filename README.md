# 事件的兼容
## IE并不支持addEventListener和removeEventListener方法，而是实现了两个类似的方法
> - attachEvent
- detachEvent

下面是添加事件的兼容函数
```
  function addEvent(node,type,handler){ // type='click load'等、handler为函数
    if (!node) return false
    if (node.addEventListener) {
      node.addEventListener(type,handler,false)
      return true
    } 
    else if (node.attachEvent) {
      node['e'+type+handler] = handler
      node[type+handler] = function(){
        node['e'+type+handler](windows.enent)
      }
      node.attachEvent('on'+type,node[type+handler])
      return true
    } 
    return false
  }
```
下面是删除事件的兼容函数
```
  function removeEvent(node,type,handler){
    if (!node) return false
    if (node.removeEventListener) {
      node.removeEventListener(type,handler,false)
      return true
    } 
    else if (node.detachEvent) { //前面已经赋值，所以，可以去掉
    /*  node['e'+type+handler] = handler
    node[type+handler] = function(){
    node['e'+type+handler](windows.enent)
    }
    */   
      node.detachEvent('on'+type,node[type+handler])
      node[type+handler] = null
    }
    return false									    }
```
