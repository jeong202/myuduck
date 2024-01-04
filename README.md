# myuduck
서울지역에 위치한 대극장의 뮤지컬 공연에 대한 정보를 볼 수 있는 사이트 입니다.대극장,뮤지컬,배우의 정보를 제공합니다.   
팀프로젝트로 진행되었으며, PHP를 이용하여 제작한 사이트입니다. DB는 MySQL을 사용하였습니다.   
카카오지도 API를 연동하였고 뮤지컬과 배우 찜기능을 구현했습니다.

---


### 메인페이지 :   
  데이터베이스(musical 테이블)에서 뮤지컬 정보를 가져오기 위한 SQL 쿼리를 실행하고 결과를 $musicalMainInfo 배열에 저장합니다.
  QAboard 테이블에서 정보를 가져오기 위해 또 다른 SQL 쿼리가 실행되고, 결과를 $QAInfo 배열에 저장합니다.   
  noticeboard 테이블에서 정보를 가져오기 위해 유사한 과정이 진행되고, 결과를 $noticeInfo 배열에 저장합니다.   
  슬라이더(최신뮤지컬소개), 개봉예정 뮤지컬 목록, 각 카테고리로 이동하는 링크, 최근 QA 게시물, 최근 공지사항 게시물 및 티켓 예약 사이트로 이동하는 링크 등이 포함되어 있습니다.   
  슬라이더를 위한 Swiper, 부드러운 스크롤 효과를 위한 Lenis가 사용되었습니다.   
  - header에는 로고, 카테고리 메뉴, 햄버거 메뉴가 있습니다. JavaScript 코드로 햄버거 메뉴 아이콘 클릭 시 어두운 배경을 생기며, 서브메뉴는 마우스 오버 시 슬라이드 업/다운 효과를 제공합니다.   

### 회원페이지   
  - 회원가입 : '이용약관 동의' -> '회원가입 양식 입력'- > '회원가입 완료'의 단계로 이루어져 있습니다.   
    - 이용약관 동의 :   
      사용자에게 이용약관 및 개인정보 수집 동의에 관한 체크박스를 보여주고, 해당 동의 내용이 표시되어 있습니다.   
      자바스크립트로 이용약관 동의" 체크박스(agreeCheck1)를 체크 또는 해제할 때, 나머지 체크박스(agreeCheck2, agreeCheck3, agreeCheck4)도 같이 체크 또는 해제되도록 하는 동작을 만들었습니다.   
      체크박스 상태 감지해 "이용약관 동의" 이외의 체크박스 중 하나라도 해제되면 "이용약관 동의" 체크박스도 해제되도록 하는 동작을 만들었습니다.   
      모든 동의 체크박스가 체크된 경우에만 다음 페이지(joinInsert.php)로 이동하도록 했습니다.   
      모든 동의 체크박스가 체크되지 않은 경우 경고창을 표시하고 이동하지 않습니다.

    - 회원가입 양식 입력 :   
      사용자가 정보를 입력하고 회원가입 완료(제출버튼)을 클릭하면 POST 방식으로 서버에 데이터를 제출합니다.   
      입력 필드로는 아이디,비밀번호,이름,주소,이메일,연락처가 있습니다.   
      아이디,이메일,비밀번호,이릅,이메일은 각각 입력조건이 있고 이를 자바스크립트로 확인해 사용자의 입력조건이 유효한지 여부를 검사하고, 문제가 있을 경우 메시지를 표시하여 사용자에게 알립니다. 이때 정규표현식을 활용하여 형식이 맞는지 확인하였습니다.   
      유효성 검사가 통과되면, 서버(joinCheck.php)로 아이디 및 이메일 중복 여부를 확인하는 AJAX 요청을 비동기적으로 전송합니다. joinCheck.php 파일은 데이터베이스와 연결하고, 입력된 아이디 또는 이메일이 이미 존재하는지 확인하는 역할을 합니다. 사용자로부터 받은 입력에 대해 real_escape_stri        을 사용하여 SQL 인젝션을 방지하고 있습니다.   
      서버는 전송받은 아이디 및 이메일을 확인하고, 결과를 JSON 형태로 응답합니다.   
      클라이언트는 서버의 응답을 받아서, 중복 여부에 따라 메시지를 표시합니다.   
      Daum 우편번호 API를 활용하여 주소 입력을 더욱 편리하게 만들었습니다.   
      모든 유효성 검사가 통과되면 사용자의 입력이 서버로 전송되고, 등록이 완료됩니다.   

    - 회원가입 완료 :   
      회원가입 폼에서 받아온 정보를 데이터베이스에 저장합니다. 폼에서 POST 방식으로 전송된 데이터를 받아 SQL 인젝션을 방지하기 위해 데이터를 안전하게 처리한 후 SQL 쿼리를 생성하여 myuduck 테이블에 새로운 회원 정보를 삽입합니다.   
      회원가입이 완료된 후에는 메인 페이지로 이동하는 링크가 제공됩니다.
      
   - 회원탈퇴 : '탈퇴약관 동의' -> '회원탈퇴 양식 입력'- > '회원탈퇴 완료'의 단계로 이루어져 있습니다.   
     - 탈퇴약관 동의 :   
       탈퇴약관동의 여부를 선택하는 체크박스와 확인, 취소 버튼이 제공됩니다.   
       사용자가 동의 버튼을 클릭하면, 체크박스의 상태를 확인하고 동의한 경우 joinCancel.php 페이지로 이동합니다. 사용자가 취소 버튼을 클릭하면 경고 메시지를 표시하고 메인 페이지로 이동합니다.   
     - 회원탈퇴 양식 입력 :   
       사용자가 확인 버튼을 클릭할 때, 입력한 비밀번호와 확인용 비밀번호를 비교하여 일치하면 폼을 제출하고, 불일치하면 경고 메시지를 표시합니다. 취소 버튼을 클릭하면 메인 페이지로 이동합니다.   
       joinCancelCheck.php에서 사용자가 입력한 비밀번호($youPass)와 현재 세션에 저장된 사용자의 ID($myuduckId)를 이용하여 회원 탈퇴 여부를 확인합니다.입력된 비밀번호와 사용자 ID를 이용하여 데이터베이스에서 해당 사용자의 정보를 조회한 후,   
       일치하면 해당 회원을 탈퇴 처리합니다. 회원 탈퇴가 성공하면 성공 메시지를 표시하고 메인 페이지로 이동합니다. 회원 탈퇴가 실패하면 실패 메시지를 표시하고 회원 탈퇴 페이지로 이동합니다.   
     - 회원탈퇴 완료 :   
       사용자에게 회원 탈퇴가 완료되었다는 안내와 함께, 메인 페이지로 이동할 수 있는 링크를 제공합니다.     
  - 아이디/비밀번호 찾기   
    - 아이디 찾기 :   
      사용자에게 이름과 이메일을 입력하고 아이디를 찾을 수 있는 폼 양식이 제공됩니다.   
      찾기 버튼을 누를 경우 findme_Id.php 파일로 데이터를 post방식으로 전송합니다.   
      자바스크립트를 사용하여 이름과 이메일이 비어 있으면 폼 제출을 막고 알림 메시지를 표시합니다. 그렇지 않으면 폼을 제출합니다.     
      사용자가 입력한 이름과 이메일로 아이디를 찾습니다. 데이터베이스에서 해당 정보와 일치하는 사용자를 조회하고, 일치하는 사용자가 없으면 알림 메시지를 표시하고 페이지를 이동합니다.   
      일치하는 사용자가 있으면 세션에 해당 아이디를 저장하고 결과 페이지로 이동합니다. 결과페이지에서 세션에 저장된 아이디를 표시하고 사용자에게 보여줍니다.   
    - 비밀번호 찾기 :   
      사용자가 입력한 이름, 아이디, 이메일로 비밀번호를 찾습니다. 이름, 아이디, 이메일이 비어 있으면 폼 제출을 막고 알림 메시지를 표시합니다. 그렇지 않으면 폼(find_Pass)을 제출합니다.   
      데이터베이스에서 해당 정보와 일치하는 사용자를 조회하고, 일치하는 사용자가 없으면 알림 메시지를 표시하고 페이지를 이동합니다. 일치하는 사용자가 있으면 세션에 해당 비밀번호를 저장하고 결과 페이지로 이동합니다.   
      결과페이지에서 세션에 저장된 비밀번호를 표시하고 사용자에게 보여줍니다.   
  - 로그인 :   
    youId (아이디), youPass (비밀번호)를 입력할 수 있는 input 요소와 로그인 버튼으로 구성되어 있습니다. 자바스크립트를 통해 로그인 버튼 클릭 시 아이디와 비밀번호를 확인하고, 조건에 따라 폼을 제출하거나 경고 메시지를 표시합니다.   
    폼 제출시 POST 메소드를 통해 아이디(youId)와 비밀번호(youPass)가 loginSave.php로 전송됩니다. MySQL 데이터베이스에서 입력받은 아이디와 비밀번호로 사용자를 찾는 쿼리를 수행한 후 쿼리 결과를 통해 로그인 성공 여부를 확인하고, 성공 시 세션에 사용자 정보를 저장하고 메인 페이지로 이동합니다.    
  - 회원정보수정(마이페이지)   
    - 프로필 :   
      아이디, 이름, 이메일은 세션에서 불러와 사용자 정보를 표시하고 있습니다.   
      회원정보수정을 클릭하면 회원정보수정 페이지로 이동합니다. 사용자에게 비밀번호, 이름, 주소, 전화번호를 업데이트할 수 있는 양식을 제공합니다.   
      자바스크립트를 통해 비밀번호 및 전화번호 필드에 대한 클라이언트 측 유효성을 수행합니다. 주소 검색은 Daum 우편번호 API를 활용합니다.   
      회원정보수정 버튼을 클릭하면 제출된 양식 데이터를 받아서 프로필을 업데이트합니다. myuduck 테이블과 QAcomment 테이블에서 비밀번호를 업데이트합니다.업데이트가 성공하면 세션 정보를 업데이트하고 메시지를 표시한 뒤, 사용자를 mypage.php로 리디렉션합니다   
 
    - 찜목록 :   
    현재 로그인한 사용자의 아이디를 세션에서 가져옵니다.   
    likeActor 테이블에서 사용자가 찜한 배우 목록을 조회합니다. 최신 3개의 목록을 가져오고, 결과를 배열에 저장합니다.   
    likeMusical 테이블과 theater 테이블을 조인하여 사용자가 찜한 뮤지컬 목록을 조회합니다. 이 역시 최신의 3개만 저장합니다.   
    조회한 찜 목록을 출력하고 찜 목록이 비어있는 경우에는 비어있다는 메시지를 출력합니다.   
    배우 찜 목록에서 더보기를 클릭하는 경우 찜목록 더보기 페이지로 이동합니다. 사용자의 세션 정보를 기반으로 데이터베이스에서 배우 찜 목록을 가져옵니다 likeStatus가 1이면서 사용자 아이디(youId)에 해당하는 경우입니다. 사용자가 찜한 배우의 상세 정보 페이지로 이동할 수 있는 링크도 제공합니다. 
    뮤지컬 찜 목록에서 더보기를 클릭하는 경우도 역시 비슷한 기능을 수행합니다. 사용자의 세션 정보를 기반으로 likeStatus가 1이면서 사용자 아이디(youId)에 해당하는 뮤지컬 찜 목록을 가져옵니다.사용자가 찜한 뮤지컬의 상세 정보 페이지로 이동할 수 있는 링크도 제공합니다.   
### 게시판
- 후기게시판 :   
  - 게시물 목록 :   
    데이터베이스에서 'QAboard' 테이블의 게시물 총 개수를 가져와 $boardTotalCount 변수에 저장합니다.   
    현재 페이지($page)를 URL의 쿼리스트링에서 가져오고, 만약 없으면 기본값으로 1을 사용합니다. 한 페이지에 보여질 게시물 수는 $viewNum으로 설정하고, 현재 페이지에 표시할 게시물 범위의 시작 위치를 계산하여 $viewLimit에 저장합니다.   
    $viewLimit은 현재 페이지에서 보여질 게시물의 시작 위치를 나타내게 됩니다.   
    테이블인 'QAboard'와 'myuduck'을 조인하고, 'youId'를 기준으로 매핑합니다. 그 후 'boardID'를 기준으로 내림차순으로 정렬한 뒤, $viewLimit에서부터 $viewNum 개수만큼의 게시물을 선택합니다. 이로써 특정 페이지에서 보여줄 범위의 게시물을 가져오게 됩니다.   
    각 게시물의 정보를 반복문을 통해 HTML로 출력합니다. 만약 결과가 없는 경우에는 "게시글이 없습니다." 메시지를 출력하고, 쿼리 수행 중 오류가 발생한 경우에는 오류 메시지를 출력합니다.
  - 게시물 :   
    - 이전글 및 다음글 :   
      QAboard 테이블에서 현재 글의 ID ($boardID)보다 작은 값 중에서 가장 큰 값을 가진 글을 이전글로 가져오고, 큰 값 중에서 가장 작은 값을 가진 글을 다음글로 가져옵니다.
      만약 이전 글(다음 글)이 존재하면 해당 글의 링크와 제목 일부를 표시하고, 그렇지 않으면 "이전글이 없습니다(다음글이 없습니다)."라는 문구를 표시합니다.   
      substr 함수를 사용해 게시글 제목을 일부분만 표시하기 위해 사용되었습니다. 구체적으로는 이전 글과 다음 글의 제목을 각각 최대 20자까지만 표시하기 위해 사용되었습니다.   
    - 댓글:   
      boardId = '$boardID'는 현재 글에 해당하는 댓글을 선택하고, commentDelete = '1'은 삭제되지 않은 댓글만을 선택합니다.   
    - 게시글 내용 :    
      QAboard 테이블에서 boardID가 현재 게시글의 ID와 일치하는 행을 찾아서 해당 행의 boardview 컬럼 값을 1 증가시키는 업데이트 쿼리를 생성해 실행합니다.   
      QAboard 테이블에서 특정 게시글의 정보를 myuduck 테이블과 조인하여 가져오고, 그 중에서도 특정 boardID에 해당하는 게시글의 정보만을 선택합니다.   
      QAboard 테이블에는 게시글 정보가, myuduck 테이블에는 사용자 정보가 저장되어 있습니다. 두 테이블을 youId를 기준으로 조인함으로써 각 게시글에 대한 등록자(사용자)의 정보를 함께 조회할 수 있습니다.   


- 공지사항

- 
- 검색페이지
- 뮤지컬 카테고리 페이지
- 극장 카테고리 페이지 
- 배우 카테고리 페이지


