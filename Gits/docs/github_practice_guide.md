# GitHub 연계 및 협업 실습 가이드

이 문서는 GitHub을 활용한 다수 참여자(User1, User2) 간의 협업 환경을 시뮬레이션하고, 소스 충돌 해결 및 강제 명령(Force Push/Pull) 사용법을 정리한 실습 가이드입니다.

---

## 1. GitHub 원격 저장소 연계
**목표:** 로컬 저장소를 GitHub 원격 저장소와 연결합니다.

### 1.1 원격 저장소 등록 (Remote Add)
```bash
# GitHub에서 생성한 저장소 주소를 연결합니다.
$ git remote add origin https://github.com/[YOUR_ACCOUNT]/[YOUR_REPO].git

# 연결 확인
$ git remote -v
```

### 1.2 첫 푸시 (Push)
```bash
# 로컬의 master 브랜치를 원격의 origin으로 전송합니다.
$ git push -u origin master
```

---

## 2. 참여자 간 협업 실습 (User1 & User2)
**시나리오:** User1과 User2가 동일한 저장소에서 작업하는 환경을 시뮬레이션합니다.

### 2.1 User2의 환경 설정 (Clone)
```bash
# 다른 폴더로 이동하여 User2의 입장에서 저장소를 복제합니다.
$ git clone https://github.com/[YOUR_ACCOUNT]/[YOUR_REPO].git user2_project
```

### 2.2 기본적인 Push & Pull 흐름
- **User2:** 코드 수정 후 Push
  ```bash
  $ echo "// User2 작업" >> calculator.cpp
  $ git add .
  $ git commit -m "feat: user2 update"
  $ git push origin master
  ```
- **User1:** 최신 내용 반영 (Pull)
  ```bash
  $ git pull origin master
  ```

---

## 3. 소스 충돌(Conflict)과 대처 방법
**목표:** 동일한 파일의 같은 라인을 수정하여 충돌을 발생시키고 해결합니다.

### 3.1 충돌 발생 상황 재현
1. **User1:** `calculator.cpp` 1번 라인을 "// User1 수정"으로 변경 후 커밋만 수행 (Push 안 함)
2. **User2:** `calculator.cpp` 1번 라인을 "// User2 수정"으로 변경 후 커밋 및 **Push 완료**
3. **User1:** 자신의 작업을 Push 하려 하면 거부됨 -> Pull 시도 -> **충돌 발생!**

### 3.2 충돌 해결 과정
```bash
# 1. Pull 시도 시 충돌 메시지 확인
$ git pull origin master
# CONFLICT (content): Merge conflict in calculator.cpp

# 2. 파일 수정 (<<<<, ====, >>>> 표시 부분 정리)
# User1과 User2의 작업 중 남길 코드를 선택하여 편집합니다.

# 3. 스테이징 및 완료 커밋
$ git add calculator.cpp
$ git commit -m "fix: resolve conflict between user1 and user2"
$ git push origin master
```

---

## 4. 복구 불가 시 강제 처리 (Force Commands)
**주의:** 강제 명령은 협업자들의 히스토리를 망가뜨릴 수 있으므로 최후의 수단으로만 사용합니다.

### 4.1 원격 내용을 로컬로 강제 덮어쓰기 (Force Pull)
로컬 작업이 꼬여서 원격 상태와 완전히 동일하게 맞추고 싶을 때 사용합니다.
```bash
# 원격의 최신 정보를 가져옵니다.
$ git fetch --all

# 로컬 master 브랜치를 원격 origin/master 상태로 강제 리셋합니다.
$ git reset --hard origin/master
```

### 4.2 원격 저장소 강제 덮어쓰기 (Force Push)
로컬의 특정 시점으로 원격을 되돌려야 하거나, Rebase 후 강제로 반영해야 할 때 사용합니다.
```bash
# 로컬의 현재 상태를 원격에 강제로 집어넣습니다. (기존 원격 커밋 무시)
$ git push origin master --force
# 또는
$ git push origin master -f
```

---

## 5. 실습 체크리스트
- [ ] `git remote add`를 통해 GitHub 연결이 되었는가?
- [ ] User2 입장에서 `git clone`을 성공했는가?
- [ ] `git pull`을 통해 상대방의 변경 사항을 가져왔는가?
- [ ] 충돌 발생 시 `<<<<`, `>>>>` 기호를 지우고 코드를 병합했는가?
- [ ] `git reset --hard`의 위험성을 이해했는가?
