# Binary Heap giải thích chi tiết & triển khai Priority Queue

Bài viết này đã được viết lại, vui lòng đọc [Nguyên lý cơ bản của Binary Heap](https://labuladong.online/algo/data-structure-basic/binary-heap-basic/) và [Triển khai Priority Queue bằng Binary Heap](https://labuladong.online/algo/data-structure-basic/binary-heap-implement/).





**＿＿＿＿＿＿＿＿＿＿＿＿＿**

**《Algorithm Notes của labuladong》 đã xuất bản, follow tài khoản WeChat chính thức để xem chi tiết; reply「**全家桶**」 qua tin nhắn để tải PDF kèm bộ luyện bài 全家桶**:

![](https://labuladong.online/algo/images/souyisou2.png)

======Code các ngôn ngữ khác======

### javascript

```js
/**
 * Max-heap
 */
function left(i) {
  return i * 2 + 1;
}

function right(i) {
  return i * 2 + 2;
}

function swap(A, i, j) {
  const t = A[i];
  A[i] = A[j];
  A[j] = t;
}

class Heap {
  constructor(arr) {
    this.data = [...arr];
    this.size = this.data.length;
  }

  /**
   * Dựng lại heap
   */
  rebuildHeap() {
    const L = Math.floor(this.size / 2);
    for (let i = L - 1; i >= 0; i--) {
      this.maxHeapify(i);
    }
  }

  isHeap() {
    const L = Math.floor(this.size / 2);
    for (let i = L - 1; i >= 0; i++) {
      const l = this.data[left(i)] || Number.MIN_SAFE_INTEGER;
      const r = this.data[right(i)] || Number.MIN_SAFE_INTEGER;

      const max = Math.max(this.data[i], l, r);

      if (max !== this.data[i]) {
        return false;
      }
      return true;
    }
  }

  sort() {
    for (let i = this.size - 1; i > 0; i--) {
      swap(this.data, 0, i);
      this.size--;
      this.maxHeapify(0);
    }
  }

  insert(key) {
    this.data[this.size++] = key;
    if (this.isHeap()) {
      return;
    }
    this.rebuildHeap();
  }

  delete(index) {
    if (index >= this.size) {
      return;
    }
    this.data.splice(index, 1);
    this.size--;
    if (this.isHeap()) {
      return;
    }
    this.rebuildHeap();
  }

  /**
   * Mọi vị trí khác trong heap đều thỏa mãn tính chất
   * chỉ riêng node gốc, dựng lại tính chất heap
   * @param {*} i
   */
  maxHeapify(i) {
    let max = i;

    if (i >= this.size) {
      return;
    }

    // Tìm index lớn hơn trong 2 node trái/phải
    const l = left(i);
    const r = right(i);
    if (l < this.size && this.data[l] > this.data[max]) {
      max = l;
    }

    if (r < this.size && this.data[r] > this.data[max]) {
      max = r;
    }

    // Nếu node hiện tại đã lớn nhất, tức đã là max-heap
    if (max === i) {
      return;
    }

    swap(this.data, i, max);

    // Đệ quy chìm xuống tiếp
    return this.maxHeapify(max);
  }
}

module.exports = Heap;
```
