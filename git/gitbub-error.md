# GITHUB ERROR: PERMISSION TO USER/REPO DENIED TO USER/OTHER-REPO

---

* Problem on github CLI interface on Windows 10
* "Error: Permission to user/repo denied to user/other-repo"
* 이 오류는 push 하는 키가 배포 키로 다른 repository에 연결되어 있으며 push 하려는 repository에 접근 할 수 없음을 의미함.
* 이 문제는 다른 github 계정으로 push 하려고 할때 발생함.
* 다음 git config를 통해 사용자 이메일을 추가 할수 있음.
```
git config user.name "username"
# Set a new name
git config user.name
# Verify the setting
username


git config --global --unset-all
# remove user configs
```
* 또 다시 에러가 발생한다면 다음 순서를 따를것.
* 제어판 > 사용자 계정 > 자격 증명 관리자 > Windows 자격증명 > 일반 자격 증명
* Github에서 키를 제거.
