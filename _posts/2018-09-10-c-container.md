---
title:  "C Container"
categories: programming
tags:
  - c
  - container
  - queue
  - c queue
comments: true
---
가끔 C++이 Support 되지 않는 machine(보통 old unix machine)에서 C로 작업할
때 container 형식의 queue를 써야 할 때가 있습니다. 퍼포먼스가 Critical한 작업이
아니면 간단히 아래와 같은 형태로 짜서 사용할 수 있습니다.

**header**
```cpp
/*******************************************************************************
*  File        : LaZyContainer.h
*  Description : LaZy Queue
*  Written     : 2010.02.16
*  Version     : 1.2
*  Author      : Hansdev
*  Contacts    : hansdev.kr@gmail.com, https://hansdev.kr
*******************************************************************************/
#ifndef _LAZY_QUEUE_H_
#define _LAZY_QUEUE_H_

#include <assert.h>

#define QUEUE_INCREASE_SIZE 20

#ifdef __cplusplus
extern "C" {
#endif

struct lazy_container_interface;

typedef struct tag_lazyContainer
{
    int quantity;
    int next;
    void** storage;
    struct lazy_container_interface* lpVtbl;
} _lazyContainer;


struct lazy_container_interface
{
    int (*add)(_lazyContainer*, void*);
    void* (*fetch)(_lazyContainer*, int);
    int (*size)(_lazyContainer*);
    void (*inflate)(_lazyContainer*, int);
    void* (*remove)(_lazyContainer*, int);
};

void lazyContainer_initialize(_lazyContainer* container);
void lazyContainer_cleanup(_lazyContainer* container);
int lazyContainer_add(_lazyContainer* container, void* element);
void* lazyContainer_fetch(_lazyContainer* container, int index);
int lazyContainer_size(_lazyContainer* container);
void lazyContainer_inflate(_lazyContainer* container, int increase);
void* lazyContainer_remove(_lazyContainer* container, int index);

static struct lazy_container_interface container_method_tbl =
{
    lazyContainer_add,
    lazyContainer_fetch,
    lazyContainer_size,
    lazyContainer_inflate,
    lazyContainer_remove
};

_lazyContainer* newContainer();
void deleteContainer(_lazyContainer* lazyContainer);

#ifdef __cplusplus
}
#endif /* #ifdef __cplusplus */

#endif /* #ifndef _LAZY_QUEUE_H_ */[-] Collapse
```

**source**
```cpp
/*******************************************************************************
*  File        : LaZyContainer.c
*  Description : LaZy Queue
*  Written     : 2010.02.16
*  Version     : 1.2
*  Author      : Hansdev
*  Contacts    : hansdev.kr@gmail.com, https://hansdev.kr
*******************************************************************************/
#include <stdio.h>
#include <stdlib.h>
#include <memory.h>
#include "LaZyContainer.h"

void lazyContainer_initialize(_lazyContainer* container)
{
    container->quantity = 0;
    container->storage = 0;
    container->next = 0;
}


int lazyContainer_add(_lazyContainer* container, void* element)
{
    if(container->next >= container->quantity)
        lazyContainer_inflate(container, QUEUE_INCREASE_SIZE);
    container->storage[container->next++] = element;
    return(container->next - 1);
}


void* lazyContainer_fetch(_lazyContainer* container, int index)
{
    assert(0 <= index);
    if(index >= container->next)
        return 0;
    return container->storage[index];
}


int lazyContainer_size(_lazyContainer* container)
{
    return container->next;
}


void* lazyContainer_remove(_lazyContainer* container, int index)
{
    void* v = lazyContainer_fetch(container, index);
    if(v != NULL)
        container->storage[index] = 0;
    return v;
}


void lazyContainer_inflate(_lazyContainer* container, int increase)
{
    int psz = sizeof(void*);
    void** st = (void**)malloc(sizeof(void*) * (container->quantity +
              increase));
    memset(st, 0x00, (container->quantity + increase) * psz);
    memcpy(st, container->storage, container->quantity * psz);
    container->quantity += increase;
    free(container->storage);
    container->storage = st;
}


/* no ownership caller free each pointers */
void lazyContainer_cleanup(_lazyContainer* container)
{
    int i = 0;
    for(i = 0; i < container->next; i++)
        assert(container->storage[i] == 0);
    free(container->storage);
}


_lazyContainer* newContainer()
{
    _lazyContainer* lazyContainer = NULL;
    lazyContainer = (_lazyContainer*)malloc(sizeof(_lazyContainer));
    if(lazyContainer)
    {
        lazyContainer->lpVtbl = &container_method_tbl;
        lazyContainer_initialize(lazyContainer);
        return lazyContainer;
    }
    else
    {
        fprintf(stderr, "%s", "newQueue() failed!!");
        exit(1);
    }
}


void deleteContainer(_lazyContainer* lazyContainer)
{
    lazyContainer_cleanup(lazyContainer);
    free(lazyContainer);
}
```

**test**
```cpp
/*******************************************************************************
*  File        : LaZyContainerTest.c
*  Description : LaZy Queue
*  Written     : 2010.02.16
*  Version     : 1.2
*  Author      : Hansdev
*  Contacts    : hansdev.kr@gmail.com, https://hansdev.kr
*******************************************************************************/
#include <stdio.h>
#include <LaZyContainer.h>

#define MAX_COUNT 2000

typedef struct tag_test
{
  int num;
} _test;

int main()
{
  _lazyContainer* lazyContainer = newContainer();
  int i = 0;
  _test* p_test;

  /* inserting into Queue */
  for(i = 0; i < MAX_COUNT; i++)
  {
     p_test = (_test*)malloc(sizeof(_test));
     p_test->num = i;
     lazyContainer->lpVtbl->add(lazyContainer, p_test);
     p_test = NULL;
  }

  p_test = NULL;
  for(i = 0; i < MAX_COUNT; i++)
  {
    p_test = (_test*)lazyContainer->lpVtbl->fetch(lazyContainer, i);
    printf("%d\n", p_test->num);
  }

  for(i = 0; i < MAX_COUNT; i++)
  {
    p_test = (_test*)lazyContainer->lpVtbl->remove(lazyContainer, i);
    free(p_test);
  }
  getchar();

  deleteContainer(lazyContainer);
  return 0;
}
```
