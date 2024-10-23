---
longform:
  format: scenes
  title: SIMD
  sceneFolder: /
  scenes: []
  ignoredFiles: []
---
In C++, the registers are exposed as variables of the 6 fundamental types.
![[Pasted image 20241023153937.png]]

Assembly only knows about 2 vector data types, 16-bytes registers and 32-bytes ones. There’re no
separate set of 16/32 byte registers, 16-byte ones are lower halves of the corresponding 32-bytes.
But they are different in C++, and there’re intrinsics to re-interpret them to different types. In the
table below, rows correspond to source types, and columns represent destination type.
![[Pasted image 20241023155028.png]]

有一些指令用于正确的类型转换，它们仅支持有符号32位整数通道：
- `_mm_cvtepi32_ps` 将4个32位整数通道转换为32位浮点数。
- `_mm_cvtepi32_pd` 将前两个整数转换为64位浮点数。
- `_mm256_cvtpd_epi32` 将4个双精度浮点数转换为4个整数，并将输出的高4个通道设置为零。
```
#include <emmintrin.h>

int main() {
    __m128i int_vector = _mm_setr_epi32(1, 2, 3, 4); // 初始化整数向量
    __m128 float_vector = _mm_cvtepi32_ps(int_vector); // 转换为浮点数向量
    return 0;
}

```
```
#include <emmintrin.h>

int main() {
    __m128i int_vector = _mm_setr_epi32(1, 2, 3, 4); // 初始化整数向量
    __m128d double_vector = _mm_cvtepi32_pd(int_vector); // 转换为双精度浮点数向量
    return 0;
}

```
```
#include <immintrin.h>

int main() {
    __m256d double_vector = _mm256_setr_pd(1.0, 2.0, 3.0, 4.0); // 初始化双精度浮点数向量
    __m256i int_vector = _mm256_cvtpd_epi32(double_vector); // 转换为整数向量
    return 0;
}

```
##### Initializing Vector Registers
###### Initializing with Zeros
```
\\初始化一个 `__m128` 类型的向量，将其所有四个单精度浮点数设置为零。
__m128 _mm_setzero_ps(void); 

\\初始化一个 `__m256i` 类型的向量，将其所有 256 位整数设置为零。
__m256i _mm256_setzero_si256(void);

```

```
#include <emmintrin.h> // 包含 SSE 指令集的头文件

int main() {
    // 初始化一个 __m128 向量为零
    __m128 zero_vector = _mm_setzero_ps();
    return 0;
}

```
```
#include <immintrin.h> // 包含 AVX 指令集的头文件

int main() {
    // 初始化一个 __m256i 向量为零
    __m256i zero_vector = _mm256_setzero_si256();
    return 0;
}

```
###### Initializing with Values








































































