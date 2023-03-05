# list_in_c

## list 又稱作 `鏈結串列` Linked List

### 第一題想有夠久的== 我是程式廢物不配讀書 卡在 ` newNode -> pNext = pList; `一輩子

```c
#include <iostream>
#include "listElement.h"

ListElement* addFront(ListElement* pList, int v){
    ListElement* newNode;
    newNode = (ListElement*)malloc(sizeof(ListElement));
    newNode -> value = v;
    newNode -> pNext = pList;
    return newNode;
}
```
--------------
### 第二題一開始想到的是把現在輸入的值當作temp先暫存起來，再用判斷式來比較要插在前面(prev)還是後面(next)
```c
#include <iostream>
#include "listElement.h"

ListElement* insertToList(ListElement* pList, int v){
    ListElement *temp, *prev, *next;
    temp = (ListElement*)malloc(sizeof(ListElement));
    temp -> value = v;
    temp -> pNext = NULL;
    if(!pList){ //第一個節點
        pList = temp;
    }
    else{
        prev = NULL;
        next = pList;
        while(next && next -> value <= v){
            prev = next;
            next = next -> pNext;
        }
        if(!next){
            prev -> pNext = temp;
        }
        else{
            if(prev){
                temp -> pNext = prev -> pNext;
                prev -> pNext = temp;
            }
            else{
                temp -> pNext = pList;
                pList = temp;
            }
        }
    }
    return pList;
}



```
----------
### 第二次發現可以精簡很多地方

```c

#include <iostream>
#include "listElement.h"

ListElement* insertToList(ListElement* pList, int v){
    ListElement *p, *x;
    p = pList;
    x = (ListElement* )malloc(sizeof(ListElement));
    x -> value = v;
    if(!p){
        x -> pNext = NULL;
        pList = x;
    }
    else if(v < p -> value){ //比第一個節點小
        x -> pNext = p;
        pList = x;
    }
    else{
        while(p -> pNext){ //如果還有下一個
            if(p -> pNext -> value > v) break; //如果下一個比較大就離開
            p = p -> pNext; //繼續往下一個
        }
        x -> pNext = p -> pNext;
        p -> pNext = x;
    }
    return pList;
}

```
