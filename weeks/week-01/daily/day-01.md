# Week 01 — Day 01: Stack vs Heap, Stack Frames, and Address Space

## 1) Today’s Tasks (2–3 tasks)
- [ ] Task 1: 스택 프레임을 gdb로 직접 확인한다 (주소/레지스터/리턴주소).
- [ ] Task 2: `/proc/<pid>/maps`로 힙/스택/주소공간을 확인한다.
- [ ] Task 3 (optional): 캐시 감각 벤치(순차 vs stride 접근)로 “데이터 이동 비용” 맛보기.

---

## 2) Concept Focus
- **CPU 실행 흐름**
  - 명령어 포인터(IP/RIP)가 다음에 실행할 명령 주소를 가리킴
  - 함수 호출은 “다음으로 돌아갈 주소(리턴 주소)”가 필요
- **스택(Stack)**
  - 함수 호출/리턴, 지역변수 수명에 최적
  - 보통 높은 주소에서 낮은 주소로 자람 (RSP가 감소)
- **힙(Heap)**
  - 동적 할당(malloc/new), 수명은 프로그래머/런타임이 관리
- **가상 메모리(Address Space)**
  - 프로세스마다 “자기만의 지도(주소공간)”가 있고 OS가 페이지 매핑/보호

### Stack frame ASCII (내가 이해한 모델)
```text
높은 주소
+------------------+
| caller locals    |
+------------------+
| return address   |  <- call이 push하는 값(개념적으로)
+------------------+
| saved FP (RBP)   |
+------------------+
| callee locals    |  <- RSP가 내려가며 공간 확보
+------------------+
낮은 주소
```

---

## 3) References (오늘 읽을 것)
- **Book:** CSAPP Ch.3 (procedures / stack frame 관련 섹션)
- **Book:** OSTEP (Address Spaces / Virtualization 관련 섹션)
- **Official Docs:** GDB docs (stack frames / backtrace), Linux procfs docs (`/proc/[pid]/maps`)
- **Blog/Article:** x86-64 calling convention + stack frame(그림 있는 글), `/proc/pid/maps` 읽는 법
- **Optional Video:** x86-64 stack frame 설명 10~20분 영상

---

## 4) Implementation

### Task 1 — Stack Frame 실험 (C + gdb)

#### 4.1) Code: `labs/memory/day1_stack.c`
```c
#include <stdio.h>

__attribute__((noinline))
int add3(int a, int b, int c) {
    int x = a + 1;
    int y = b + 2;
    int z = c + 3;
    printf("&a=%p &b=%p &c=%p &x=%p &y=%p &z=%p\n",
           (void*)&a, (void*)&b, (void*)&c, (void*)&x, (void*)&y, (void*)&z);
    return x + y + z;
}

int main() {
    int r = add3(10, 20, 30);
    printf("r=%d\n", r);
    return 0;
}
```

#### 4.2) Build/Run
```bash
gcc -O0 -g -fno-omit-frame-pointer labs/memory/day1_stack.c -o labs/memory/day1_stack
./labs/memory/day1_stack | tee artifacts/logs/day01_stack_run.txt
```

#### 4.3) Debug (gdb)
```bash
gdb ./labs/memory/day1_stack
(gdb) break add3
(gdb) run
(gdb) info registers
(gdb) bt
(gdb) info frame
(gdb) x/32gx $rsp
(gdb) p &a
(gdb) p &x
```

관찰 포인트:
- `&a`, `&b`, `&c`, `&x`, `&y`, `&z` 주소가 서로 가깝게 나오나?
- `$rsp` 주변에 리턴 주소로 보이는 값이 있나?
- `bt`에서 call stack이 어떻게 보이나?

### Task 2 — Heap/Address Space 실험 (`/proc/<pid>/maps`)

#### 4.4) Code: `labs/memory/day1_heap.c`
```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main() {
    int local = 123;
    void *p = malloc(4096 * 10); // 40KB
    printf("pid=%d\n", getpid());
    printf("&local=%p (stack)\n", (void*)&local);
    printf("p=%p (heap-ish)\n", p);

    printf("Press Enter to continue...\n");
    getchar();

    free(p);
    return 0;
}
```

#### 4.5) Build/Run
```bash
gcc -O0 -g labs/memory/day1_heap.c -o labs/memory/day1_heap
./labs/memory/day1_heap
```

#### 4.6) `maps` 확인
프로그램 출력에서 pid를 확인하고:

```bash
cat /proc/<pid>/maps | tee artifacts/logs/day01_maps_full.txt
grep -nE "\[heap\]|\[stack\]" /proc/<pid>/maps | tee artifacts/logs/day01_maps_grep.txt
```

관찰 포인트:
- `[heap]`, `[stack]` 라벨이 보이나?
- `&local`은 보통 높은 주소대(스택 근처)인가?
- `p`는 `[heap]` 근처 주소인가? (항상 100%는 아닐 수 있음)

## 5) Artifacts (Evidence)
- `artifacts/logs/day01_stack_run.txt`
- gdb 결과를 아래 중 하나로 남기기:
  - `artifacts/logs/day01_stack_gdb.txt` (gdb 출력 복붙)
  - `artifacts/screenshots/day01_stack_gdb_*.png`
- `artifacts/logs/day01_maps_full.txt`
- `artifacts/logs/day01_maps_grep.txt`
- `artifacts/diagrams/day01_stack_frame.md` (내 그림/요약)

## 6) Results / Notes (작성란)
Observations:

What surprised me:

What I still don’t understand:

## 7) Quiz (1 question)
Q: 함수 호출에서 “리턴 주소”는 왜 필요하고, 보통 어디에 저장될까?
A: (내 답)

## 8) Reflection (1 question)
오늘 본 주소들(`&local`, `p`, `$rsp` 근처 값)을 보고, 내가 갖고 있던 스택/힙 이미지와 달랐던 점이 있었나? 왜 그럴까?

---

## Bonus) `artifacts/diagrams/day01_stack_frame.md` 템플릿
원하면 이것도 바로 만들어서 “내 말로 정리”할 수 있게 해줄게:

```md
# Day 01 — Stack Frame Diagram (My Understanding)

## What happened when add3() was called
-
-
-

## Stack frame sketch
~~~text
높은 주소
+------------------+
|                  |
+------------------+
|                  |
+------------------+
낮은 주소
~~~

## Key registers (my words)
- RIP:
- RSP:
- RBP:

## One sentence takeaway
-
```
