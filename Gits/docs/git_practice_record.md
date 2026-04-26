# Git 기초 및 활용 마스터 과정 실습 기록

이 문서는 `docs/GitMasterCourseSlides.tex.pdf` 강의 자료의 실습 내용을 순차적으로 실행한 결과입니다.

## 1. 저장소 초기화 및 상태 확인
**명령어 설명:** 일반 폴더를 Git 저장소로 변환하고 상태를 확인합니다.

```bash
$ git init
$ git status
```

**실행 결과:**
```text
Initialized empty Git repository in /apps/test_cpp_project/.git/
On branch master

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

---

## 2. 스테이징과 커밋
**연습 문제:** `calculator.cpp` 파일을 생성하고 첫 번째 커밋을 생성합니다.

```bash
$ echo '#include <iostream>

int add(int a, int b) {
    return a + b;
}

int main() {
    std::cout << "Calculator" << std::endl;
    return 0;
}' > calculator.cpp
$ git add .
$ git commit -m "feat: 덧셈 기능 모듈 추가 "
```

**실행 결과:**
```text
[master (root-commit) fa17d7e] feat: 덧셈 기능 모듈 추가
 1 file changed, 11 insertions(+)
 create mode 100644 calculator.cpp
```

---

## 3. 로그 확인 및 변경 사항 취소
**연습 문제:** 히스토리를 확인하고 의도적으로 코드를 수정한 뒤 복원합니다.

```bash
$ git log --oneline
$ sed -i 's/a + b/a * b/' calculator.cpp
$ git diff
$ git restore calculator.cpp
```

**실행 결과:**
```text
fa17d7e feat: 덧셈 기능 모듈 추가
diff --git a/calculator.cpp b/calculator.cpp
index 1cb0abb..c3e2f6d 100644
--- a/calculator.cpp
+++ b/calculator.cpp
@@ -3,7 +3,7 @@
 #include <iostream>
 
 int add(int a, int b) {
-    return a + b;
+    return a * b;
 }
```

---

## 4. 브랜치 생성 및 이동
**연습 문제:** `feature/divide` 브랜치를 생성하여 나눗셈 기능을 추가합니다.

```bash
$ git branch
$ git switch -c feature/divide
$ echo 'int divide(int a, int b) { return a / b; }' >> calculator.cpp
$ git add .
$ git commit -m "feat: 나눗셈 기능 추가"
$ git switch master
```

**실행 결과:**
```text
* master
Switched to a new branch 'feature/divide'
[feature/divide 90249e6] feat: 나눗셈 기능 추가
 1 file changed, 1 insertion(+)
Switched to branch 'master'
```

---

## 5. 병합(Merge)과 충돌(Conflict) 해결
**연습 문제:** `master`와 `feature/divide`에서 동일한 위치를 수정하여 충돌을 유발하고 해결합니다.

```bash
# master 브랜치 수정
$ sed -i '1i // 버전 1.0' calculator.cpp
$ git add .
$ git commit -m "update: master version"

# feature/divide 브랜치 수정
$ git switch feature/divide
$ sed -i '1i // 나눗셈 버전' calculator.cpp
$ git add .
$ git commit -m "update: feature version"

# 병합 시도 및 충돌 발생
$ git switch master
$ git merge feature/divide
```

**충돌 해결 및 완료:**
```bash
# calculator.cpp 수동 수정 후
$ git add .
$ git commit -m "fix: 병합 충돌 해결 "
$ git log --oneline --graph --all
```

**실행 결과 (그래프):**
```text
*   b055fde (HEAD -> master) fix: 병합 충돌 해결
|\  
| * 9c54997 (feature/divide) update: feature version
| * 90249e6 feat: 나눗셈 기능 추가
* | 90b0c62 update: master version
|/  
* fa17d7e feat: 덧셈 기능 모듈 추가
```

---

## 6. 기타 도구 (Stash & Rebase)
**명령어 설명:** 작업을 임시 보관하거나 히스토리를 재배치합니다.

### Stash 테스트
```bash
$ echo "// 임시 작업" >> calculator.cpp
$ git status
$ git stash
$ git stash pop
```

### Rebase 테스트
```bash
$ git checkout -b feature/rebase_test
$ echo "// rebase 작업" >> calculator.cpp
$ git add .
$ git commit -m "feat: rebase test branch commit"
$ git checkout master
$ echo "// master 새 작업" >> calculator.cpp
$ git add .
$ git commit -m "update: master new work"
$ git checkout feature/rebase_test
$ git rebase master
```

**최종 로그 그래프:**
```text
* 593342c (HEAD -> feature/rebase_test) feat: rebase test branch commit
* d4de5b9 (master) update: master new work
*   b055fde fix: 병합 충돌 해결
|\  
| * 9c54997 (feature/divide) update: feature version
| * 90249e6 feat: 나눗셈 기능 추가
* | 90b0c62 update: master version
|/  
* fa17d7e feat: 덧셈 기능 모듈 추가
```
