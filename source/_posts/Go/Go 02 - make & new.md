---
title: Go 02 - make & new
date: 2019-09-21 17:06:29
categories: Go
---
# make & new

<!--more-->

## make

> func make(t Type, size ...IntegerType) Type

> The make built-in function allocates and initializes an object of type slice, map, or chan (only). Like new, the first argument is a type, not a value. Unlike new, make's return type is the same as the type of its argument, not a pointer to it. The specification of the result depends on the type:

> Slice: The size specifies the length. The capacity of the slice is
equal to its length. A second integer argument may be provided to
specify a different capacity; it must be no smaller than the
length. For example, make([]int, 0, 10) allocates an underlying array
of size 10 and returns a slice of length 0 and capacity 10 that is
backed by this underlying array.

> Map: An empty map is allocated with enough space to hold the
specified number of elements. The size may be omitted, in which case
a small starting size is allocated.

> Channel: The channel's buffer is initialized with the specified
buffer capacity. If zero, or the size is omitted, the channel is
unbuffered.

make 在 Go 中只能应用于初始化语言的基本类型 slice / map / channel:

三者返回了不同类型的数据结构:

1. slice

	slice := make([]int, 0, 100)
	slice是一个包括了data, cap, len的结构体

    ```go
	func makeslice(et *_type, len, cap int) unsafe.Pointer {
    		mem, overflow := math.MulUintptr(et.size, uintptr(cap))
    		if overflow || mem > maxAlloc || len < 0 || len > cap {
        		mem, overflow := math.MulUintptr(et.size, uintptr(len))
        		if overflow || mem > maxAlloc || len < 0 {
            			panicmakeslicelen()
        		}
        		panicmakeslicecap()
    		}

    		return mallocgc(mem, et, true)
	}
    ```


2. map
   
	hash := make(map[int]bool, 10)
	hash是一个指向hmap结构体的指针

    ```go
	func makemap(t *maptype, hint int, h *hmap) *hmap {
    		mem, overflow := math.MulUintptr(uintptr(hint), t.bucket.size)
    		if overflow || mem > maxAlloc {
        		hint = 0
    		}

	    	if h == nil {
        		h = new(hmap)
    		}
    		h.hash0 = fastrand()

    		B := uint8(0)
    		for overLoadFactor(hint, B) {
        		B++
    		}
    		h.B = B

    		if h.B != 0 {
        		var nextOverflow *bmap
        		h.buckets, nextOverflow = makeBucketArray(t, h.B, nil)
        		if nextOverflow != nil {
            			h.extra = new(mapextra)
            			h.extra.nextOverflow = nextOverflow
        		}
    		}

    		return h
	}
    ```

3. channel

    ch := make(chan int, 5)
    
    ch是一个指向hchan结构体的指针

    ```go
	func makechan(t *chantype, size int) *hchan {
	    elem := t.elem
	   	mem, _ := math.MulUintpt(elem.size, uintptr(size))

	   	var c *hchan
	 	switch {
	   	case mem == 0:
	       	c = (*hchan)(mallocgc(hchanSize, nil, true))
	       	c.buf = c.raceaddr()
	    case elem.kind&kindNoPointers != 0:
	        c = (*hchan)(mallocgc(hchanSize+mem, nil, true))
	       	c.buf = add(unsafe.Pointer(c), hchanSize)
	    default:
	       	c = new(hchan)
	       	c.buf = mallocgc(mem, elem, true)
	   	}

	   	c.elemsize = uint16(elem.size)
	   	c.elemtype = elem
	   	c.dataqsiz = uint(size)
	    return c
	}
    ```

编译期间的类型检查阶段, 就将代表make关键字的OMAKE节点根据参数类型的不同转换成了OMAKESLICE, OMAKEMAP和OMAKECHAN三种不同类型的节点, 这些节点最终也会调用不同的运行时函数来初始化数据.

## new

new会在编译期间经过callnew函数的处理, 如果请求创建的类型大小是0, 就会返回一个表示空指针的zerobase变量, 在遇到其他情况时会将关键字newobject

```go
func callnew(t *types.Type) *Node {
    if t.NotInHeap() {
        yyerror("%v is go:notinheap; heap allocation disallowed", t)
    }
    dowidth(t)

    if t.Size() == 0 {
        z := newname(Runtimepkg.Lookup("zerobase"))
        z.SetClass(PEXTERN)
        z.Type = t
        return typecheck(nod(OADDR, z, nil), ctxExpr)
    }

    fn := syslook("newobject")
    fn = substArgTypes(fn, t)
    v := mkcall1(fn, types.NewPtr(t), nil, typename(t))
    v.SetNonNil(true)
    return v
}
```

需要注意的是, 哪怕当前变量是使用var进行初始化, 这也阶段也可能会被转化成newobject的函数调用并在堆上申请内存. 当然不是绝对的, 如果当前声明的变量或者参数不需要在当前作用域外生存(逃逸分析), 那么就会初始化在函数栈中, 并且伴随着函数调用的结束销毁.

newobject函数的工作就是获取传入类型的大小并调用mallocgc在堆上申请一片大小合适的内存空间并且返回指向这片内存空间的指针:

```go
func newobject(typ* _type) unsafe.Pointer {
    return mallocgc(typ.size, typ, true)
}
```