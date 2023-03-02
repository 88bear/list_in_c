# list_in_c

## list 又稱作 `鏈結串列` Linked List

```c
#include <stdio.h>
#include <stdlib.h>    // malloc、free

/** 宣告結構:
 *  struct XXX 是結構型態名稱
 *  NODE 是重新定義的型態名稱
 **/
typedef struct XXX NODE;
struct XXX {
    int value;
    NODE *ptr;
};

int main() {
    NODE *head,*p,*x;
    int n;

    head=NULL;
    while(EOF!=scanf("%d",&n)) {
        if(n>0) { //新增節點
            x=malloc(sizeof(NODE));
            x->value=n; // (*x).value 寫做 x->value 
            if(!head) { //第一個節點
                x->ptr=NULL;
                head=x;
            } else if(head->value > n) { //比第一個節點小
                x->ptr=head;
                head=x;
            } else {
                p=head;
                while(p->ptr) { //如果還有下一個
                    if(p->ptr->value > n) break; //如果下一個比較大就離開
                    p=p->ptr;   //繼續往下一個
                }
                x->ptr=p->ptr;
                p->ptr=x;
            }
        } else if(n<0) { //刪除節點
            x=NULL; n*=-1;
            if(!head) continue;
            if(head->value==n) { //刪除第一個
                x=head;
                head=x->ptr;
            } else {
                p=head;
                while(p->ptr) {
                    if(p->ptr->value==n) {
                        x=p->ptr;
                        p->ptr=x->ptr;
                        break;
                    }
                    p=p->ptr;
                }
            }
            if(x) free(x);
        } else { //全部刪除後離開
            //從前面刪除
            while(head) {
                p=head;
                head=head->ptr;
                free(p);
            }
            break;
        }
        //顯示串列全部內容
        p=head;
        while(p) {
            printf("%d->",p->value);
            p=p->ptr;
        }
    }
    return 0;
}

```
----------
## 當你讀懂了，會發現上面有很多行可以寫成自訂函式的型態以減少行數</br>所以可以寫成下面的樣子
```c
#include <stdio.h>
#include <stdlib.h>    // malloc、free

typedef struct XXX NODE;
struct XXX {
    int value;
    NODE *ptr;
};

//注意雙重指標的用法
void addNode(NODE **head,int n) {
    NODE *p,*x;

    p=*head;
    x=malloc(sizeof(NODE));
    x->value=n;
    if(!p) { //第一個節點
        x->ptr=NULL;
        *head=x; //重點
    } else if(p->value > n) { //比第一個節點小
        x->ptr=p;
        *head=x; //重點
    } else {
        while(p->ptr) { //如果還有下一個
            if(p->ptr->value > n) break; //如果下一個比較大就離開
            p=p->ptr;   //繼續往下一個
        }
        x->ptr=p->ptr;
        p->ptr=x;
    }
}

//注意雙重指標的用法
void delNode(NODE **head,int n) {
    NODE *p,*del=NULL;
    
    p=*head;
    if(!p) return;
    if(p->value==n) { //刪除第一個
        del=p;
        *head=del->ptr; //重點
    } else {
        while(p->ptr) {
            if(p->ptr->value == n) {
                del=p->ptr;
                p->ptr=del->ptr;
                break;
            }
            p=p->ptr;
        }
    }
    if(del) free(del);
}

//用遞迴的方式從後面刪除
void freeAll(NODE *p) {
    if(p->ptr) freeAll(p->ptr);
    free(p);
}

void show(NODE *p) {
    while(p) {
        printf("%d->",p->value);
        p=p->ptr;
    }
}

int main() {
    int n;
    NODE *head=NULL;
    while(EOF!=scanf("%d",&n)) {
        if(n>0) { //新增節點
            addNode(&head,n);
        } else if(n<0) { //刪除節點
            delNode(&head,-1*n);
        } else { //全部刪除後離開
            freeAll(head); //從後面刪除
            break;
        }
        show(head);
    }
    return 0;
}
```
