```python
heapify(i) {
   l ← left(i); r ← right(i);
   largest ← i;
   if (l≤size) {
      if (h[l]>h[i])
         largest ← l;
      if (r≤size ∧ h[r]>h[largest])
         largest ← r;
   }
   if (largest≠i) {
      h.swap(i, largest);
      heapify(largest);
   }
}
```

```python
decrease(v):
	var i := index in heap of v
	var p := parent(i)
	while { (p > 0) && { h[i].priority < h[p].priority} } do {
		h.swap(i, p)		// exchange  contents of h[i] and h[p]
		i := p
		p := parent(i)
	}
```
