# gomodule

a go generic proposal

# demo

	// pkg.go

	package pkg

	module List (EType) {
		type List struct {
			head *Element
			tail *Element
			len  int
		}
		
		type Element struct {
			E    EType
			prev *Element
			next *Element
			list *List
		}
		
		type Iter struct {
			cursor *Element
		}
		
		func (l *List) Append(v EType) *Element {
			// ...
		}
		
		func (l *List) Remove(e *Element) bool {
			// ...
		}
		
		func (l *List) Iter() *Iter {
		}
		
		func (i *Iter) HasMore() bool {
		}
		
		func (i *Iter) Next() *Element {
		}
	}

	module Sort (EType)(
		EType in [int, string]
	) {
		func SortSlice(slc []EType) {
			// ...
		}
		
		module list = List(EType)
		func SortList(l *list.List) {
			// ...
		}
	}

	module Channel (EType) {
		func Merge(chs ...[]chan<- EType) <-chan EType {
			// ...
		}
	}

	module ConvertWithFunc (T1, T2) {
		func ConvertSlice(t1s []T1, f func(T1)T2) []T2 {
			t2s := make([]T2, len(t1s))
			for i, t1 := range t1s {
				t2s[i] = f(t1)
			}
			return t2s
		}
	}

	module Convert (T1, T2)(
		T1 -> T2, // T1 is convertable to T2
	) {
		func ConvertSlice(t1s []T1) []T2 {
			t2s := make([]T2, len(t1s))
			for i, t1 := range t1s {
				t2s[i] = T2(t1)
			}
			return t2s
		}
	}

	module Assertion (I, T)(
		I is interface,
		T implements I,
	) {
		func AssertSlice(is []I) []T {
			ts := make([]T, len(is))
			for k, i := range is {
				ts[k] = i.(T)
			}
			return ts
		}
	}

	module String (T)(
		T implements interface{String() string},
	) {
		func ToStrings(ts []T) []string {
			ss := make([]T, len(ts))
			for i, t := range ts {
				ss[i] = t.String()
			}
			return ss
		}
	}

	moudle Range (CType)(
		CType is [map, slice, *array],
		CType.ElementType is numeric,
	) {
		func DoubleAll(c CType) {
			for i := range c {
				c[i] = 2 * c[i]
			}
		}
	}

	==============================

	// main.go

	package main

	import "pkg"

	func f1() {
		module list = pkg.List(Int)
		
		var il ntList.List
		e := il.Append(1)
		il.Remove(e)
		i := il.Iter()
		for i.HasMore() {
			e = i.Next()
		}
		
		mobule sort = pkg.Sort(*)
		
		sort.SortList(il)
	}

	func f2(values []string) {
		mobule sort = pkg.Sort(*)
		
		sort.SortSlice(values)
	}

	func f3(chs ...[]chan int) <-chan int {
		module channel = pkg.Channel(*)
		
		return channel.Merge(chs...)
	}

	func f4(params []string) []interface{} {
		module convert = pkg.Convert(*, *})
		
		return convert.ConvertSlice(params)
	}

	func f5(values []interface{}) []string {
		module assertion = pkg.Assertion(*, *)
		
		return assertion.AssertSlice(values)
	}

	func f6(s []int, m map[string]float64, a *[100]complex128) {
		module range = pkg.Range(*)
		
		range.DoubleAll(s)
		range.DoubleAll(m)
		range.DoubleAll(a)
	}

	func main() {
	}
