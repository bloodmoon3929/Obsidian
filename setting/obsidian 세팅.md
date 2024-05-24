# 향후 계획
[chatGPT와의 연동](https://www.youtube.com/watch?v=heqhzKPBVHs&list=PL9eMAqlazwAjmkf7xKP94kChxVfc6tNJV&index=16)
[리뷰 템플릿 만들기](https://www.youtube.com/watch?v=Q2-aHQKEOHs&list=PL9eMAqlazwAjmkf7xKP94kChxVfc6tNJV&index=19)
- 여행지
- 음악
https://www.youtube.com/watch?v=yLwZk86LoCk
https://www.youtube.com/watch?v=uzXQGvMmPtQ
# 완료
### 파일 및 링크
모든 파일 확장 명 인식
새 첨부 파일을 만들 위치
- 아래에 지정된 폴더
- 첨부 파일 폴더 경로

# 커뮤니티 플러그인
## Advanced Tables
표 작성에 용의하도록 도와줌

Code Styler

Custom Frames

## Dataview
``안에 같은 js문법을 넣어 미리보기 화면에서 데이터들을 볼 수 있게 해줌
$=dv.list(dv.pages('').sort(f=>f.file.mtime.ts,"desc").limit(5).file.link)


Excaildraw

## Execute Code
```
```
에서 기존에 cpp를 적어준다면, cpp문법을 인식하였는데, 그것에 덧붙여 runcpp라고 입력하면 run 버튼이 생겨 코드의 작동상태를 확인할 수 있느나 ciling을 아직 설치하지 못해 사용할 수 없음

## Git
ctrl+p를 눌려 명령어 창을 사용할 수 있는데, 
Git: commit all changes
Git: Push
를 사용하여 github에 동기화 할 수있다. 모바일로는 구현이 불가하여 현재는 onedrive를 사용중임으로 자동적으로 커밋하지 않고 수동으로만 사용중이다.

## Homepage
home.md 파일을 홈페이지로 사용하기 위해 사용

## Iconize
폴더나 파일에 아이콘을 사용하기 쉽게 하기 위해 사용
: 와 영단어를 입력하면 아이콘이 나옴


## KoreanBook Info plugin
책의 정보를 교보문구 기준으로 파싱해서 저장하는 플러그인
사용방법은 md파일의 이름을 원하는 책 이름으로 하고, 왼쪽의 작대기 3개인 add book info를 누름

## templater
템플릿을 사용하기 위해 사용, 탬플릿 파일에 미리 만들어 놓은 템플릿 사용
alt + e 를 눌려, 템플릿을 적용함

<% %>를 사용하여 템플릿에 적용할때, 자동으로 기입하게 할 수 있음
ex) 
<% tp.system.suggester(["★★★★★", "★★★★", "★★★", "★★", "★"],["★★★★★", "★★★★", "★★★", "★★", "★"]) %>
별점 선택

<% tp.file.creation_date("YYYY년-MM월-DD일 HH시:mm분") %>
파일 생성 날짜

<% tp.file.folder() %>
폴더명

<% tp.file.title.split("/")[0] %>
md파일 제목 (.spilt으로 나눈것이 배열에 들어간다 생각하며 [index])


## VSCode Editor
vscode를 키지 않고, cpp파일을 열어볼 수 있음

[대시보드 홈페이지 만들기](https://www.youtube.com/watch?v=JknCCe1k9UY&list=PL9eMAqlazwAjmkf7xKP94kChxVfc6tNJV&index=18)